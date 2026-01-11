## 🧑‍💻 Assets & Identities

أثناء شيفت الليل، ممكن يجيلك **Alert** بيقول إن المستخدم **G.Baker** عمل **Login** على السيرفر  
**HQ-FINFS-02**، وبعد كده:

- نزل ملف اسمه **"Financial Report US 2024.xlsx"**
    
- وبعته للمستخدم **R.Lund**
    

علشان تعمل **Alert Triage** صح وتحدد هل النشاط ده طبيعي ولا مشبوه، لازم تجاوب على شوية أسئلة مهمة:

### أسئلة لازم تتسأل

- **Who is G.Baker?**
    
    - وظيفته إيه؟
        
    - بيشتغل في أنهي فريق؟
        
    - إيه **Working Hours** بتاعته؟
        
- **What is HQ-FINFS-02?**
    
    - ده سيرفر إيه؟ (Finance / File Server)
        
    - موجود فين؟
        
    - مين عنده **Access** عليه؟
        
- **Why R.Lund needs the file?**
    
    - هل شغله له علاقة بالـFinance؟
        
    - هل في سبب مشروع لمشاركة الملف؟
        

📌 الإجابة على الأسئلة دي بتحدد إذا كان:

- **Expected Activity**
    
- أو **Suspicious Activity**
    

---

## 🗂️ Identity Inventory

**Identity Inventory** هو:

> Catalogue (قائمة) فيها كل **Identities** جوه الشركة

وبيشمل:

- **User Accounts** (موظفين)
    
- **Service Accounts** (خدمات)
    
- **Machine Accounts** (أجهزة)
    

### البيانات اللي بتكون موجودة

- Role
    
- Privileges
    
- Department
    
- Contact Information
    
- Working Hours
    

---

## 🧠 ليه Identity Inventory مهم؟

في السيناريو اللي فوق:

- يساعدك تفهم:
    
    - هل **G.Baker** عنده صلاحية يدخل على **HQ-FINFS-02**
        
    - هل الوقت ده داخل **Working Hours** ولا لا
        
    - هل مشاركة الملف مع **R.Lund** منطقية ولا لأ
        

بدون **Identity Inventory**:

- أي Alert هيبان مشبوه
    
- ومش هتعرف تاخد قرار صح
    

📌 باستخدامه، تقدر:

- تقلل **False Positives**
    
- وتحدد **True Positives** بدقة
## 🖥️ Asset Inventory

**Asset Inventory** (وأحيانًا بتتسمى **Asset Lookup**) هي:

> قائمة بكل **Computing Assets** الموجودة داخل بيئة الـIT في الشركة.

في السياق ده، كلمة **Asset** المقصود بيها:

- **Servers**
    
- **Workstations**
    

❌ مش المقصود هنا:

- Software
    
- Employees
    
- Licenses
    

---

## 🧠 ليه Asset Inventory مهم في SOC؟

في المثال السابق:

- عندك Alert بيقول إن في نشاط على السيرفر **HQ-FINFS-02**
    
- علشان تحدد هل النشاط ده طبيعي ولا لأ، لازم تعرف:
    
    - السيرفر ده بيعمل إيه؟
        
    - تابع لأنهي Department؟
        
    - مين المفروض يدخل عليه؟
        

📌 هنا بييجي دور **Asset Inventory** لأنه بيديك **Context** عن السيرفر أو الجهاز.

---

## 📌 معلومات بتوفرها Asset Inventory

عادةً بتلاقي فيها:

- Asset Name (مثال: HQ-FINFS-02)
    
- Asset Type (Server / Workstation)
    
- Operating System
    
- Location (HQ / Cloud / Branch)
    
- Owner / Department
    
- Criticality (High / Medium / Low)
    
- Allowed Users or Groups
    

---

## 🗄️ Sources of Assets

دي أشهر المصادر اللي SOC بيستخدمها علشان يبني **Asset Inventory**:

### 1️⃣ Active Directory (AD)

**Examples:**

- On-prem AD
    
- Entra ID
    

**Description:**

- AD مش بس **Identity Management**
    
- كمان يعتبر **Asset Inventory** قوي لأنه بيحتوي على:
    
    - Computer Objects
        
    - Server Names
        
    - Domain Membership
        

---

### 2️⃣ SIEM or EDR

**Examples:**

- Elastic
    
- CrowdStrike
    

**Description:**

- بعض **SIEM / EDR Agents** بتجمع:
    
    - Host Information
        
    - OS Details
        
    - IP Addresses
        
- وده بيساعد في ربط الـAlert بالجهاز مباشرة
    

---

### 3️⃣ MDM Solution

**Examples:**

- Microsoft Intune
    
- Jamf MDM
    

**Description:**

- حلول متخصصة لإدارة الأجهزة
    
- بتستخدم غالبًا مع:
    
    - Laptops
        
    - Mobile Devices
        
- بتدي رؤية كاملة عن حالة الجهاز وسياساته
    

---

### 4️⃣ Custom Solution

**Examples:**

- CSV Files
    
- Excel Sheets
    

**Description:**

- بعض الشركات الصغيرة أو المتوسطة
    
- بتستخدم حلول Manual
    
- زي جداول Excel لتسجيل الأجهزة
    

⚠️ لكنها:

- أقل دقة
    
- وممكن تكون Outdated


