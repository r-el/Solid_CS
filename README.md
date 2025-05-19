# Solid-CS
## תרגיל: שיפור והרחבת מערכת בקרת משימה

**מטרה**

אתם חלק מצוות פיתוח תוכנה הבונה מערכת בקרת משימה עבור סוכנות חלל בין-כוכבית. המערכת הנוכחית עובדת, אך היא מפרה מספר עקרונות של תכנון תוכנה טוב. משימתכם היא לשפר את הקוד הקיים כך שיעמוד בעקרונות הבאים:

* **עקרון האחריות היחידה (SRP)** – לכל מחלקה או מתודה צריכה להיות אחריות אחת בלבד.
* **עקרון הפתוח-סגור (OCP)** – קוד צריך להיות פתוח להרחבה, אך סגור לשינוי.
* **עקרון הפרדת הממשקים (ISP)** – לקוחות לא צריכים להיות תלויים בממשקים שהם לא משתמשים בהם.

תתחילו עם קטע קוד בעייתי ותתקנו אותו, ולאחר מכן תתכננו מודולים חדשים בעצמכם תוך שימוש באותם עקרונות.

**חלק 1: שיפור מודרך (תיקון ההפרות)**

להלן קטע קוד בעייתי:

```csharp
public class MissionControl
{
    public void LaunchRocket()
    {
        Console.WriteLine("Rocket launched!");
    }

    public void SendTelemetry()
    {
        Console.WriteLine("Telemetry sent to Earth...");
    }

    public void EmergencyShutdown()
    {
        Console.WriteLine("Emergency shutdown triggered!");
        File.WriteAllText("log.txt", "EMERGENCY OCCURRED");
        Email.Send("admin@spaceagency.com", "Emergency triggered");
    }
}

public static class Email
{
    public static void Send(string to, string message)
    {
        Console.WriteLine($"Sending email to {to}: {message}");
    }
}
```
משימות

**חלק 1: יישום SRP**

פצלו את המחלקה MissionControl למחלקות נפרדות עם אחריות יחידה.

כתיבת לוג ושליחת מיילים לא צריכות להיות חלק מ-MissionControl.

צרו לפחות 3 מחלקות נפרדות, לדוגמה: RocketLauncher, TelemetrySender, EmergencyHandler.

*שלב ב: יישום OCP*
נניח שבהמשך מתווספת דרישה חדשה: רישום לוג למסד נתונים במקום לקובץ.

אסור לכם לשנות את המחלקה EmergencyHandler בעת הוספת יכולת זו.

השתמשו בהפשטה (ממשקים או מחלקות בסיס) כדי לאפשר חיבור של לוגרים שונים.

הגדירו ממשק כמו ILogger, וממשו את FileLogger, DatabaseLogger.

*שלב ג: יישום ISP*

נניח שאנחנו רוצים להשתמש במנגנוני התראה שונים: מייל, SMS, בתוך האפליקציה.

ממשק ה-IAlert לא צריך לכפות את כל שיטות ההתראה לחוזה אחד.

פצלו את הממשקים כך שכל סוג התראה יוכל לשמש באופן עצמאי:

```csharp

public interface IEmailAlert { void SendEmail(string message); }
public interface ISmsAlert { void SendSms(string message); }
```

**חלק 2: תכנון עצמאי**

כעת שהמערכת מודולרית ומכבדת את עקרונות(SRP, OCP, ISP), הגיע הזמן להרחיב אותה.

תכננו פיצ'ר חדש: הוסיפו מערכת לניטור רמת החמצן והפעלת התראות כאשר היא נמוכה מדי.

אתם מחליטים:

כיצד לבנות את מבנה הקוד.
באילו ממשקים ומחלקות להשתמש.
כיצד ליישם את עקרונות (SRP, OCP, ISP)
דרישות

חיישן קורא את רמות החמצן.
אם רמת החמצן נמוכה מדי, המערכת רושמת את האירוע ושולחת התראות (מייל, SMS או אחרות).
ניטור החמצן צריך להיות מודולרי, כך שתוכלו לחבר בהמשך חיישן CO2 בלי לשנות את הלוגיקה המרכזית.

**תוצרים**

* קוד משופר עבור MissionControl (חלק 1).
* התכנון החדש שלכם עבור ניטור חמצן (חלק 2), כולל:
הגדרות מחלקות וממשקים.
* הסבר קצר כיצד התכנון שלכם מכבד את עקרונות SRP, OCP ו-ISP.
<!-- end list --> 
