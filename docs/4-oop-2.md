# פרק 4: האנטומיה של מחלקות (ממברים, פונקציות והרשאות)

עכשיו כשאנחנו יודעים שכל הקוד שלנו חי בתוך מחלקות (Classes), הגיע הזמן להבין איך מסדרים את הריהוט בתוך הבית הזה.
מחלקה ב-Java מורכבת משני דברים עיקריים:

1.  **ממברים/שדות (Members/Fields):** הנתונים.
2.  **פונקציות/שיטות (Methods):** הפעולות.

אבל לפני שנצלול אליהם, צריך לדבר על "מי מורשה לגעת במה".

-----

## 1\. הרשאות גישה (Access Modifiers) 🔒

ב-Java, אנחנו יכולים להחליט מי רואה כל משתנה וכל פונקציה. זה נקרא **Encapsulation** (כימוס), וזה קריטי כדי שהקוד יעבוד ויהיה קריא וברור.

יש לנו שתי מילים עיקריות שמתחילות כמעט כל שורה ב-Java:

### `public` - ציבורי

  * **המשמעות:** כולם יכולים לראות את זה, כולם יכולים להשתמש בזה.
  * **מתי משתמשים?** בפעולות שאנחנו רוצים שהנהג או הקוד הראשי יפעילו (כמו `drive()`, `shoot()`).

### `private` - פרטי

  * **המשמעות:** רק הקוד שנמצא **בתוך המחלקה הזו** יכול לגעת בזה. מבחוץ? זה כאילו לא קיים.
  * **מתי משתמשים?** במשתנים הפנימיים של הרובוט (כמו המנוע עצמו, או משתנה זיכרון פנימי).

-----

## 2\. ממברים / שדות (Fields) 💾

אלו המשתנים שמוגדרים **בראש המחלקה** (מחוץ לכל הפונקציות).
הם הזיכרון לטווח ארוך של האובייקט. כל עוד האובייקט קיים, המשתנים האלו זוכרים את הערך שלהם.

**חוק:** ב-99% מהמקרים ב-FTC, המשתנים האלו יהיו `private`.

```java
public class Shooter {
    // --- שדות / ממברים ---
    
    // פרטי: המנוע עצמו (אנחנו לא רוצים שיגעו בו ישירות)
    private Motor shooterMotor; 
    
    // פרטי: מהירות הירי (ברירת מחדל)
    private double targetSpeed = 0.8; 
}
```

-----

## 3\. פונקציות / שיטות (Methods) ⚙️

הפונקציה היא "מכונה" קטנה שעושה פעולה אחת מוגדרת.
לכל פונקציה יש מבנה קבוע (חתימה):

`שם הפונקציה`  `מה היא מחזירה`  `הרשאה`  `(פרמטרים)`

### א. סוג ההחזרה (Return Type) - מה יוצא מהמכונה?

כל פונקציה חייבת להצהיר מה היא נותנת לנו בסוף.

  * **`void` (ריק):** הפונקציה עושה משהו (מזיזה מנוע), אבל לא מחזירה תשובה.
  * **סוג משתנה (`int`, `boolean`, `double`):** הפונקציה עושה חישוב ומחזירה תשובה (למשל: בודקת אם החיישן לחוץ ומחזירה `true` או `false`).

### ב. פרמטרים (Parameters) - מה נכנס למכונה?

בתוך הסוגריים `()` אנחנו מגדירים מה הפונקציה צריכה לקבל כדי לעבוד.

  * פונקציה יכולה לקבל כלום: `()`
  * או לקבל נתונים: `(double speed, int time)`

### דוגמה למבנה פונקציה:

```java
//   1.הרשאה    2.החזרה   3.שם     4.פרמטרים (קלט)
    public     void     setSpeed (double newSpeed) {
        
        // גוף הפונקציה
        shooterMotor.set(newSpeed);
        
    }
```

-----

## אוסף דוגמאות מפורטות 🧪

הדרך הכי טובה להבין היא לראות את זה בעיניים. הנה דוגמאות למנגנונים נפוצים ב-FTC.

### דוגמה 1: מנגנון פשוט (פעולות `void`)

מחלקה שמייצגת זרוע פשוטה. היא לא מחזירה תשובות, רק מבצעת הוראות.

```java
public class ArmSystem {
    
    // שדה פרטי - המנוע
    private Motor armMotor;

    // בנאי (זוכרים? מחבר את החומרה)
    public ArmSystem(Motor armMotor) {
        this.armMotor = armMotor;
    }

    // פונקציה 1: להרים למעלה
    // public: כי אנחנו רוצים להשתמש בזה
    // void: כי זה רק מעשה, אין תשובה
    // (): לא צריך פרמטרים, זה תמיד אותו כוח
    public void raise() {
        armMotor.set(0.5);
    }

    // פונקציה 2: להוריד למטה
    public void lower() {
        armMotor.set(-0.3); // כוח חלש יותר בירידה
    }

    // פונקציה 3: לעצור
    public void stop() {
        armMotor.set(0);
    }
}
```

### דוגמה 2: פונקציה שמקבלת פרמטרים

כאן אנחנו רוצים שליטה מדויקת יותר. במקום סתם "סע", נגיד "סע בכוח X".

```java
public class DriveTrain {
    private Motor leftMotor;
    private Motor rightMotor;
    
    public DriveTrain(Motor leftMotor, Motor rightMotor){
        this.leftMotor = leftMotor;
        this.rightMotor = rightMotor;
    }

    // הפונקציה הזו דורשת מאיתנו לתת לה מספר (power)
    public void driveForward(double power) {
        // אנחנו משתמשים בפרמטר 'power' שנכנס לפונקציה
        leftMotor.set(power);
        rightMotor.set(power);
    }
    
    // שימוש בפונקציה הזו יראה ככה:
    // robot.driveForward(0.8);
    // robot.driveForward(0.2);
}
```

### דוגמה 3: פונקציה שמחזירה ערך (`return`)

זה קריטי לחיישנים. אנחנו שואלים את הרובוט שאלה, והוא חייב לענות.

```java
public class SensorSystem {
    private TouchSensor limitSwitch;
    
    public SensorSystem(TouchSensor limitSwitch){
        this.limitSwitch = limitSwitch;
    }

    // שימו לב! כתוב כאן boolean במקום void
    // זה אומר שהפונקציה חייבת להחזיר אמת או שקר בסוף
    public boolean isPressed() {
        
        // בדיקה מול החיישן
        if (limitSwitch.isPressed() == true) {
            return true; // החזרת התשובה "כן"
        } else {
            return false; // החזרת התשובה "לא"
        }
    }
}
```

### דוגמה 4: הכל ביחד (לוגיקה חכמה)

פונקציה חכמה שמשתמשת גם בפרמטרים וגם בהחזרה.

```java
public class BatteryMonitor {
    private double currentVoltage; // המתח הנוכחי

    private Robot robot;

    public BatteryMonitor(Robot robot){
        this.robot = robot;
    }

    // פונקציה שבודקת האם הסוללה חלשה מדי
    // מקבלת: מהו הגבול התחתון (limit)
    // מחזירה: אמת/שקר (האם אנחנו בסכנה?)
    public boolean isLowBattery(double limit) {
        
        // קריאה מהחיישן (קוד דמיוני לצורך הדוגמה)
        currentVoltage = robot.getRobotVoltage(); 
        
        // בדיקה
        if (currentVoltage < limit) {
            return true; // הסוללה נמוכה מהגבול שביקשת!
        } else {
            return false; // הכל בסדר
        }
    }
}
```

-----

## סיכום

1.  **שדות (`private` variables):** הנתונים של המחלקה. מוחבאים בפנים.
2.  **פונקציות (`public` methods):** הפעולות שהמחלקה יודעת לעשות.
3.  **`void`:** פונקציה שעושה עבודה וזהו.
4.  **`return`:** פונקציה שמחזירה תשובה/ערך.
5.  **פרמטרים `(...)`:** המידע שהפונקציה צריכה כדי לעבוד.