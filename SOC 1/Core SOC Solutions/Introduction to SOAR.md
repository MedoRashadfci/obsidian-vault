
## How Traditional SOCs Work

## إزاي الـ SOC التقليدي بيشتغل؟

### ما هو الـ SOC؟

الـ **Security Operations Center (SOC)** هو:

- مكان مركزي داخل المؤسسة
    
- مسؤول عن:
    
    - مراقبة الأنظمة
        
    - اكتشاف الهجمات
        
    - الاستجابة للحوادث الأمنية
        

الـ SOC اتطور على مراحل (Generations):

- كل جيل بيضيف أدوات وتقنيات جديدة
    
- الهدف الأساسي ثابت: **حماية أصول الشركة الرقمية**
    

---

### ليه وجود SOC مهم؟

الميزة الأساسية لوجود SOC في أي مؤسسة هي:

- تحسين التعامل مع الحوادث الأمنية
    
- المراقبة المستمرة (24/7)
    
- التحليل السريع لأي نشاط مش طبيعي
    

وده بيتم عن طريق:

- أشخاص (People)
    
- إجراءات (Processes)
    
- تقنيات (Technologies)
    

وكل ده لازم يكون:

- مناسب لقدرات الـ SOC
    
- متماشي مع أهداف الشركة
    

---

## القدرات الأساسية لأي SOC

### 1️⃣ Monitoring and Detection

### المراقبة والاكتشاف

- SOC بيراقب الشبكة بشكل مستمر
    
- الهدف:
    
    - اكتشاف أي نشاط مشبوه
        
    - منع الهجمات بدري قبل ما تكبر
        

🛠️ الأداة الأساسية هنا:

- **SIEM (Security Information and Event Management)**
    

#### أمثلة:

- عدد كبير من محاولات تسجيل الدخول الفاشلة
    
- تسجيل دخول من دولة أو مكان غير معتاد
    
- نشاط غير طبيعي على جهاز مهم (Critical Workstation)
    

---

### 2️⃣ Recovery and Remediation

### التعافي والمعالجة

لما يحصل Incident:

- الـ SOC بيكون **أول المستجيبين (First Responders)**
    

المهام:

- عزل الأجهزة المصابة
    
- إيقاف العمليات الخبيثة
    
- إزالة البرمجيات الضارة (Malware)
    
- تقليل الضرر قدر الإمكان
    

🛠️ الأدوات المستخدمة:

- EDR
    
- Firewalls
    
- IAM
    
- أدوات أمنية أخرى
    

#### أمثلة:

- عزل جهاز باستخدام EDR
    
- حظر IP على Firewall
    
- تعطيل مستخدم من IAM
    

---

### 3️⃣ Threat Intelligence

### استخبارات التهديدات

- SOC محتاج باستمرار:
    
    - معلومات حديثة عن التهديدات
        
- زي:
    
    - IPs خبيثة
        
    - Domains
        
    - Hashes
        
    - Indicators of Compromise (IOCs)
        

الهدف:

- منع الهجمات قبل ما تحصل
    
- تحسين دقة الاكتشاف
    

#### مثال:

- حظر دومين تم تصنيفه خبيث من Threat Feed
    

---

### 4️⃣ Communication

### التواصل

الـ SOC مش بيشتغل لوحده:

- لازم يتواصل مع:
    
    - فريق الـ IT
        
    - الإدارة
        
    - فرق أخرى
        

الهدف:

- التنسيق
    
- ضمان حل المشكلة بشكل كامل
    

#### مثال:

- فتح Ticket لفريق الـ IT لمراجعة Patch جديد
    
- إبلاغ الإدارة بحادث أمني مهم
    

---

## الخلاصة عن عمل الـ SOC

- SOC يستخدم أدوات كثيرة
    
- بيتعامل مع فرق مختلفة
    
- العمليات دي بتحمي المؤسسة  
    لكن… 👇
    

---

## Challenges Faced by SOCs

## التحديات اللي بتواجه الـ SOC

### 1️⃣ Alert Fatigue

### إرهاق التنبيهات

- كثرة الأدوات = كثرة Alerts
    
- نسبة كبيرة:
    
    - False Positives
        
    - Alerts ناقصة معلومات
        

🔴 النتيجة:

- المحللين بيتشتتوا
    
- صعب تميّز الهجوم الحقيقي وسط الزحمة
    
- حوادث خطيرة ممكن تتفوت
    

---

### 2️⃣ Too Many Disconnected Tools

### أدوات كتير ومش متصلة ببعض

- كل أداة شغالة لوحدها:
    
    - Firewall Logs
        
    - EDR Logs
        
    - IAM Logs
        

❌ مفيش Integration  
❌ مفيش رؤية موحدة

🔴 النتيجة:

- المحلل يضطر:
    
    - يفتح أكتر من Tool
        
    - يربط الأحداث يدويًا
        
- ضياع وقت ومجهود
    

---

### 3️⃣ Manual Processes

### العمليات اليدوية

- إجراءات التحقيق:
    
    - مش موثقة
        
    - معتمدة على خبرة أشخاص معينين (Tribal Knowledge)
        

❌ مفيش Playbooks واضحة  
❌ كل Analyst يشتغل بطريقته

🔴 النتيجة:

- بطء في الاستجابة
    
- زيادة MTTR
    
- أخطاء بشرية
    

---

### 4️⃣ Talent Shortage

### نقص الكفاءات

- صعب تلاقي:
    
    - SOC Analysts مؤهلين
        
- التهديدات بتزيد تعقيدًا
    
- عدد الـ Alerts بيزيد
    

🔴 النتيجة:

- ضغط رهيب على المحللين
    
- إرهاق نفسي وذهني
    
- استجابة أبطأ
    
- المهاجم ياخد وقت أطول جوه الشبكة



## أولًا: المشكلة الأساسية في الـ SOC

قبل ما نفهم SOAR، لازم نفهم **ليه أصلًا اتعمل**.

داخل أي **Security Operations Center (SOC)** بيكون عندك مشاكل كبيرة، أهمها:

1. **عدد تنبيهات ضخم (Alert Fatigue)**  
    مئات أو آلاف الـ alerts يوميًا (VPN, Malware, Phishing, Brute Force…).
    
2. **التعامل اليدوي مع أدوات كتير**  
    الـ Analyst بيقعد يفتح:
    
    - SIEM
        
    - EDR
        
    - Firewall
        
    - Threat Intelligence
        
    - IAM
        
    - Ticketing System
        
    
    وده بياخد وقت + مجهود + أخطاء بشرية.
    
3. **بطء الاستجابة**  
    الهجوم ممكن يكبر وإنت لسه بتتنقل بين الأدوات.
    
4. **توثيق ضعيف**  
    ناس تنسى تفتح Ticket أو تكتب تفاصيل ناقصة.
    

---

## ثانيًا: ما هو SOAR بالضبط؟

**SOAR = Security Orchestration, Automation, and Response**

هو **منصة واحدة** بتجمع كل أدوات الأمن السيبراني اللي في الـ SOC وتخليها تشتغل مع بعض **بشكل منظم وأوتوماتيك**.

يعني بدل ما الـ Analyst:

- يفتح 10 أدوات
    
- يعمل 20 Click
    
- يكرر نفس الخطوات كل مرة
    

👉 SOAR يعمل ده كله **نيابة عنه**.

---

## الميزة الأساسية في SOAR

SOAR بيعمل 3 حاجات رئيسية (وده سر قوته):

---

# 1️⃣ Orchestration (التنسيق)

### معناها إيه؟

تنظيم وربط كل أدوات الأمن مع بعض داخل منصة واحدة.

### مثال واقعي (VPN Brute Force):

من غير SOAR:

- تدخل SIEM تشوف اللوجز
    
- تفتح Threat Intel تشوف IP
    
- تدخل IAM توقف اليوزر
    
- تدخل Ticketing تفتح Incident
    

### مع SOAR:

كل الأدوات دي **متوصلة ببعض**.

### Playbooks

SOAR بيشتغل باستخدام **Playbooks**  
ودي عبارة عن:

> سيناريو جاهز فيه خطوات محددة للتحقيق في نوع معين من الهجمات

#### Playbook مثال:

1. استلام Alert من SIEM
    
2. تحليل سلوك اليوزر سابقًا
    
3. فحص سمعة الـ IP
    
4. التأكد هل في Login ناجح
    
5. تحديد هل نحتوي الهجوم ولا لأ
    

كل خطوة **مرتبطة باللي بعدها**.

---

# 2️⃣ Automation (الأتمتة)

### معناها إيه؟

SOAR **ينفذ الخطوات لوحده** من غير تدخل بشري.

### نفس المثال بس أوتوماتيك:

- SIEM يبعت Alert
    
- SOAR:
    
    - يسأل SIEM تلقائيًا
        
    - يكلم Threat Intelligence تلقائيًا
        
    - يقرر بناءً على النتائج
        
    - ينفذ أكشن (Block / Disable User)
        
    - يفتح Ticket تلقائيًا
        

### النتيجة؟

- وقت أقل
    
- مجهود أقل
    
- أخطاء أقل
    
- Analyst يركز في الحاجات المعقدة
    

---

# 3️⃣ Response (الاستجابة)

### معناها إيه؟

تنفيذ **إجراءات فعلية** لاحتواء الهجوم.

### أمثلة Actions:

- Block IP من Firewall
    
- Disable User من IAM
    
- Quarantine Device من EDR
    
- Reset Password
    
- Notify SOC Team
    

وكل ده:

- من **واجهة واحدة**
    
- أوتوماتيك
    
- موثق في Ticket
    

---

## SOAR حل إيه بالضبط؟

|المشكلة|SOAR حلها|
|---|---|
|Alert Fatigue|يقللها بالأتمتة|
|كثرة الأدوات|يوحدها|
|بطء الاستجابة|يسرّعها|
|أخطاء بشرية|يقللها|
|توثيق ضعيف|توثيق تلقائي|

---

## هل SOAR يلغي دور الـ SOC Analyst؟

❌ لا، ودي نقطة مهمة جدًا في الامتحانات والمقابلات.

### ليه؟

- SOAR **مش بيفكر**
    
- مش بيقدر:
    
    - يحكم على سيناريو معقد
        
    - يفهم سياق البزنس
        
    - يعدّل Playbook حسب ظروف الشركة
        

### دور الـ Analyst:

- تصميم Playbooks
    
- اتخاذ قرارات حرجة
    
- التحقيق في الهجمات المعقدة
    
- تحسين القواعد والأتمتة
    

👉 SOAR **يساعد** الـ Analyst، مش يستبدله.


#  يعني إيه SOAR Playbook عمليًا؟

**SOAR Playbook** هو:

> Workflow أو سيناريو مُسبق التجهيز  
> بيحدد للـ SOAR:

- إمتى يبدأ
    
- يعمل إيه
    
- وياخد قرار إزاي
    
- وإمتى يدخل الإنسان (SOC Analyst)
    

هو **تحويل خبرة الـ SOC Analyst لخطوات أوتوماتيك**.

يعني بدل ما الخبرة تفضل في دماغ شخص:  
👉 تتحول لسيستم ذكي يمشي عليها.

---

# ليه بنبني Playbooks؟

لأن في:

- Alerts متكررة
    
- نفس خطوات التحقيق بتتكرر
    
- نفس القرارات بتتاخد
    

أشهر مثالين:

- Phishing
    
- CVEs
    

---

# 1️⃣ Phishing Playbook (بالتفصيل العملي)

## المشكلة

الهجمات الاحتيالية (Phishing):

- أكتر Attack Vector شائع
    
- بتستهلك وقت كبير
    
- خطواتها كلها Manual لو من غير SOAR
    

## خطوات التحقيق التقليدية (من غير SOAR)

SOC Analyst بيعمل:

- يفتح الإيميل
    
- يفحص اللينكات
    
- يحلل الـ Attachment
    
- يراجع Threat Intel
    
- يرد على المستخدم
    
- يعمل Remediation
    

وده بيكرر **مئات المرات يوميًا**.

---

## تصميم Playbook Phishing (تفكير Analyst)

تخيل إنك:

> Senior SOC Analyst  
> وبتشرح لجونيور يعمل إيه لما يوصله Alert

### بداية الـ Playbook

**Trigger**

- Alert من:
    
    - Email Gateway
        
    - SIEM
        
    - User Report
        

📌 _"Suspicious Email Received"_

---

### Step 1: Create Ticket

- SOAR يفتح Incident Ticket تلقائي
    
- يسجل:
    
    - Sender
        
    - Receiver
        
    - Subject
        
    - Timestamp
        

📌 التوثيق بيبدأ من أول ثانية

---

### Step 2: Analyze Email Content

SOAR يسأل:

- هل الإيميل فيه:
    
    - URL؟
        
    - Attachment؟
        

---

### Case 1: لا URL ولا Attachment

- غالبًا:
    
    - Spam
        
    - Low-risk Phishing
        

🔹 Action:

- Notify User
    
- Close Ticket (Low Severity)
    

---

### Case 2: URL موجود

SOAR ينفذ:

1. Extract URL
    
2. Check URL Reputation
    
    - VirusTotal
        
    - AbuseIPDB
        
    - AlienVault OTX
        
3. Sandbox URL (اختياري)
    

#### Decision:

- Malicious؟
    
    - Block URL
        
    - Quarantine Email
        
    - Notify Users
        
    - Escalate Incident
        
- Clean؟
    
    - Close Ticket
        

---

### Case 3: Attachment موجود

SOAR ينفذ:

1. Extract Attachment
    
2. Hash File
    
3. Scan via:
    
    - Antivirus
        
    - Sandbox
        
4. Analyze Behavior
    

#### Decision:

- Malicious؟
    
    - Delete Email
        
    - Block Sender
        
    - Containment Actions
        
- Clean؟
    
    - Inform User
        
    - Close Incident
        

---

### Human Touch (نقطة تدخل Analyst)

- لو:
    
    - النتائج مش واضحة
        
    - False Positive محتمل
        
    - Target VIP User
        

📌 هنا يظهر دور الإنسان

---

## النتيجة

- 80% من الشغل اتعمل أوتوماتيك
    
- Analyst يدخل بس في الحالات الحساسة
    

---

# 2️⃣ CVE Patching Playbook (تفصيل تقني)

## المشكلة

- CVEs بتطلع يوميًا
    
- صعب تتابعهم يدويًا
    
- Backlog كبير
    
- Patching متأخر = اختراق محتمل
    

---

## فكرة Playbook CVE

تحويل:

> Vulnerability Management  
> من شغل يدوي → Workflow أوتوماتيك

---

## خطوات Playbook CVE

### Step 1: CVE Detection

SOAR يتابع:

- NVD
    
- Vendor Advisories
    
- Security Feeds
    

📌 Trigger:

> New CVE Published

---

### Step 2: CVE Analysis

SOAR يحلل:

- CVSS Score
    
- Severity (Low / Medium / High / Critical)
    
- Affected Products
    

---

### Step 3: Asset Mapping

SOAR يسأل:

- هل عندنا:
    
    - Servers
        
    - Devices
        
    - Applications  
        متأثرة بالـ CVE؟
        

📌 لو مش موجودة:

- Close Ticket (Not Applicable)
    

---

### Step 4: Risk Assessment

لو موجودة:

- Severity عالي؟
    
- Exposed to Internet؟
    
- Business Critical؟
    

🔹 Decision Tree:

- Low Risk → Schedule Patch
    
- High Risk → Immediate Action
    

---

### Step 5: Create Patching Ticket

- SOAR يفتح Ticket للـ IT / Patch Team
    
- يحدد:
    
    - Priority
        
    - Deadline
        
    - Affected Assets
        

---

### Step 6: Patch Testing

قبل Production:

- Deploy Patch على:
    
    - Test Environment
        
- Verify:
    
    - Stability
        
    - No Service Break
        

📌 Analyst Review Required هنا

---

### Step 7: Production Deployment

- Patch applied
    
- Verify remediation
    
- Rescan vulnerabilities
    

---

## نقاط تدخل SOC Analyst

- تقييم المخاطر
    
- الموافقة على Production Deployment
    
- التعامل مع Exceptions
    

---

# نقطة مهمة جدًا (Core Concept)

في كل Playbook:

- **الأتمتة مش 100%**
    
- الإنسان موجود عند:
    
    - Decision Making
        
    - Risk Approval
        
    - Complex Scenarios
        

وده سبب إن:

> SOAR ≠ Replacement for SOC Analysts