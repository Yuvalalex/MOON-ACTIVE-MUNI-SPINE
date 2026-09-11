<!-- מקור: Master_PRD.md | פרק 23 מתוך 25 -->

> **שם הפרק:** חוזי API ו-Reconciliation

> ניווט: [פרק 22](<22 - מודל אבטחה, פרטיות והרשאות.md>) | [README](README.md) | [פרק 24](<24 - מדידה, ניסויים ו-Observability.md>)

## 23. חוזי API ו-Reconciliation

- הפרק מגדיר את קווי החוזה בין לקוח לשרת ואת כללי המיזוג הבטוח לאחר reconnect.

### 23.1 מפרט חוזי API (OpenAPI 3.0 Specification)

כל התקשורת מתבצעת מעל HTTPS (TLS 1.3 בלבד) עם הצפנת Payload וחתימת מכשיר חומרתית.

#### 1. בקשת הנפקת חכירה: `POST /v1/offline/lease`
נקראת באופן שקוף ברקע בעת התחברות תקינה לרשת ומכינה את המכשיר למקרה של ניתוק:
```json
// Request Body
{
  "client_version": "3.52.1",
  "device_id": "d_ios_88192a_p256",
  "attestation_token": "eyJh...app_attest_blob",
  "village_level": 142,
  "client_boot_uptime_ms": 52910400
}

// Response (200 OK)
{
  "lease_token": "eyJhbGciOiJFZERTQSI...",
  "session_id": "lease_sess_881920",
  "spin_quota": 50,
  "expires_at_utc": 1773100800,
  "outcome_commitment": "sha256:d8a9f2e34b...",
  "ghost_villages": [
    { "id": "ghost_vlg_101", "name": "Viking_Thor_Bot", "net_worth": 45000000 },
    { "id": "ghost_vlg_102", "name": "Shield_Maiden_NPC", "net_worth": 48000000 }
  ],
  "liveops_snapshot": {
    "active_event_id": "viking_quest_w12",
    "end_time_utc": 1773097200
  }
}
```

#### 2. שידור חבילת פעולות מנותקת: `POST /v1/offline/replay`
נשלחת בעת חידוש הקשר לרשת. כוללת מפתח Idempotency למניעת חיוב כפול:
```json
// Request Headers:
// X-Idempotency-Key: idemp_sess_881920_try1
// X-Client-Signature: sig_ed25519_client_payload

// Request Body
{
  "session_id": "lease_sess_881920",
  "lease_token": "eyJhbGciOiJFZERTQSI...",
  "initial_hash": "0000000000000000000000000000000000000000000000000000000000000000",
  "actions": [
    {
      "action_id": "act_01",
      "action_type": "SPIN",
      "bet_multiplier": 3,
      "outcome": "COINS_WIN",
      "coins_earned": 1500000,
      "local_timestamp": 1773061205,
      "nonce": "n_88192_01",
      "action_hash": "a4f91b7d82..."
    },
    {
      "action_id": "act_02",
      "action_type": "SHIELD_WIN",
      "bet_multiplier": 1,
      "outcome": "SHIELD",
      "coins_earned": 0,
      "local_timestamp": 1773061218,
      "nonce": "n_88192_02",
      "action_hash": "c7e2210a44..."
    }
  ],
  "final_hash": "9f82b01c3e...",
  "deferred_iap_intents": []
}

// Response (200 OK - Reconciled)
{
  "reconciliation_id": "rec_99218204",
  "status": "ACCEPTED",
  "total_coins_credited": 1500000,
  "shields_added": 1,
  "liveops_points_merged": 45,
  "vault_unlock_animation": true
}
```

#### 3. קטלוג שגיאות רשמי (Error Taxonomy)
| HTTP Status | קוד שגיאה (Error Code) | משמעות עסקית וטכנית | פעולת הלקוח (Client Action) |
| :---: | :--- | :--- | :--- |
| **400** | `HASH_CHAIN_BROKEN` | שרשרת הגיבוב נשברה (חשד לעריכת זיכרון מקומית) | ביטול הסשן המקומי, הצגת שגיאת סנכרון רגועה, ושליחת התראה ל-SIEM |
| **403** | `LEASE_EXPIRED` | עברו מעל 12 שעות ממועד הנפקת ה-Token | אי-זיכוי הפעולות שבוצעו לאחר מועד הפקיעה; זיכוי פעולות שבוצעו בזמן |
| **409** | `IDEMPOTENCY_CONFLICT` | אותה חבילה נשלחה פעמיים בגלל בעיית רשת | החזרת התשובה הקודמת (Cached 200 OK) ללא רישום כפול ב-Ledger |
| **422** | `OUTCOME_COMMITMENT_MISMATCH` | תוצאת ספין שנרשמה אינה תואמת ל-PRNG של השרת | פסילה מיידית של הסשן עקב ניסיון זיוף (Cheat Attempt) |
| **429** | `RATE_LIMIT_EXCEEDED` | שידור תכוף מדי של בקשות Replay | הפעלת Exponential Backoff והמתנה אקראית של 3-7 שניות |

---

### 23.2 מטריצת יישוב קונפליקטים (Conflict Resolution & Reconciliation Rules)

בעת סנכרון חבילת אופליין, עשויות להיווצר התנגשויות בין מצב המשחק החי לבין הפעולות שבוצעו במכשיר המנותק:

| תרחיש קונפליקט | לוגיקת הכרעה של השרת (Server Authority Rule) | השפעה על יתרת השחקן |
| :--- | :--- | :--- |
| **השחקן הותקף בזמן שהותו באופליין** | מגן שהושג באופליין מופעל רטרואקטיבית: אם השחקן זכה במגן בספין מס' 4, והותקף לאחר מכן – התקיפה נבלמת. | הכפר מוגן, והתוקף מקבל חיווי "Blocked by Shield". |
| **ניתוק רשת באמצע שידור Replay** | הלקוח שולח את אותה חבילה שוב עם אותו `X-Idempotency-Key`. השרת מזהה את המפתח ומחזיר את תוצאת האימות שכבר בוצעה. | אפס סיכון לזיכוי כפול של מטבעות. |
| **אירוע LiveOps הסתיים בטיסה** | פעולות שבוצעו לפני שעת הסיום הרשמית מזכות בנקודות; פעולות שבוצעו לאחר מכן מזכות במטבעות רגילים בלבד. | פרסי מדרגות מועברים ל-Player Inbox. |
| **אימות חלק מתוך ה-Action Log נכשל** | השרת אינו מוחק הכל באופן עיוור: כל עוד השרשרת תקינה עד ספין מסוים (למשל ספין 20), ספינים 1-20 מזוכים, וספינים 21-50 נפסלים. | תגמול הוגן על החלק המאומת, ופתיחת חקירת Fraud. |
| **שחקן התחבר ממכשיר שני במקביל** | ה-`device_binding` נועל את ה-Lease למכשיר המקורי. המכשיר השני מודיע: *"קיים סשן מנותק פעיל במכשיר אחר. סנכרן אותו תחילה"*. | מניעת Race Condition של כפל ספינים. |


---

