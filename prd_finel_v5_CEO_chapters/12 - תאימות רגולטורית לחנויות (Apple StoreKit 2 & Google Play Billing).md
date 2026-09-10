<!-- מקור: prd_finel_v5_CEO.md | פרק 12 מתוך 25 -->

> **שם הפרק:** תאימות רגולטורית לחנויות (Apple StoreKit 2 & Google Play Billing)

> ניווט: [פרק 11](<11 - דרישות לא-פונקציונליות, שוברי מעגלים ו-Kill-Switch (Circuit Breakers & Emergency Governance).md>) | [README](README.md) | [פרק 13](<13 - ערך עסקי ורווחי - טווח מיידי מול טווח בינוני-ארוך (Business Value & Two-Horizon ROI).md>)

## 12. תאימות רגולטורית לחנויות (Apple StoreKit 2 & Google Play Billing)

- הקו המנחה: אין להניח אישור רגולטורי מראש; כל זרימה תעבור Legal ו-Platform.
- ב-MVP נשמרת לכל היותר כוונת רכישה, ולא מוענקים נכסים לפני אישור מאומת.

> [!IMPORTANT]
> **שאלת הנהלה מרכזית:** האם אפל וגוגל מאשרות מודל של "רכישה באופליין" (Offline IAP)?
### 12.1 יישום מבוסס StoreKit 2 ו-Google Play Billing Deferred Queue

- המודל מבדיל בין intent אופלייני לבין settlement אונלייני מאומת ו-idempotent.
<details>
<summary>📖 להרחבה - פרטים טכניים מלאים</summary>

המסמך אינו קובע תאימות רגולטורית מראש. כל זרימת רכישה תחייב סקירה ואישור עדכניים של Apple, Google והייעוץ המשפטי. ב-MVP נשמרת לכל היותר כוונת רכישה מקומית; fulfillment מתבצע רק לאחר transaction מאומתת ו-idempotent:

</details>

12.1 יישום מבוסס StoreKit 2 ו-Google Play Billing Deferred Queue

```mermaid

%%{init: {
  "theme": "base",
  "themeVariables": {
    "fontFamily": "Segoe UI, Assistant, sans-serif",
    "fontSize": "22px",
    "primaryColor": "#0f172a",
    "primaryTextColor": "#f8fafc",
    "primaryBorderColor": "#38bdf8",
    "lineColor": "#38bdf8",
    "titleColor": "#38bdf8"
  },
  "flowchart": {
    "nodeSpacing": 35,
    "rankSpacing": 50,
    "padding": 35
  }
}}%%
flowchart LR

    subgraph OFFLINE_PHASE ["✈️ <b>שלב אופליין (In-Flight Intent)</b>"]
        direction TB
        S1["🛒 <b>1. בחירת חבילה</b><br/><br/><span style='font-size:18px;color:#94a3b8;'>שחקן בוחר חבילה בחנות (למשל: $4.99 / 150 ספינים)</span>"]
        S2["📝 <b>2. רישום Purchase Intent</b><br/><br/><span style='font-size:18px;color:#94a3b8;'>הנפקת רשומת הבטחה חתומה מקומית ב-SQLCipher</span>"]
        S3["🔒 <b>3. אפס מימוש מוקדם (Zero Fulfillment)</b><br/><br/><span style='font-size:18px;color:#fbbf24;'>המשאבים נעולים לחלוטין עד אישור השרת והחנות</span>"]
        
        S1 ==> S2 ==> S3
    end

    subgraph ONLINE_PHASE ["🌐 <b>חזרת רשת (Store Settlement & Vault Unlock)</b>"]
        direction TB
        S4["📱 <b>4. הפעלת Native Sheet</b><br/><br/><span style='font-size:18px;color:#38bdf8;'>פתיחת מסך תשלום מול Apple / Google בחזרת הרשת</span>"]
        S5["✅ <b>5. אישור תשלום ומיזוג</b><br/><br/><span style='font-size:18px;color:#34d399;'>קבלה מאומתת בשרת ➔ שחרור המשאבים למאזן הראשי</span>"]
        S6["🛑 <b>6. מנגנון ביטול ו-Rollback</b><br/><br/><span style='font-size:18px;color:#f87171;'>במקרה של סירוב תשלום/ביטול: הרשומה נמחקת מיידית</span>"]
        
        S4 ==> S5
        S4 -.->|"סירוב / כשל"| S6
    end

    OFFLINE_PHASE ==>|"חיבור מחודש לרשת"| ONLINE_PHASE

    %% מסגרות חיצוניות כהות ומודגשות
    style OFFLINE_PHASE fill:#090d16,stroke:#f59e0b,stroke-width:3px,color:#fbbf24
    style ONLINE_PHASE fill:#090d16,stroke:#10b981,stroke-width:3px,color:#34d399

    %% כרטיסים פנימיים מוגדלים
    style S1 fill:#0f172a,stroke:#334155,stroke-width:2px,color:#f8fafc
    style S2 fill:#0f172a,stroke:#334155,stroke-width:2px,color:#f8fafc
    style S3 fill:#0f172a,stroke:#f59e0b,stroke-width:2px,color:#f8fafc

    style S4 fill:#0f172a,stroke:#38bdf8,stroke-width:2px,color:#f8fafc
    style S5 fill:#064e3b,stroke:#34d399,stroke-width:2.5px,color:#ecfdf5
    style S6 fill:#450a0a,stroke:#ef4444,stroke-width:2px,color:#fee2e2
```
---

### 12.2 מפרט טכנולוגי: Apple StoreKit 2 ו-Google Play Billing Deferred Queue

כדי להבטיח עמידה בלתי מתפשרת בדרישות חברות אפל וגוגל, יישום הרכישות באופליין אינו כולל אספקה מוקדמת של מוצרים:

#### 1. יישום מבוסס Apple StoreKit 2 (iOS)
- הלקוח עושה שימוש במנגנון ה-`Transaction.updates` האסינכרוני של StoreKit 2.
- בעת בחירת חבילת ספינים במצב טיסה, נוצרת רשומת `SignedPurchaseIntent` מקומית המוצפנת ב-Keychain:
  ```json
  {
    "intent_id": "pi_9921_apple",
    "product_id": "com.moonactive.coinmaster.spins150",
    "price_tier": "USD_4_99",
    "local_timestamp": 1773061200,
    "device_key_attestation": "attest_p256_sig"
  }
  ```
- ברגע חידוש הקשר, האפליקציה מזניקה את ה-Native StoreKit Sheet לסליקה רשמית מול שרתי Apple.
- ה-Server-to-Server Webhook (`App Store Server Notifications V2`) מאמת את ה-JWS Transaction, ורק אז מזכה את ה-Master Ledger.

#### 2. יישום מבוסס Google Play Billing Library 6.x+ (Android)
- שימוש במנגנון ה-Deferred Purchase Intent.
- כל עוד המכשיר ללא חיבור רשת, ספריית `BillingClient` אינה מנסה ליצור תקשורת כושלת, אלא ממתינה לאירוע `onBillingServiceDisconnected` / `onNetworkReconnected`.
- אימות קבלה סופי מתבצע דרך `Google Play Developer API` (Google Cloud Pub/Sub Topic) לפני פריקת המטבעות לחשבון.

---

### 12.3 תאימות משפטית, רגולציית הימורים והגנת הצרכן (Legal & Compliance)

1. **עמידה בהנחיות Apple Guideline 3.1.1 ו-Google Play Monetization:**
   - כלל הברזל: **אפס מימוש ללא תשלום (Zero Unverified Fulfillment)** מבטיח שאפל וגוגל מקבלות את מלוא עמלת ה-30% שלהן, ללא שום חשד למעקף סליקה או עסקאות שחורות.
2. **הגנה מטענות "הימורים ללא רישיון" (Social Casino Regulations):**
   - המשחק אינו מאפשר פדיון כספי של מטבעות וירטואליים.
   - מאחר שתוצאות הספינים באופליין נקבעות על ידי מנוע PRNG דטרמיניסטי חתום מראש ולא על ידי אלגוריתם מקומי הניתן למניפולציה, המשחק עומד בתקני השקיפות של ה-UK Gambling Commission ו-EU Digital Services Act (DSA).
3. **ניהול סיכוני Chargeback ו-Refunds:**
   - במקרה ששחקן מבצע רכישה, מתחרט ומבטל את העסקה מול הבנק או חנות האפליקציות לאחר הנחיתה – מערכת ה-Reconciliation מזהה את ביטול ה-Transaction ומבצעת ביטול (Rollback) אוטומטי של הספינים והמטבעות שהיו בכספת טרם פריקתם.

