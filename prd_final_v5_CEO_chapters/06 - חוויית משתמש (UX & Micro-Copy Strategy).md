<!-- מקור: Master_PRD.md | פרק 6 מתוך 25 -->

> **שם הפרק:** חוויית משתמש (UX & Micro-Copy Strategy)

> ניווט: [פרק 05](<05 - גבולות גזרה קשיחים - מה אנחנו במכוון לא עושים (Explicit Non-Goals & Scope Boundaries).md>) | [README](README.md) | [פרק 07](<07 - דרישות פונקציונליות וארכיטקטורת Leased Session (MoSCoW).md>)

## 6. חוויית משתמש (UX & Micro-Copy Strategy)

- עיקרון UX: המעבר לאופליין חייב להיות שקט, לא מאיים ולא לשבור Flow.
- החזרה לרשת הופכת לרגע חיובי של תגמול, שקיפות ואפשרות Upsell.

> [!TIP]
> **השורה התחתונה:** המעבר לאופליין חייב להיות שקוף לחלוטין ולשמור על ה-Flow State. אין חסימות, אין פופ-אפים מפחידים.
### 6.1 אינדיקטור שקט (Ambient Offline Indicator)

- המטרה: ליידע שהסשן מוגן בלי להטריד את השחקן בפופ-אפים.

![Offline Concept](assets/pic/offline_indicator_concept.jpg)

<details>
<summary><b>📖 לחץ להרחבת פרטי ה-UX המלאים</b></summary>

* **ללא פופ-אפ חוסם:** כשהרשת מתנתקת, לא קופצת שום התראה מבהילה.
* **הסמן הוויזואלי:** כנפי זהב זעירות (Golden Wings) מופיעות מעל כפתור ה-SPIN עם כיתוב מוזהב מעודן: `OFFLINE: 85 SPINS LEFT`. השחקן מבין מיד שהוא מוגן ושהסשן פעיל.

<p align="center">
<img src="assets/pic/image1.png" alt="אינדיקטור אופליין בממשק Coin Master" width="300" />
</p>
</details>

---

### 6.2 חגיגת החזרה לרשת: אנימציית פתיחת הכספת (The Reconnect Touchpoint)

- רגע החזרה לרשת ממותג כנקודת שיא חיובית (Peak-End Rule) שמחזקת תחושת סיפוק, הישג ונכונות למונטיזציה.
- המעבר אינו מסתכם בסנכרון טכני אילם, אלא באירוע דופמין ויזואלי:

![Coin Master Vault Reconnect Celebration](assets/pic/coinmaster_vault_reconnect.jpg)
*איור: אנימציית פתיחת הכספת ברגע החזרה לרשת (The Reconnect Touchpoint) – אפקט פיצוץ המטבעות, שחרור המגנים ופריקת השלל*

![Vault Concept](assets/pic/vault_opening_concept.jpg)
*מפרט קונספט ויזואלי למסך פתיחת הכספת ומאזן השלל המצטבר*

![Reconnect Touchpoint Progression](assets/pic/image-3.png)
*שלבי המעבר החווייתי: מזיהוי חיבור הרשת, דרך אנימציית המנעולים ועד להפקדת המטבעות לחשבון השחקן*

* **הודעת חגיגה מונפשת:** *"Offline Vault Unlocked!"* – פיצוץ זהב, אבני חן ומטבעות שנאגרו בסשן המנותק.
* **המרת ספינים ורכישות:** פריקת המטבעות לתוך המאזן הראשי עם צלילי ג'קפוט ואפקט Haptic עשיר.
* **טריגר מונטיזציה מיידי (Loss-Aversion Retargeting):** הופעת הצעת מבצע מוגבלת בזמן (*"היית מדהים בטיסה! המשך את הרצף עם חבילת 250 ספינים ב-50% הנחה"*).

---

### 6.3 מסך Soft-Block בסיום המכסה: מיקרו-קופי אמפתי ורב-לשוני

כאשר מכסת הספינים המנותקת מגיעה לסיומה (50 ספינים בסיס / 100 VIP), הממשק אינו מציג מסך שגיאה אדום ומאיים, אלא מסך **Soft-Block מכבד ואמפתי** המעודד שהייה חיובית באפליקציה:

![Soft Block Concept](assets/pic/coinmaster_soft_block_ui.jpg)
*מסך ה-Soft-Block המרגיע: הגנה על הכספת, הפניה לאלבומי קלפים ואינדיקטור סנכרון אוטומטי*

![Soft Block Flow Context](assets/pic/image-4.png)
*התנהגות הממשק וזרימת הניווט בעת הגעה לקצה מכסת האופליין*

#### מפרט מיקרו-קופי גלובלי (Global Micro-Copy Specification - 4 Languages)

| אלמנט ממשק | English (Global Default) | עברית (Hebrew) | Español (LATAM/ES) | Deutsch (DACH) | רציונל התנהגותי (Behavioral UX) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **כותרת ראשית** | `ALL OFFLINE SPINS USED!` | `כל הספינים באופליין נוצלו!` | `¡TODAS LAS TIRADAS USADAS!` | `ALLE OFFLINE-SPINS GENUTZT!` | בהירות עובדתית מלאה ללא האשמת המשתמש. |
| **גוף ההודעה** | `Your vault is safe! Connect to the internet to recharge spins, or enjoy browsing your Card Collections.` | `הכספת שלך מאובטחת! התחבר לאינטרנט כדי לטעון ספינים חדשים, או המשך ליהנות מאלבומי הקלפים שלך.` | `¡Tu caja fuerte está a salvo! Conéctate a internet para recargar tiradas o explora tus colecciones de cartas.` | `Dein Tresor ist sicher! Verbinde dich mit dem Internet, um neue Spins zu laden, oder stöbere in deinen Kartensets.` | הרגעת חרדת אובדן משאבים (Loss Aversion) והפניה לפעילות מרגיעה ובלתי מנותקת. |
| **כפתור פעולה ראשי** | `[ VIEW CARD ALBUMS ]` | `[ צפה באלבומי הקלפים ]` | `[ VER COLECCIÓN DE CARTAS ]` | `[ KARTENSETS ANSEHEN ]` | כפתור חיובי המונע תסכול ומאפשר המשך שהייה חיובית באפליקציה. |
| **טקסט עזר תחתון** | `Reconnecting automatically when signal returns...` | `מתחבר אוטומטית כשהקליטה תחזור...` | `Reconectando automáticamente al recuperar la señal...` | `Automatische Wiederverbindung bei Netzempfang...` | הסרת הצורך בלחיצות רענון ידניות מציקות מצד השחקן. |

#### מפרט נגישות ומשוב תחושתי (Accessibility & Haptic Profiles)
- **Screen Reader / TalkBack / VoiceOver:** לכל רכיבי המסך מוגדרים תגי `accessibilityLabel` ברורים (לדוגמה: *"Offline mode active, 32 spins remaining in secure vault"*).
- **פרופיל Haptic (iOS CoreHaptics / Android VibrationEffect):**
  - כניסה למצב מנותק: פעימה קלה עמומה כפולה (Double Subtle Pulse - 40ms, 60ms gap, 40ms).
  - ביצוע ספין באופליין: נקישה חדה וקצרה (Impact Light - 25ms).
  - פתיחת הכספת בחיבור מחדש: רצף ויברציות מתגברות דמויות ג'קפוט (Crescendo Notification Pattern - 150ms).


---


> [!IMPORTANT]
> **מדוע הגבלנו את הסשן ל-12 שעות?**
> 1. **סייבר:** חלון צר מצמצם דרסטית את משטח התקיפה למניפולציות.
> 2. **זיכרון:** מונע התנפחות של Action Log שעלולה לקרוס במכשירי אנדרואיד חלשים.
> 3. **Reconciliation:** מונע "State Drift" אגרסיבי של נתונים מול השרת.
> 4. **סטטיסטיקה:** הסיכוי ששחקן סלולרי לא יהיה בקרבת שום רשת מעל 12 שעות ברציפות שואף לאפס.
> 5. **התרחבות מדורגת:** בשלב הראשון, הפיצ'ר יוגדר לתמוך רק ב"חורי קליטה נקודתיים" (דקות ספורות של ניתוק). רק לאחר שנוכיח יציבות טכנית ופידבק חיובי, נאפשר את פתיחת המכסה לסשנים ארוכים יותר עד למגבלת ה-12 שעות (החזון וההתרחבות העתידית מפורטים בהרחבה בסוף המסמך).
---

### 6.4 מכונת המצבים של חוויית השחקן (Player Lifecycle State Machine)

- הפרק מתאר את המעבר בין Online, Offline, Soft-Block ו-Reconnect בצורה ניהולית וברורה.

---

תרשים  (מיקרו): פירוט לוגיקת המנוע המקומי (Offline Micro-Mechanics) תרשים זה מפרט מה קורה מאחורי הקלעים עבור כל ספין כשהשחקן נמצא באופליין:



```mermaid
---
title: 6.4 מכונת המצבים של חוויית השחקן (Player Lifecycle State Machine)
---
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontFamily": "Segoe UI, Assistant, sans-serif",
    "fontSize": "16px",
    "primaryColor": "#0f172a",
    "primaryTextColor": "#f8fafc",
    "primaryBorderColor": "#38bdf8",
    "lineColor": "#38bdf8",
    "secondaryColor": "#1e293b",
    "tertiaryColor": "#1e293b",
    "edgeLabelBackground": "#0f172a",
    "nodeBorder": "#38bdf8",
    "clusterBkg": "#0b1120",
    "clusterBorder": "#38bdf8"
  },
  "flowchart": { "curve": "basis", "nodeSpacing": 30, "rankSpacing": 35, "padding": 15 },
  "sequence": { "actorMargin": 30, "messageMargin": 30 },
  "state": { "nodeSpacing": 30, "rankSpacing": 35, "titleTopMargin": 15 }
}}%%
flowchart TD
    Start((שחקן לוחץ Spin)) --> CheckBudget{האם נותרו ספינים<br>במכסת ה-Lease?}
    CheckBudget -- לא --> Alert[הצגת הודעת אלגנטית:<br>'התחבר לרשת להמשך']
    
    CheckBudget -- כן --> RNG[חישוב תוצאה לוקאלית<br>על בסיס ה-Seed מהשרת]
    RNG --> ActionType{סוג התוצאה?}
    
    ActionType -- מטבעות/מגנים --> Escrow[הוספת פריטים ל-Local Escrow]
    ActionType -- פשיטה (Raid) --> LoadRaid[שליפת בוט ממאגר ה-Ghosts<br>וביצוע פשיטת דמה]
    ActionType -- תקיפה (Attack) --> LoadAttack[שליפת בוט ממאגר ה-Ghosts<br>וביצוע תקיפת דמה]
    
    Escrow --> Hash[חישוב קריפטוגרפי:<br>Hash = Hash_prev + Action + Timestamp]
    LoadRaid --> Hash
    LoadAttack --> Hash
    
    Hash --> Queue[(הוספת הפעולה לתור הסינכרון)]
    Queue --> UI[עדכון UI:<br>אנימציית זכייה מדורגת]
```
---
מכונת המצבים של חוויית השחקן

---


<details>
<summary><b>❇️לחץ כאן לצפייה בתרשים המלא</b></summary>

---

```mermaid
---
title: 6.4 מכונת המצבים של חוויית השחקן (Player Lifecycle State Machine)
---
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontFamily": "Segoe UI, Assistant, sans-serif",
    "fontSize": "16px",
    "primaryColor": "#0f172a",
    "primaryTextColor": "#f8fafc",
    "primaryBorderColor": "#38bdf8",
    "lineColor": "#38bdf8",
    "secondaryColor": "#1e293b",
    "tertiaryColor": "#1e293b",
    "edgeLabelBackground": "#0f172a",
    "nodeBorder": "#38bdf8",
    "clusterBkg": "#0b1120",
    "clusterBorder": "#38bdf8"
  },
  "flowchart": { "curve": "basis", "nodeSpacing": 30, "rankSpacing": 35, "padding": 15 },
  "sequence": { "actorMargin": 30, "messageMargin": 30 },
  "state": { "nodeSpacing": 30, "rankSpacing": 35, "titleTopMargin": 15 }
}}%%
stateDiagram-v2
    [*] --> OnlineConnected: פתיחת המשחק ברשת תקינה
    
    state OnlineConnected {
        [*] --> RequestLease: בקשת סשן שקטה
        RequestLease --> ReadyToPlay: קבלת Token חתום + 100 ספינים + 5 בוטים
    }

    OnlineConnected --> SilentOfflineMode: ניתוק רשת / קפיצת פינג
    
    state SilentOfflineMode {
        [*] --> LocalSpinLoop: סיבוב גלגל מקומי (Local RNG)
        LocalSpinLoop --> ActionEvaluation: בדיקת תוצאה
        
        ActionEvaluation --> CoinsShields: מטבעות ומגנים
        ActionEvaluation --> GhostRaid: פשיטה על כפר בוט (NPC)
        
        CoinsShields --> EscrowQueue: רישום בכספת מקומית מוצפנת
        GhostRaid --> EscrowQueue: חישוב Hash-Chain קריפטוגרפי
        
        EscrowQueue --> CheckQuota: בדיקת מכסה
        CheckQuota --> LocalSpinLoop: נותרו ספינים (<100)
        CheckQuota --> SoftBlock: מוצתה המכסה / עברו 12 שעות
    }

    SoftBlock --> BrowsingOnly: שיטוט חופשי באלבומי קלפים
    
    SilentOfflineMode --> ReconnectingState: זיהוי אות Wi-Fi / סלולר
    BrowsingOnly --> ReconnectingState: זיהוי אות Wi-Fi / סלולר
    
    state ReconnectingState {
        [*] --> FastReplay: שידור Action Log לשרת (<10ms)
        FastReplay --> LedgerCommit: אימות קריפטוגרפי ומיזוג למאזן
        LedgerCommit --> VaultCelebration: פתיחת כספת חגיגית + Upsell Offer
    }
    
    ReconnectingState --> OnlineConnected: חזרה לסשן חי רגיל
```

</details>

---
זרימת סנכרון ו-Reconciliation


---
 ארכיטקטורת נתונים ושמירה מקומית (Data Persistence)
תרשים זה ממחיש כיצד הנתונים נשמרים מקומית באופן מאובטח ומועברים לשרת הראשי ללא סיכון הכלכלה המרכזית.

---
<details>
<summary><b>❇️לחץ כאן לצפייה בתרשים המלא</b></summary>
---
---

```mermaid
---
title: 6.4 מכונת המצבים של חוויית השחקן (Player Lifecycle State Machine)
---
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontFamily": "Segoe UI, Assistant, sans-serif",
    "fontSize": "16px",
    "primaryColor": "#0f172a",
    "primaryTextColor": "#f8fafc",
    "primaryBorderColor": "#38bdf8",
    "lineColor": "#38bdf8",
    "secondaryColor": "#1e293b",
    "tertiaryColor": "#1e293b",
    "edgeLabelBackground": "#0f172a",
    "nodeBorder": "#38bdf8",
    "clusterBkg": "#0b1120",
    "clusterBorder": "#38bdf8"
  },
  "flowchart": { "curve": "basis", "nodeSpacing": 30, "rankSpacing": 35, "padding": 15 },
  "sequence": { "actorMargin": 30, "messageMargin": 30 },
  "state": { "nodeSpacing": 30, "rankSpacing": 35, "titleTopMargin": 15 }
}}%%
sequenceDiagram
    participant App as Mobile App
    participant Vault as Local Hash-Chain
    participant Sync as Sync / Replay Engine
    participant Server as Game Server (Ledger)
    
    Note over App,Server: ONLINE: הכנת סשן האופליין
    Server-->>App: שליחת Seed, Budget (100 Spins), 5 Ghost Targets
    
    Note over App,Vault: OFFLINE EXECUTION
    App->>Vault: רצף ספינים + תקיפת NPC Bots
    Vault->>Vault: חתימת כל פעולה בשרשרת Hash
    App->>Vault: רכישת Offline IAP מושהית
    
    Note over App,Server: RECONNECTION: חזרה לרשת
    App->>Sync: פריקת Log מלא ואימות קבלת רכישה
    Sync->>Sync: Fast-Forward Replay (<10ms)
    alt זיוף שעון או שבירת Hash
        Sync-->>App: פסילת סשן וחזרה למאזן שרת קודם
    else יומן תקין ומאומת
        Sync->>Server: חיוב בפועל ומיזוג למאזן הראשי
        Server-->>App: פתיחת ה-Escrow Vault לאיסוף מטבעות לשחקן
    end
```

</details>


---
```mermaid
---
title: 6.4 מכונת המצבים של חוויית השחקן (Player Lifecycle State Machine)
---
%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontFamily": "Segoe UI, Assistant, sans-serif",
    "fontSize": "16px",
    "primaryColor": "#0f172a",
    "primaryTextColor": "#f8fafc",
    "primaryBorderColor": "#38bdf8",
    "lineColor": "#38bdf8",
    "secondaryColor": "#1e293b",
    "tertiaryColor": "#1e293b",
    "edgeLabelBackground": "#0f172a",
    "nodeBorder": "#38bdf8",
    "clusterBkg": "#0b1120",
    "clusterBorder": "#38bdf8"
  },
  "flowchart": { "curve": "basis", "nodeSpacing": 30, "rankSpacing": 35, "padding": 15 },
  "sequence": { "actorMargin": 30, "messageMargin": 30 },
  "state": { "nodeSpacing": 30, "rankSpacing": 35, "titleTopMargin": 15 }
}}%%
flowchart TD
    subgraph Client ["Client Device (Offline)"]
        direction TB
        A["ממשק שחקן"] --> B["מנוע אופליין (Offline Engine)"]
        B --> C[/"שמירה ב-DB מקומי"/]
        C --> D["חתימת תור (Hash-Chain)"]
    end

    subgraph Server ["Server (Cloud)"]
        direction TB
        E{"API Gateway"} 
        F["מנוע אימות (Anti-Cheat)"]
        G[/"כספת המתנה (Escrow Vault)"/]
        H[/"המאזן הראשי (Ledger)"/]
        
        E --> F
        F -->|"זיוף"| Reject(("חסימה"))
        F -->|"תקין"| G
        G --> H
    end

    D -.->|"סנכרון בחזרת רשת"| E

```


---

