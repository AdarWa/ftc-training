# פרק 5: לולאת ה-While - הלב הפועם של הרובוט

עד עכשיו, דמיינו שהקוד שלנו רץ שורה אחרי שורה ונעצר.
אבל רובוט אמיתי לא עובד ככה. אם נגיד לרובוט "סע קדימה", הוא יסע למאית שנייה ויפסיק.
אנחנו רוצים שהרובוט **ימשיך** להגיב לנו, **ימשיך** לבדוק את הג'ויסטיק, ו**ימשיך** לעדכן את המנועים כל הזמן.

בשביל זה יש לנו את לולאת ה-**`while`**.

## מה זה While? 🔄

המילה `while` באנגלית משמעותה "כל עוד".
הפקודה הזו אומרת למחשב:
**"כל עוד התנאי שבסוגריים הוא 'אמת' (true) - תבצע את הפקודות שבפנים שוב ושוב ושוב."**

### התחביר (Syntax)

זה נראה כמעט כמו `if`, אבל עובד במעגלים:

```java
while ( התנאי_שלנו ) {
    
    // גוף הלולאה
    // הקוד כאן ירוץ שוב ושוב
    // כל עוד התנאי למעלה נשאר "אמת"
    
}
```

-----

## המבנה של משחק רובוטיקה (The Game Loop)

ב-FTC (ובכל משחק מחשב שאתם מכירים), הקוד בנוי משני שלבים עיקריים:

1.  **ההכנה (Setup):** קורה פעם אחת בלבד בהתחלה. כאן בונים את האובייקטים ומכינים את הרובוט.
2.  **הלולאה (The Loop):** קורה אלפי פעמים בשנייה. כאן הרובוט "חי".

בואו נראה איך זה נראה בקוד אבסטרקטי (כללי), בלי להסתבך עם הפקודות המסובכות של ה-FTC כרגע.

נשתמש במשתנה דמיוני שנקרא `matchIsRunning` (האם המשחק רץ). כל עוד הוא `true`, הרובוט עובד. ברגע שהשופט שורק לסיום, הוא הופך ל-`false` והרובוט עוצר.

### הדוגמה המלאה: יום בחייו של רובוט

נשתמש במחלקות שיצרנו בפרקים הקודמים (`Gripper`, `DriveTrain`).

```java
public class MyRobotProgram {

    public void runRobot(){
        // --- שלב 1: ההכנה (לפני שהמשחק מתחיל) ---
        // הקוד כאן רץ רק פעם אחת!
        
        // יצירת האובייקטים (החלקים של הרובוט)
        DriveTrain wheels = new DriveTrain();
        Gripper claw = new Gripper();
        
        // משתנה שבודק אם המשחק פעיל (במציאות זה מגיע הDriverHub)
        boolean matchIsRunning = true; 

        // --- שלב 2: הלולאה (המשחק עצמו) ---
        // נכנסים ללולאה שתרוץ כל עוד המשחק פעיל
        
        while (matchIsRunning == true) {
            
            // --- א. קליטה (SENSE) ---
            // בודקים מה הנהג עושה כרגע
            double speedInput = gamepad.left_stick_y; // כמה מהר לנסוע?
            boolean grabButton = gamepad.a;           // האם כפתור A נלחץ?
            
            // --- ב. חשיבה (THINK) ---
            // מחליטים מה לעשות עם המידע
            
            // טיפול בצבת:
            if (grabButton == true) {
                claw.close(); // אם לחצו - סגור
            } else {
                claw.open();  // אחרת - פתח
            }
            
            // --- ג. פעולה (ACT) ---
            // שולחים את הפקודות למנועים
            wheels.drive(speedInput);
            
            
            // ...ואז המחשב חוזר מיד להתחלה של ה-while
            // לבדוק שוב את הג'ויסטיק!
        }
        
        // --- שלב 3: סיום ---
        // אם יצאנו מהלולאה, סימן שהמשחק נגמר
        wheels.stop();
    }
}
```

-----

## למה זה כל כך חשוב?

תחשבו מה היה קורה אם **לא** הייתה לנו לולאה, והיינו כותבים רק:

```java
if (gamepad.a) {
    claw.open();
}
```

הרובוט היה מפעיל את התוכנה, בודק את הכפתור בשבריר השנייה הראשונה (כשאף אחד עוד לא הספיק ללחוץ), רואה שהוא לא לחוץ, ומסיים את התוכנית. הרובוט היה "מת" מיד.

**הלולאה מאפשרת לרובוט להיות "קשוב".** בכל "סיבוב" של הלולאה (שנקרא איטרציה), הרובוט בודק: "האם משהו השתנה? האם הנהג הזיז את הסטיק עכשיו? ומה עכשיו? ומה עכשיו?".

-----

## סכנה: הלולאה האינסופית (Infinite Loop) ⚠️

מה קורה אם התנאי תמיד נשאר `true` ולעולם לא משתנה?
התוכנה "נתקעת" בתוך הלולאה ולעולם לא מגיעה לשורות שאחריה.

ב-FTC זה דווקא **בסדר גמור** בתוך הקטע של הנהיגה - אנחנו רוצים להיתקע שם עד שהמשחק נגמר. המערכת החיצונית (האפליקציה בDriverHub) היא זו שתהרוג את הלולאה בכוח כשהזמן ייגמר.

אבל, היזהרו מלולאות `while` קטנות בתוך הלולאה הראשית\!
**דוגמה רעה ומסוכנת:**

```java
while (matchIsRunning) {
    // ... נהיגה רגילה ...
    
    // טעות חמורה!
    // אם נכתוב פה: "חכה עד שהחיישן יראה קיר"
    while (sensor.distance > 10) {
        // הרובוט תקוע כאן!
        // הוא לא בודק ג'ויסטיקים, לא מגיב לכלום
        // עד שהוא יגיע לקיר. ומה אם הוא לא יגיע בחיים?
        // הרובוט קפא.
    }
}
```

**הכלל:** הלולאה הראשית צריכה לרוץ מהר מאוד. אל תעצרו אותה.

-----

## סיכום

1.  **Setup (אתחול):** קורה פעם אחת לפני ה-`while`. שם בונים את המחלקות.
2.  **Loop (לולאה):** ה-`while` הוא המנוע שמריץ את הקוד שוב ושוב.
3.  **Sense-Think-Act:** בתוך הלולאה אנחנו קוראים חיישנים/ג'ויסטיק, מקבלים החלטות (`if`), ומפעילים שיטות של מחלקות.