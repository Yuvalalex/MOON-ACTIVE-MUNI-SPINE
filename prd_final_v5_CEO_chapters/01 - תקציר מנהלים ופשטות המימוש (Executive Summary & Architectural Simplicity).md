<!-- מקור: Master_PRD.md | פרק 1 מתוך 25 -->

> **שם הפרק:** תקציר מנהלים ופשטות המימוש (Executive Summary & Architectural Simplicity)

> ניווט: — | [README](README.md) | [פרק 02](<02 - למה זה פשוט ולא מורכב למימוש (The 3-Block Plug-and-Play Simplicity).md>)

## 1. תקציר מנהלים ופשטות המימוש (Executive Summary & Architectural Simplicity)

- המטרה: לאפשר רציפות משחק בתנאי קליטה חלשים בלי לשכתב את המערכת המרכזית.
- המסר ההנדסי: השרת נשאר מקור האמת, וההרחבה נבנית כמעטפת מדורגת ובטוחה.

### 1.1 חזון המוצר והרציונל העסקי

- חזון המוצר: Coin Master נשאר זמין גם בניתוקי רשת, טיסות ומנהרות.
- רציונל עסקי: להפוך בעיית תסכול צרכנית למנוע Retention וצמיחה מדידים.

> [!NOTE]
> **מתודולוגיית קבלת החלטות מונחית נתונים (Data-Driven Decision Making):**  
> ככלל - פתרון לבעיית ניתוקי רשת הוא לא רק מענה צרכני לתלונות שחקנים, אלא **מהלך צמיחה אסטרטגי (Strategic Growth Initiative)** לכל דבר ועניין.  
---

> [!IMPORTANT]
> **עיקרון הרציפות החווייתית (Unbroken Player Engagement):**  
> ניתוח טלמטריית משתמשים מוכיח כי כל ניתוק כפוי במהלך סשן פעיל מהווה דליפת שימור (Retention Leak) בלתי-הכרחית. שחקני Coin Master אינם מבחינים בין כשל תשתית סלולרית לכשל אפליקטיבי — עבורם, חוסר זמינות גורם לנטישה מיידית לטובת אפליקציות מתחרות. מענה מבוקר לבעיה זו מייצר יתרון תחרותי מובהק.
---

<details>
<summary><b>📖 לחץ כאן להרחבת החזון</b></summary>

החזון הוא ביצוע קפיצת מדרגה ארכיטקטונית: מעבר ממודל Online נוקשה ושביר למודל **Hybrid Offline Mode** המבוסס על "חכירת סשן מראש" (**Leased Session Architecture**).
</details>

---
```mermaid
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
flowchart LR
    subgraph ZERO_RISK ["🛡️ <b>ZERO-RISK ARCHITECTURE & FEASIBILITY</b>"]
        direction LR
        
        R1["🗄️ <b>אפס שינוי ב-Master DB</b><br/>━━━━━━━━━━━━━━━━━━━━<br/><span style='font-size:18px;color:#34d399;'>• ה-Master DB נשאר ללא שינוי במילימטר</span><br/><span style='font-size:17px;color:#cbd5e1;'>• אפס מיגרציות מסוכנות • אפס השבתות (Zero Downtime)</span>"]
        
        R2["⚙️ <b>שימוש ביכולות Unity קיימות</b><br/>━━━━━━━━━━━━━━━━━━━━<br/><span style='font-size:18px;color:#38bdf8;'>• מנוע PRNG דטרמיניסטי כבר פעיל בלקוח</span><br/><span style='font-size:17px;color:#cbd5e1;'>• כפרי בוטים (Ghosts) קיימים בבדיקות אוטומציה<br/>• ספריית SQLCipher מוטמעת ומוכחת במוצר</span>"]
        
        R3["🧯 <b>הגנה מוחלטת מכשל (Blast Radius = 0)</b><br/>━━━━━━━━━━━━━━━━━━━━<br/><span style='font-size:18px;color:#f59e0b;'>• תקלה מקומית מחזירה מיידית להתנהגות הקיימת</span><br/><span style='font-size:17px;color:#cbd5e1;'>• דרישת חיבור פשוטה • אפס סיכון לשחקני האונליין</span>"]

        %% כפיית סדר אופקי
        R1 ~~~ R2 ~~~ R3
    end

    %% מסגרת עליונה כהה
    style ZERO_RISK fill:#090d16,stroke:#38bdf8,stroke-width:3px,color:#38bdf8

    %% כרטיסים פנימיים כהים ומודגשים
    style R1 fill:#0f172a,stroke:#34d399,stroke-width:2px,color:#f8fafc
    style R2 fill:#0f172a,stroke:#38bdf8,stroke-width:2px,color:#f8fafc
    style R3 fill:#0f172a,stroke:#f59e0b,stroke-width:2px,color:#f8fafc

```



```mermaid
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
graph TD
    subgraph Threat1["1. אבטחה ורמאויות (Anti-Cheat)"]
        T1["הגרלות בצד שרת (RNG)<br/>חשש מביטול ספין מפסיד (Save-Scumming)<br/>מניפולציות שעון מקומי (Time-Travel)"]
    end
    subgraph Threat2["2. סנכרון ו-PvP חברתי"]
        T2["תנאי מרוץ (Race Conditions)<br/>תקיפת כפר מוגן בו-זמנית<br/>שימוש מקביל בשני מכשירים"]
    end
    subgraph Threat3["3. מונטיזציה ו-LiveOps"]
        T3["אימות רכישות IAP ללא שרת<br/>הורדת נכסי וידאו מקמפיינים<br/>ניהול אירועי טורניר דינמיים"]
    end

    Threat1 --> Impact["קריסת כלכלת המשחק, אינפלציית משאבים ואובדן הכנסות"]
    Threat2 --> Impact
    Threat3 --> Impact

    style Threat1 fill:#450a0a,stroke:#f87171,stroke-width:2px,color:#fff
    style Threat2 fill:#431407,stroke:#fb923c,stroke-width:2px,color:#fff
    style Threat3 fill:#2e1065,stroke:#c084fc,stroke-width:2px,color:#fff
    style Impact fill:#1f2937,stroke:#ef4444,stroke-width:2px,color:#fff
```

---
```mermaid
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
flowchart LR

    %% עמודת איומים ואתגרים
    subgraph THREATS ["⚠️ אתגרי ליבה באופליין (Core Vulnerabilities)"]
        direction TB
        T1["🎲 <b>1. אבטחה ורמאויות (Anti-Cheat)</b><br/><span style='font-size:13px;color:#cbd5e1;'>• חשש מביטול ספין מפסיד (Save-Scumming)<br/>• מניפולציות שעון מקומי (Time-Travel)<br/>• עריכת זיכרון מקומית לשכפול מטבעות</span>"]
        T2["⚔️ <b>2. סנכרון ו-PvP חברתי</b><br/><span style='font-size:13px;color:#cbd5e1;'>• תנאי מרוץ (Race Conditions) בתקיפות<br/>• תקיפת כפר שמוגן אונליין במקביל<br/>• פיצול סטייט בשימוש בשני מכשירים</span>"]
        T3["💎 <b>3. מונטיזציה ו-LiveOps</b><br/><span style='font-size:13px;color:#cbd5e1;'>• אימות רכישות IAP ללא רשת זמינה<br/>• ניהול אירועים מוגבלי זמן (LiveOps Drift)<br/>• סכנת חלוקת משאבים ללא תשלום מאומת</span>"]
    end

    %% עמודת פתרונות ומנגנוני בקרה
    subgraph CONTROLS ["🛡️ מנגנוני בקרה וארכיטקטורת פתרון"]
        direction TB
        S1["🔐 <b>פתרון אבטחה ודטרמיניזם</b><br/><span style='font-size:13px;color:#cbd5e1;'>• <b>Leased Session Token:</b> מכסה מוגבלת (100 ספינים/12h)<br/>• <b>שרשרת גיבוב:</b> Hash-Chain SHA-256 לפסילת זיופים<br/>• <b>NTP Uptime:</b> שעון מונה פנימי בלתי תלוי ב-OS</span>"]
        S2["👻 <b>פתרון סנכרון ובידוד PvP</b><br/><span style='font-size:13px;color:#cbd5e1;'>• <b>Ghost Village Cache:</b> ניתוב תקיפות ל-5 כפרי דמה מובנים<br/>• <b>Zero Live Conflict:</b> אפס השפעה על שחקנים אמיתיים<br/>• <b>First-Write-Wins & Token Lock:</b> נעילת מכשיר יחיד</span>"]
        S3["📦 <b>פתרון מונטיזציה ו-Escrow</b><br/><span style='font-size:13px;color:#cbd5e1;'>• <b>Offline Escrow Vault:</b> צבירה מושהית ב-SQLCipher<br/>• <b>Delayed Intent Queue:</b> שמירת כוונת קנייה בלבד<br/>• <b>Zero-Fulfillment Rule:</b> מימוש רק לאחר אישור חנות ושרת</span>"]
    end

    %% תוצאה עסקית וטכנולוגית סופית
    subgraph OUTCOME ["🎯 הישג ויציבות מערכתית"]
        direction TB
        RES["👑 <b>כלכלה מוגנת הרמטית וחוויה רציפה</b><br/>━━━━━━━━━━━━━━━━━━━━<br/>• <b>100% הגנה</b> על ה-Economy ומניעת אינפלציה<br/>• <b>Zero Friction:</b> רציפות משחק בטיסות ובאזורי ניתוק<br/>• <b>Zero-Trust Client:</b> השרת בלבד נשאר מקור האמת"]
    end

    %% חיבורים וזרימה
    T1 ==>|"מענה ארכיטקטוני"| S1
    T2 ==>|"מענה ארכיטקטוני"| S2
    T3 ==>|"מענה ארכיטקטוני"| S3

    S1 ==> RES
    S2 ==> RES
    S3 ==> RES

    %% סגנונות כהים ומודרניים
    style THREATS fill:#090d16,stroke:#ef4444,stroke-width:2.5px,color:#f87171
    style CONTROLS fill:#090d16,stroke:#38bdf8,stroke-width:2.5px,color:#38bdf8
    style OUTCOME fill:#090d16,stroke:#10b981,stroke-width:2.5px,color:#34d399

    style T1 fill:#450a0a,stroke:#f87171,stroke-width:1.5px,color:#f8fafc
    style T2 fill:#431407,stroke:#fb923c,stroke-width:1.5px,color:#f8fafc
    style T3 fill:#2e1065,stroke:#c084fc,stroke-width:1.5px,color:#f8fafc

    style S1 fill:#0f172a,stroke:#38bdf8,stroke-width:1.5px,color:#f8fafc
    style S2 fill:#0f172a,stroke:#38bdf8,stroke-width:1.5px,color:#f8fafc
    style S3 fill:#0f172a,stroke:#38bdf8,stroke-width:1.5px,color:#f8fafc

    style RES fill:#064e3b,stroke:#34d399,stroke-width:2px,color:#ecfdf5
```


---

---

```mermaid
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
graph LR
    subgraph Growth["1. צמיחה בשווקי Volume"]
        A["Emerging Markets<br/>(הודו, ברזיל, מקסיקו)<br/>הסרת חסם הניתוקים ב-4G"]
    end
    subgraph Mon["2. מונטיזציה מתמשכת"]
        B["Offline IAP & Escrow<br/>רכישת חבילות בטיסות/רכבות<br/>חיוב מושהה בחזרת רשת"]
    end
    subgraph Retention["3. לכידת קשב בשעות מתות"]
        C["Captive Attention<br/>המשחק היחיד שעובד באופליין<br/>הארכת סשן ב-15%-25%"]
    end
    subgraph HW["4. אופטימיזציית חומרה"]
        D["Low-End Hardware Fit<br/>חיסכון בהפעלת מודם סלולרי<br/>מניעת התחממות ופריקת סוללה"]
    end

    A --> B --> C --> D

    style Growth fill:#1e293b,stroke:#3b82f6,stroke-width:2px,color:#fff
    style Mon fill:#1e293b,stroke:#10b981,stroke-width:2px,color:#fff
    style Retention fill:#1e293b,stroke:#f59e0b,stroke-width:2px,color:#fff
    style HW fill:#1e293b,stroke:#ec4899,stroke-width:2px,color:#fff
```



---

```mermaid
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
flowchart LR

    subgraph PlugPlay ["🧩 3 רכיבי ה-Plug-and-Play החדשים (מבודדים ובטוחים)"]
        direction TB
        B1["📱 <b>1. Client Network Interceptor</b><br/><span style='font-size:14px;color:#94a3b8;'>• מנוע יירוט בלקוח (~250 שורות C# ב-Unity)<br/>• ניתוב שקוף ל-SQLite מקומי בפינג איטי</span>"]
        B2["🛡️ <b>2. Isolated Escrow Buffer</b><br/><span style='font-size:14px;color:#94a3b8;'>• טבלת חיץ מבודדת (Redis / Postgres)<br/>• צבירת נתוני אופליין באפס מגע עם ה-Core</span>"]
        B3["⚡ <b>3. Fast-Forward Replay Validator</b><br/><span style='font-size:14px;color:#94a3b8;'>• פונקציית אימות קלה (Stateless Go Function)<br/>• בדיקת Hash-Chain בתוך פחות מ-5ms</span>"]

        B1 ==>|"סנכרון בחזרת רשת"| B2
        B2 ==>|"הרצת ולידציה אסינכרונית"| B3
    end

    subgraph Legacy ["🏛️ תשתית שרת חיה קיימת (100% שמורה וללא שינוי)"]
        direction TB
        
        LiveServer["🖥️ <b>Live Game Server</b><br/><span style='font-size:14px;color:#94a3b8;'>שרתי משחק חיים (Game Core Engine)</span>"]
        LiveLedger[("🏦 <b>Master Game Ledger</b><br/><span style='font-size:14px;color:#34d399;'>טבלאות שחקנים ומאזן מרכזי קיים</span>")]
        
        LiveServer ~~~ LiveLedger
    end

    B3 ==>|"אימות קריפטוגרפי מאושר בלבד"| LiveLedger
    LiveServer ==>|"יש רשת רשת"| LiveLedger
    LiveServer ==>|"אין רשת רשת"| B1


    %% מסגרות חיצוניות - רקע כהה ועמוק
    style PlugPlay fill:#090d16,stroke:#38bdf8,stroke-width:2.5px,color:#38bdf8
    style Legacy fill:#090d16,stroke:#64748b,stroke-width:2px,stroke-dasharray: 4 4,color:#94a3b8

    %% כרטיסים פנימיים כהים עם מסגרות ניאון מובחנות
    style B1 fill:#0f172a,stroke:#34d399,stroke-width:2px,color:#f8fafc
    style B2 fill:#0f172a,stroke:#f59e0b,stroke-width:2px,color:#f8fafc
    style B3 fill:#0f172a,stroke:#a855f7,stroke-width:2px,color:#f8fafc

    %% כרטיסי תשתית קיימת
    style LiveServer fill:#0f172a,stroke:#475569,stroke-width:1.5px,color:#e2e8f0
    style LiveLedger fill:#064e3b,stroke:#34d399,stroke-width:2px,color:#ecfdf5
```

---





![Coin Master Hybrid Offline Architecture Overview](assets/pic/image-2.png)
*תרשים תפיסתי: ארכיטקטורת המעטפת ההיברידית של Coin Master מול השרת המרכזי*

---


---

```mermaid
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

    subgraph TOP ["📈 <b>צמיחה, שימור והכנסות (Top-Line Growth)</b>"]
        direction LR
        K1["💰 <b>הגדלת הכנסות (Offline IAP)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> 2%-4% ל-ARPU</span><br/><span style='font-size:18px;color:#34d399;'>💵 <b>אימפקט:</b> תוספת $2M-$4M לחודש</span>"]
        K2["🛑 <b>שימור משתמשים (Retention)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> +3%-5% ב-D1/D7</span><br/><span style='font-size:18px;color:#cbd5e1;'>👥 <b>אימפקט:</b> מניעת נטישת ~150K שחקנים/חודש</span>"]
        K3["🛡️ <b>חסימת זליגה (Anti-Churn)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> הגנה על נתח שוק</span><br/><span style='font-size:18px;color:#fcd34d;'>🔒 <b>אימפקט:</b> עצירת מעבר למשחקים מתחרים</span>"]
        K4["🚀 <b>רכישה אורגנית (Organic UA)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> +5%-7% בהורדות</span><br/><span style='font-size:18px;color:#c084fc;'>📥 <b>אימפקט:</b> 200,000+ התקנות חינמיות</span>"]
        
        K1 ~~~ K2 ~~~ K3 ~~~ K4
    end

    subgraph BOTTOM ["⚙️ <b>יעילות, בידול וחוויית משתמש (Efficiency & Product Advantage)</b>"]
        direction LR
        K5["☁️ <b>חיסכון תעבורה ושרת</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> ירידה של 20%-30% בעומס</span><br/><span style='font-size:18px;color:#22d3ee;'>📉 <b>אימפקט:</b> חיסכון של $50K-$100K לחודש</span>"]
        K6["👑 <b>יתרון תחרותי (USP)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> 100% Share of Voice</span><br/><span style='font-size:18px;color:#f472b6;'>✨ <b>אימפקט:</b> בידול מוחלט מול Monopoly Go</span>"]
        K7["⏳ <b>הגדלת ערך שחקן (LTV)</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> +15%-25% באורך סשן</span><br/><span style='font-size:18px;color:#818cf8;'>⏱️ <b>אימפקט:</b> 5-10 דקות מסך נוספות</span>"]
        K8["🔋 <b>חוויית משתמש ו-ASO</b><br/><br/><span style='font-size:17px;color:#38bdf8;'>📊 <b>הערכה:</b> מניעת זלילת סוללה</span><br/><span style='font-size:18px;color:#4ade80;'>⭐ <b>אימפקט:</b> עלייה של 0.1-0.2 בדירוג בחנויות</span>"]
        
        K5 ~~~ K6 ~~~ K7 ~~~ K8
    end

    TOP ~~~ BOTTOM

    %% מסגרות ראשיות כהות ומודרניות
    style TOP fill:#090d16,stroke:#38bdf8,stroke-width:3px,color:#38bdf8
    style BOTTOM fill:#090d16,stroke:#10b981,stroke-width:3px,color:#34d399

    %% כרטיסים עליונים
    style K1 fill:#0f172a,stroke:#34d399,stroke-width:2px,color:#f8fafc
    style K2 fill:#0f172a,stroke:#38bdf8,stroke-width:2px,color:#f8fafc
    style K3 fill:#0f172a,stroke:#f59e0b,stroke-width:2px,color:#f8fafc
    style K4 fill:#0f172a,stroke:#a855f7,stroke-width:2px,color:#f8fafc

    %% כרטיסים תחתונים
    style K5 fill:#0f172a,stroke:#06b6d4,stroke-width:2px,color:#f8fafc
    style K6 fill:#0f172a,stroke:#ec4899,stroke-width:2px,color:#f8fafc
    style K7 fill:#0f172a,stroke:#6366f1,stroke-width:2px,color:#f8fafc
    style K8 fill:#0f172a,stroke:#10b981,stroke-width:2px,color:#f8fafc
```

---



```mermaid
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
    subgraph PITCH ["🎯 <b>EXECUTIVE SUMMARY: THE PM PITCH</b>"]
        direction LR
        
        P1["🛡️ <b>גידור סיכונים ו-Economy</b><br/>━━━━━━━━━━━━━━━━━━━━<br/>🔒 <b>Leased Offline State</b> פותר את אתגרי הליבה<br/>של ה-Social Casino במניעת הונאות ואבטחת נכסים."]
        
        P2["🌍 <b>מנוף סקייל גלובלי</b><br/>━━━━━━━━━━━━━━━━━━━━<br/>🚀 <b>Low-Bandwidth Resilience</b> מרחיב שווקים,<br/>מוריד חסמי חומרה ומעלה Retention בהודו וברזיל."]
        
        P3["👑 <b>The Bottom Line</b><br/>━━━━━━━━━━━━━━━━━━━━<br/>💎 מהלך Game-Changer שמשאיר את Coin Master<br/>עמוק בכיס של השחקן — <b>תמיד זמין, תמיד מתגמל.</b>"]

        P1 ~~~ P2 ~~~ P3
    end

    %% סגנון ראשי - Dark Executive
    style PITCH fill:#090d16,stroke:#38bdf8,stroke-width:2.5px,color:#38bdf8
    
    %% כרטיסים כהים עם מסגרות זוהרות
    style P1 fill:#0f172a,stroke:#38bdf8,stroke-width:1.5px,color:#f8fafc
    style P2 fill:#0f172a,stroke:#34d399,stroke-width:1.5px,color:#f8fafc
    style P3 fill:#0f172a,stroke:#f59e0b,stroke-width:1.5px,color:#f8fafc
```


---

> [!NOTE]
> **מתודולוגיית הערכה מספרית (Guesstimation):** האומדן הכמותי מטה הותאם לקנה המידה העצום של Coin Master (בהנחת יסוד של מיליוני DAU והכנסות חודשיות בטווח הגבוה של תעשיית ה-Social Casino). אחוזי השיפור נגזרו מ-Benchmarks תעשייתיים בטיפול ב-Drop-offs.
---


**תרשים (מאקרו): ניתוב פעולות השחקן (Network Interception)**

<details>
<summary><b>❇️לחץ כאן לצפייה בתוכן המלא❇️</b></summary>

```mermaid
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
graph TD
    UserAction[פעולת שחקן: ספין / רכישה / שדרוג] --> CheckNetwork{האם יש קליטה מהירה?}
    
    CheckNetwork -- כן --> NormalFlow[שליחת בקשה לשרת בזמן אמת]
    CheckNetwork -- לא / איטי מדי --> Intercept[מנוע אופליין מיירט את הבקשה]
    
    Intercept --> CheckLease{האם ה-Leased State בתוקף?}
    
    CheckLease -- לא (חריגה מ-12 שעות או מספינים) --> HardBlock[מסך: 'אנא התחבר מחדש']
    CheckLease -- כן --> LocalExe[מעבר לזרימת מיקרו אופליין]
    
    LocalExe --> BG_Sync[המתנה שקופה ברקע]
    BG_Sync -. כשהרשת חוזרת .-> PushSync[סנכרון אסינכרוני מהיר לשרת]
```

</details>


---

