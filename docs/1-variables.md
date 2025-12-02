# פרק 1: הבסיס ומשתנים

ברוכים הבאים לעולם ה-Java\!
ב-FTC, שפת Java היא הדרך שלנו "לדבר" עם הרובוט. הרובוט הוא צייתן מאוד, אבל הוא לא מבין רמזים - הוא צריך שנכתוב לו הוראות מדויקות.

## המבנה הכללי: ה"בית" של הקוד

לפני שנכתוב שורת קוד אחת, צריך להבין איפה כותבים אותה.
בשפת Java, כל הקוד שלנו חי בתוך **מחלקות** (Classes), וכל הפקודות תחומות בתוך סוגריים מסולסלים `{ }`, כרגע אין צורך להבין מה זה אומר מחלקה\! נדבר על זה בהרחבה בהמשך.

תחשבו על הסוגריים `{ }` כמו הקירות של החדר. מה שבתוך הסוגריים שייך לחדר הזה, ומה שבחוץ לא קיים עבורו.

```java
public class MyRobotCode {
    // כאן אנחנו בתוך ה"בית" של הקוד
    // כאן נגדיר את הזיכרון של הרובוט
}
```

> **הערה:** הסימן `//` אומר "הערה". המחשב מתעלם מכל מה שכתוב אחריו. זה נועד בשבילנו, בני האדם, כדי שנזכור מה עשינו.

-----

## משתנים (Variables): הזיכרון של הרובוט

כדי שהרובוט יוכל לעשות דברים חכמים, הוא צריך לזכור נתונים. למשל: באיזו מהירות לנסוע? כמה זמן עבר? האם הכפתור נלחץ?
בשביל זה יש לנו **משתנים**.

### משתנה הוא קופסה 📦

תחשבו על משתנה כמו על **קופסת אחסון**.
לכל קופסה יש שלושה מאפיינים:

1.  **סוג (Type):** איזו צורה יש לקופסה? (האם היא נועדה להחזיק מספרים? טקסט? אמת\שקר?)
2.  **שם (Name):** מה כתוב על המדבקה של הקופסה? (כדי שנוכל למצוא אותה).
3.  **ערך (Value):** מה שמנו בתוך הקופסה?

-----

## סוגי המשתנים החשובים ל-FTC

ב-Java יש המון סוגים, אבל ברובוטיקה אנחנו משתמשים בעיקר בארבעה:

### 1\. מספר שלם (`int`)

קיצור של Integer. משמש למספרים ללא נקודה עשרונית.

  * **למה זה טוב ברובוט?** לספור דברים. מספר הקפות, מספר כדורים שאספנו, זמן.

### 2\. מספר עשרוני (`double`)

משמש למספרים מדויקים עם נקודה (כמו 0.5, 3.14).

  * **למה זה טוב ברובוט?** **זה הסוג הכי חשוב\!** כוח מנוע (בין 0.0 ל-1.0), קריאות מחיישני מרחק, מיקום של מנוע.

### 3\. בוליאני (`boolean`)

משתנה לוגי שיכול להיות רק אחד משני מצבים: `true` (אמת) או `false` (שקר).

  * **למה זה טוב ברובוט?** מתגים וחיישני מגע (Touch Sensor). האם הכפתור לחוץ? (כן/לא). האם הרובוט סיים את המסלול?

### 4\. מחרוזת טקסט (`String`)

רצף של תווים ומילים. תמיד נכתוב את הערך בתוך גרשיים `" "`.

  * **למה זה טוב ברובוט?** כדי לשלוח הודעות לטלפון של הנהג (Telemetry), כמו "Robot is Ready".

-----

## איך יוצרים משתנה? (תחביר)

הנוסחה ליצירת משתנה היא קבועה:
`נקודה-פסיק` + `ערך` = `שם`  `סוג`

```java
// דוגמה כללית
Type name = value;
```

> **חוק:** בסוף כל פקודה ב-Java חייבים לשים נקודה-פסיק (`;`). זה כמו הנקודה בסוף משפט. אם תשכחו את זה - הקוד לא יעבוד\!

### דוגמאות (FTC)

הנה איך זה נראה בקוד אמיתי:

```java
// יצירת משתנה למספר שלם - כמה כדורים אספנו?
int ballsCollected = 3;

// יצירת משתנה עשרוני - באיזה כוח המנוע צריך לפעול? (חצי כוח)
double motorPower = 0.5;

// יצירת משתנה בוליאני - האם חיישן המגע לחוץ?
boolean isTouchSensorPressed = false;

// יצירת משתנה טקסט - הודעה לנהג
String statusMessage = "Waiting for Start";
```

-----

## חוקים למתן שמות (Naming Conventions)

איך קוראים למשתנים?

1.  **באנגלית בלבד.**
2.  **משמעותי:** אל תקראו למשתנה `x` או `a`. תקראו לו `motorSpeed` או `armPosition`. זה יציל אתכם כשתהיו בלחץ בתחרות.
3.  **שיטת הגמל (camelCase):** מתחילים באות קטנה, וכל מילה חדשה מתחילה באות גדולה (בלי רווחים).
      * לא טוב: `motorpower` (קשה לקרוא)
      * לא טוב: `MotorPower` (מתחיל באות גדולה - שמור למחלקות)
      * מצויין: `motorPower` 🐪

-----

## סיכום

עד עכשיו למדנו איך **ליצור** את הקופסאות ולשים בהן ערך התחלתי.
הנה דוגמה מלאה שמסכמת את הדף:

```java
public class RobotVariables {
    
    // הגדרת משתנים לרובוט שלנו
    double leftMotorPower = 0.8;   // כוח למנוע שמאל
    double servoPosition = 0.0;    // סרוו במצב סגור
    int targetDistance = 1200;     // מרחק יעד בטיקים של האנקודר
    boolean isGripperOpen = true;  // האם הצבת פתוחה?
    String teamName = "Apollo";    // שם הקבוצה
    
}
```