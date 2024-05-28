
![image](https://github.com/noam-ben-lulu/minip_basnat/assets/128416447/708b6ede-de54-4220-ac07-a200576416dd)

## תיאור מילולי של המערכת 
מטרת התוכנית שאנו נבנה היא על מנת לאפשר לבנק לשלוט על מחלקת משאבי האנוש בבנק. 
התוכנית תאפשר לבנק לשלוט על ניהול העובדים שלו, ניהול תפקידי העובדים והמחלקות בהם הם נמצאים. 
בנוסף יתאפשר לעשות פיתוח מחלקות כאשר לכל יוזמה יהיה סוג יוזמה, מספר המחלקה, ותאריך היוזמה. 
בנוסף תיהיה אופציה לניהול משאבי אנוש עבור איש ספציפי במחלקה עם תיאור הפעולה הנעשית(כגון העלה בשכר ענישה וכדו). 
בעצם כמו שאמרנו בשער הפתיחה של הפרויקט אנחנו נתעסק בתוך ארגון גדול של בנק כאשר הפרויקט שלנו ייקח מחלקה מתוך הבנק וזה מחלקת משאבי אנוש על כל פרטיה עובדים מחלקות מעמדות משאבי אנוש ונוכחות

## ERD
![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/169597958/ac2f178e-73dc-4180-bb86-1638190a8fe6)


## Entities:

1.**Employees:**:

- **EmployeeID** (מזהה העובד, INT).
- First Name (שם פרטי, VARCHAR2(255)).
- Last Name (שם משפחה, VARCHAR2(255))
- PositionID FK (מזהה התפקיד, INT)
- Salary (משכורת, DECIMAL(10,2))
- DepartmentID FK (מזהה מחלקה, INT)

2.**Positions**:
- **PositionID** (מזהה התפקיד, INT)
- Position Name (כותרת התפקיד, VARCHAR2(255))
- Responsibilities (אחריות עיקרית, TEXT)
- Requirements (דרישות לתפקיד, TEXT)

3. **department**:
- **departmentID**(INT, מזהה ראשי)
- departmentName (VARCHAR2(255))
- Description(TEXT)

4.**Development**:   

- **DevelopmentID**(INT, מזהה ראשי)
- departmentID FK (INT, מזהה חיצוני לטבלת מחלקה)
- initiative_type(VARCHAR2(255))
- initiative_date(DATE)


5. **Human Resource Management**:
- **HRActionID** (מזהה פעולת משאבי אנוש, INT PRIMARY KEY)
- ActionType (סוג הפעולה, VARCHAR2(255))
- Date (תאריך, DATE)
- Description (תיאור, TEXT)
- EmployeeID FK (מזהה עובד, INT)


6. **Attendance**: 
- **EmployeeID** FK(מזהה העובד, INT PRIMARY KEY)
- **Aten_Date**(תאריך, DATE PRIMARY KEY)
- Status(VARCHAR2(255))




**Schemes**
- Employees( EmployeeID , PositionID, DepartmentID, First name, Last name, Salary).
- Positions(PositionID,PositionName, Responsibilities, Requirements)
- Departments(DepartmentID,DepartmentName, Description)
- Development(DevelopmentID,DepartmentID, InitiativeType, InitiativeDate)
- Human Resource Management(HRActionID,EmployeeID, ActionType, Date, Description)
- Attendance(EmployeeID, Aten_Date, Status)


## DSD
![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/169597958/ea6c9ed7-cba5-4858-8958-597ed428c851)

## נימוקים לעיצוב:
נורמליזציה: הטבלאות נורמליזציות כדי למנוע כפילות נתונים ולאפשר גישה ותחזוקה יעילים יותר של הנתונים.

קשרים לוגיים: המודל מגדיר קשרים ברורים בין הטבלאות באמצעות מפתחות זרים, מה שמבטיח שלמות נתונים והקפדה על מבנה לוגי ברור.

גמישות: המודל מאפשר גמישות בשינויים עתידיים במבנה הארגוני מבלי להשפיע משמעותית על מבנה הנתונים הקיים.

## CreatTable

CREATE TABLE Positions (

    PositionsID     NUMBER(38) NOT NULL,
    
    PositionsName  VARCHAR2(255) NOT NULL,
    
    Requirements   VARCHAR2(600) NOT NULL,
    
    CONSTRAINT pk_Positions PRIMARY KEY (PositionsID)
)
/

CREATE TABLE department (

    departmentID   NUMBER(38) NOT NULL,
    
    departmentName VARCHAR2(255) NOT NULL,
    
    Numofworkers   NUMBER(38) NOT NULL,
    
    CONSTRAINT pk_department PRIMARY KEY (departmentID)
    
)
/

CREATE TABLE Employee (

    EmployeeID     NUMBER(38) NOT NULL,
    
    PositionID     NUMBER(38) NOT NULL,
    
    First_Name     VARCHAR2(255) NOT NULL,
    
    Last_Name      VARCHAR2(255) NOT NULL,
    
    DepartmentID   NUMBER(38) NOT NULL,
    
    salary         NUMBER(38) NOT NULL,
    
    CONSTRAINT pk_Employee PRIMARY KEY (EmployeeID),
    
    CONSTRAINT fk_Employee_Position FOREIGN KEY (PositionID)
        REFERENCES Positions (PositionsID)
        ON DELETE CASCADE,
        
    CONSTRAINT fk_Employee_Department FOREIGN KEY (DepartmentID)
        REFERENCES department (departmentID)
        ON DELETE CASCADE
)
/

CREATE TABLE Development (

    DevelopmentID  NUMBER(38) NOT NULL,
    
    departmentID   NUMBER(38) NOT NULL,
    
    initiative_type VARCHAR2(255) NOT NULL,
    
    initiative_date DATE NOT NULL,
    
    CONSTRAINT pk_Development PRIMARY KEY (DevelopmentID),
    
    CONSTRAINT fk_Development FOREIGN KEY (departmentID)
        REFERENCES department (departmentID)
)
/

CREATE TABLE Human_Resource_Management (

    HRActionID     NUMBER(38) NOT NULL,
    
    ActionType     VARCHAR2(255) NOT NULL,
    
    HRA_Date       DATE NOT NULL,
    
    Description    VARCHAR2(600) NOT NULL,
    
    EmployeeID     NUMBER(38) NOT NULL,
    
    CONSTRAINT pk_Human_Resource_Management PRIMARY KEY (HRActionID),
    
    CONSTRAINT fk_Human_Resource_Management FOREIGN KEY (EmployeeID)
        REFERENCES Employee (EmployeeID)
        ON DELETE CASCADE
)
/



CREATE TABLE Attendance (

    EmployeeID     NUMBER(38) NOT NULL,
    
    Aten_Date      DATE NOT NULL,
    
    Status         VARCHAR2(255) NOT NULL,
    
    CONSTRAINT pk_Attendance PRIMARY KEY (EmployeeID, Aten_Date),

    CONSTRAINT fk_Attendance FOREIGN KEY (EmployeeID)
        REFERENCES Employee (EmployeeID)
)


/
![image](https://github.com/noam-ben-lulu/minip_basnat/assets/128416447/a4a4211c-a18a-453f-8388-624487184344)

## Data-Generator(screenshots)
![WhatsApp Image 2024-05-28 at 17 37 44](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/83bda7f1-91bf-4f7b-94ca-25c3b4c937d7)

![WhatsApp Image 2024-05-20 at 21 26 45](https://github.com/noam-ben-lulu/minip_basnat/assets/128416447/94e38e4d-1cca-4278-b4cd-e5f44b9adcdd)

![WhatsApp Image 2024-05-27 at 18 50 37](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/03049901-b4d1-46e7-a876-495b524c932c)

![WhatsApp Image 2024-05-27 at 18 47 35](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/a4c8a8d8-0067-420c-83b3-500adab1c194)



![WhatsApp Image 2024-05-20 at 21 27 37](https://github.com/noam-ben-lulu/minip_basnat/assets/128416447/eda0e882-2e07-4211-8e31-e225a96d81d5)
![WhatsApp Image 2024-05-28 at 17 40 18](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/5bb660a8-9507-4c16-a84f-adb06f8f83bd)



## Text Importer(from csv files)
the csv files created by mockaroo 
![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/127217c5-60ae-4bd9-9f57-36efcaf6f56f)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/14ed6743-4484-448c-a3a8-f9f6e3a0e87b)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/0678fee0-e4ae-4fe7-836a-71fecd20bb9c)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/03c92381-0eaf-4bfc-ae40-00769de19113)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/6fa97a5a-b51e-4dc7-90b5-a12c0f0a6af3)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/5ac67d52-bd5c-4a3f-b775-4bfbe57ed357)

## Backup and restore
![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/790c2549-4e72-4e8e-a323-dd619f26be67)


![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/e56972b4-c926-44ff-bc8b-6cd6100461c1)

   ## Restore
   ![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/ad6b9543-c569-40fc-8f0b-efdf036cbc21)
   ![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/c3d17467-074f-47cb-9853-29bc554fbe4b)

## שלב 2
 שאילתות ללא פרמטרים
 
   שאילתות select
  
 שאילתה 1:
 
 ![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/cc8faba7-262e-46ea-a642-eded9ab6300e)

 
 
 הסבר: 

 
 השאילתה מחזירה את שמות העובדים, המחלקה שלהם והמשכורת, ממיינת לפי שם המחלקה ולאחר מכן לפי המשכורת בסדר יורד 
 
 שאילתה 2: 

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/bd293a70-189c-4b32-a1c1-60b214e812f1)
 
 
הסבר:

השאילתה מחזירה את ימי הנוכחות של כל עובד ממוינת לפי ימי הנוכחות בסדר יורד ( כלומר בהתחלה נראה את כל מי שהיה מספר ימים הכי גבוה)

שאילתה 3:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/9a1da763-4914-40c1-aaa8-a8a8b00f2d7f)


הסבר:

השאילתה הזו מחזירה לנו בעצם את מספר העובדים שנכחו בכל חודש בכל שנה( לדוגמא כפי שניתן לראות בשנת 2020 בחודש ינואר (1) נכחו 6 עובדים וכן על זאת הדרך..)

שאילתה 4:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/bbf1f1bf-1f48-4719-b941-8ecb666632bb)


הסבר:

השאילתה בעצם מראה לנו את ממוצע המשכורות של כל מחלקה התוצאה היא שם המחלקה מספר העובדים בה וממוצע המשכורות של העובדים במחלקה

שאילתות delete:

בהתחלה כדי לראות את מסד הנתונים לפני ואחרי כלומר כדי לראות שבאמת הפעולה בוצעה נריץ שאילתה כדי להראות את הבסיס לאחר מכן נבצע את השאילתה שלנו ולאחר מכן נריץ את אותה שאילתה כדי לודא שבאמת היה מחיקה ממסד הנתונים.

שאילתה 1:
 מחיקת כל העובדים שלא ביצעו שום יוזמה (initiative) בטבלת Development

 בדיקת הנתונים:

 ![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/4e5cecdd-5227-4473-b309-2839c45b7afd)

מחיקת העובדים האלו (נשים לב שקיימים 175 כאלו שלא ביצעו שום יוזמה)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/247b105f-fc98-44d2-a352-c90394b73c0c)

נשים לב שביצענו מחיקה גם מטבלת הנוכחות מפני שהתעודת זהות של העובדים היא מפתח זר בטבלה הזו

בדיקה שהמחיקה התקיימה כראוי:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/6ecbb590-377c-48ec-a404-5201c5d9f7b0)

באמת אפשר לראות שלא קיימים עכשיו עובדים כאלו כלומר המחיקה בוצעה כראוי.

שאילתה 2:

מחיקת כל העובדים שאין לנו עליהם רישום בטבלת נוכחות שלנו

בדיקת הנתונים:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/e59e746d-1272-40aa-b51c-020c9eb48c7c)

מחיקת העובדים האלו (נשים לב שקיימים 90 כאלו שאין לנו עליהם רישום בטבלת נוכחות)

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/9e0c4a47-304c-4875-9839-50ad496a1f86)

בדיקה שהמחיקה התקיימה כראוי:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/df6ee7e9-2bdd-4a65-a80e-e8a2987b08c6)

באמת אפשר לראות שהמחיקה התקיימה כראוי

שאילתות update:

גם כאן נבצע שאילתה להראות את מסד הנתונים לפני נעשה שאילתת עדכון ולאחר מכן שאילת להראות את מסד הנתונים לאחר העדכון

שאילתה 1: עדכון משכורת של עובדים במחלקת HR ב-10% בעקבות עבודתם הקשה בפרויקט

בדיקת הנתונים:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/52a0f103-f5e3-497a-8b9b-8099d09c32f3)

ביצוע העדכון:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/02a0b737-c49e-4f89-b589-c0edfeef2266)

בדקית העדכון:

![image](https://github.com/noam-ben-lulu/DBProject6850-8904/assets/128416447/44e43038-2a98-45b2-9fba-98b8310abbec)
נסתכל על לדוגמא העובד הראשון בהתחלה המשכורת שלו הייתה 
