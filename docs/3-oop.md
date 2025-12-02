# פרק 3: הקדמה לתכנות מונחה עצמים (OOP)

עד עכשיו כתבנו משתנים ופקודות "באוויר". ברובוט אמיתי יש המון מנועים וחיישנים. אם נכתוב את הכל במקום אחד, נקבל "קוד ספגטי" - ארוך, מסובך וקשה לתיקון.

הפתרון: **מחלקות (Classes).**
אנחנו נתייחס לכל מנגנון ברובוט (למשל: שאיבה, מערכת נסיעה, ירייה) בתור יחידה עצמאית.

## השרטוט מול הרובוט 📝🤖

כדי להבין את ההבדל בין **מחלקה** (Class) ל**אובייקט** (Object), נשתמש במטפורה של אדריכל:

1.  **המחלקה (Class) = השרטוט:**
    זהו דף הנייר שבו מתואר איך בונים את הצבת. כתוב שם איזה מנוע צריך ומה הצבת יודעת לעשות. השרטוט עצמו לא יכול לירות כדורים. הוא רק רעיון.

2.  **האובייקט (Object) = המנגנון האמיתי:**
    זהו הדבר הפיזי שבנינו לפי השרטוט. יש לו מנוע אמיתי, והוא תופס דברים במציאות.

> **בקיצור:** אנחנו כותבים **מחלקה** (פעם אחת), וממנה יוצרים **אובייקט** (שיעבוד ברובוט).

-----

## איך בונים מחלקה? (המבנה)

לכל מחלקה (מנגנון ברובוט) יש שלושה חלקים קבועים. בואו נבנה מחלקה עבור **צבת (Gripper)**.

### 1\. שדות (Fields) - "מה יש לי?"

כאן נגדיר את הרכיבים ששייכים למנגנון הזה. לצבת שלנו יש מנוע סרוו.

### 2\. בנאי (Constructor) - "איך מחברים אותי?"

זו פעולה מיוחדת שרצה רק פעם אחת - כשיוצרים "עותק" חדש של האובייקט. כאן בדרך כלל אנחנו מחברים את המנועים והחיישנים לשאר המחלקה שלנו.

### 3\. שיטות (Methods) - "מה אני יודע לעשות?"

כאן נגדיר את הפעולות: לפתוח, לסגור.

-----

## הדוגמה המלאה: מחלקת `Gripper`

הנה איך זה נראה בקוד. שימו לב להערות:

```java
// שם המחלקה - מתחיל באות גדולה
public class Gripper {

    // --- חלק 1: שדות (מה יש לי?) ---
    // נתעמק בדף הבא בדיוק במה כל אחד המילים האלו אומרת, כרגע נתייחס לזה כקיים.
    private Motor gripperMotor; 

    // --- חלק 2: הבנאי (ההתקנה) ---
    // הפעולה הזו נקראת בדיוק כמו שם המחלקה
    public Gripper(Motor gripperMotor) {
        // אנחנו מכניסים את המנוע שקיבלנו מהבנאי לשדה שלנו(כדי שכל הפעולות שלנו יוכלו להשתמש בו)
        this.gripperMotor = gripperMotor;
    }

    // --- חלק 3: פעולות (מה אני עושה?) ---
    
    // פעולה לפתיחת הצבת
    public void open() {
        gripperMotor.set(1.0); // נניח ש-1 זה פתוח
    }

    // פעולה לסגירת הצבת
    public void close() {
        gripperMotor.set(0.0); // נניח ש-0 זה סגור
    }
}
```

-----

## איך משתמשים בזה? (יצירת אובייקט)

יופי, יש לנו "שרטוט" (מחלקה) של צבת. עכשיו אנחנו בתוך הקוד הראשי של הרובוט ורוצים להשתמש בה.

כאן נכנסת מילת הקסם: **`new`**.

### השלבים לשימוש:

1.  **הצהרה:** יוצרים משתנה מסוג המחלקה שיצרנו.
2.  **אתחול:** משתמשים ב-`new` כדי לבנות את האובייקט (קוראים לבנאי).
3.  **שימוש:** קוראים לפעולות שיצרנו (`open`, `close`) בעזרת נקודה.

<!-- end list -->

```java
// אנחנו בתוך הקוד הראשי של הרובוט 

// 1. הצהרה: יש לנו משתנה מסוג Gripper
Gripper myGripper; 

// --- בתוך ה-Init (אתחול) ---
// 2. יצירת האובייקט האמיתי
// המילה new הופכת את השרטוט למציאות!
// בקוד האמיתי היינו צריכים גם להגדיר את motor, כרגע נתייחס אליו כקיים
myGripper = new Gripper(motor);


// --- בתוך ה-Loop (הריצה) ---
// 3. שימוש באובייקט
if (gamepad1.a) {
    myGripper.open(); // קריאה לפעולה "פתח"
} else {
    myGripper.close(); // קריאה לפעולה "סגור"
}
```

-----

## למה זה טוב לנו? (היתרונות ב-FTC)

אולי זה נראה כמו יותר עבודה, אבל תראו איזה יופי הקוד הראשי שלנו נראה למעלה.

1.  **קריאות (Readability):** במקום לראות `set(1.0)` ולנחש מה המספר הזה אומר, אנחנו רואים `myGripper.open()`. זה כמעט אנגלית\!
2.  **סדר:** אם הצבת לא עובדת, אתם יודעים בדיוק לאיזה קובץ ללכת (`Gripper.java`) ולא צריכים לחפש בתוך אלף שורות קוד.
3.  **שכפול:** נניח שבשנה הבאה בונים רובוט עם שתי צבתות?
    ```java
    Gripper leftGripper = new Gripper(leftMotor);
    Gripper rightGripper = new Gripper(rightMotor);
    ```

-----

## סיכום

  * **Class (מחלקה):** הקובץ שמגדיר איך המנגנון עובד (השרטוט).
  * **Object (אובייקט):** המשתנה החי בזיכרון שמפעיל את המנגנון.
  * **Constructor:** הפעולה שבונה את האובייקט ומכניסה ערך לFields.
  * **Methods:** הפקודות שהמנגנון יודע לבצע.