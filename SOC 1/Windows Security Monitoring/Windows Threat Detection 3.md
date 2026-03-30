
---

# Command and Control (C2)

## الفكرة الأساسية

بعد أن يتم **اختراق الجهاز (Initial Access)** باستخدام:

- USB malware
    
- Phishing attachment
    

يبقى سؤال مهم:

```text
كيف يتحكم المهاجم في الجهاز بعد اختراقه؟
```

الإجابة هي:

```text
Command and Control (C2)
```

وهو أحد **MITRE ATT&CK tactics**.

---

# ما هو C2

**Command and Control** يعني:

```text
Channel between attacker and infected machine
```

أي قناة اتصال تسمح للمهاجم أن:

- يرسل **commands**
    
- يستقبل **data**
    
- يتحكم بالجهاز عن بعد
    

---

# Attacks Without C2

في بعض الحالات **C2 غير ضروري**.

مثال:

```text
RDP Breach
```

إذا تمكن المهاجم من الدخول إلى الجهاز عبر:

```text
Remote Desktop Protocol
```

فيمكنه ببساطة:

- فتح **CMD**
    
- تشغيل البرامج
    
- استخدام **GUI tools**
    

مثل:

- Computer Management
    
- Task Manager
    
- System Settings
    

أي أن المهاجم يتحكم مباشرة بالجهاز عبر **interactive session**.

لكن النص يوضح مشكلة هذه الطريقة:

```text
This method becomes unavailable when RDP is closed
```

أي أنه إذا تم:

- إغلاق RDP
    
- تأمينه
    

فإن المهاجم يفقد الوصول.

لذلك غالبًا يقوم المهاجم بإنشاء **C2 connection** حتى بعد نجاح RDP.

---

# Simplest C2

في حالات مثل:

```text
Phishing attacks
```

المهاجم لا يملك **RDP access**.

لذلك يحتاج إلى برنامج داخل الجهاز يقوم بـ:

```text
Connect back to attacker
```

أي:

```text
Reverse connection
```

ويظل متصلًا بالمهاجم:

```text
24/7
```

ويكون هذا البرنامج عادة:

```text
Phishing attachment
```

أي الملف الخبيث الذي فتحه الضحية.

---

# ماذا يفعل هذا البرنامج

البرنامج يقوم بـ:

1️⃣ الاتصال بالمهاجم  
2️⃣ انتظار الأوامر  
3️⃣ تنفيذ الأوامر على الجهاز

أي يعمل كـ:

```text
Remote command executor
```

---

# مثال مذكور في النص

النص يذكر أداة مشهورة في **C2 frameworks**:

```text
CobaltStrike
```

وهي أداة تستخدم لإنشاء:

```text
C2 channel
```

بين الضحية والمهاجم.

---

# Advanced C2 Scenario

في الهجمات الأكثر تطورًا يحدث شيء مختلف.

بدل أن يقوم **phishing attachment** بعمل C2 مباشرة.

يقوم بالخطوات التالية:

---

# الخطوة 1

تحميل **malware إضافي**.

```text
Download additional C2 malware
```

---

# الخطوة 2

إخفاءه داخل مجلد مثل:

```text
C:\Temp
```

---

# الخطوة 3

تشغيله كـ:

```text
New process
```

---

# لماذا يفعل المهاجم ذلك

لأن هذا يعطيه ميزة مهمة.

النص يوضح:

```text
Keep the attack going
```

أي استمرار الهجوم.

---

# مثال المشكلة

إذا قام الضحية بحذف:

```text
Original attachment
```

فإن الهجوم **لن يتوقف**.

لأن:

```text
C2 malware still running
```

أي أن البرنامج الثاني ما زال يعمل.

---

# استخدام هذه الطريقة في الهجمات الحقيقية

النص يذكر أن هذه الطريقة استخدمت في:

### 1️⃣ Ransomware attacks

حيث يقوم المهاجم:

- بسرقة البيانات
    
- ثم تحميل ransomware
    

---

### 2️⃣ APT29 phishing campaign

وهي حملة تجسس معروفة استخدمت:

```text
Phishing + C2 malware
```

---

# التسلسل الكامل للهجوم

من بداية الهجوم حتى C2 يكون غالبًا هكذا:

```text
1. Initial Access (Phishing / USB / RDP)
2. Malware execution
3. Download additional tools
4. Start C2 malware
5. Attacker sends commands remotely
```

---

# الخلاصة

**Command and Control (C2)** هو:

```text
Communication channel between attacker and infected machine
```

ويسمح للمهاجم بـ:

- التحكم بالجهاز
    
- إرسال أوامر
    
- استلام البيانات
    

وقد يتم إنشاؤه بطريقتين:

### الطريقة الأولى

```text
Phishing attachment acts as C2 client
```

---

### الطريقة الثانية

```text
Attachment downloads dedicated C2 malware
```

ثم يقوم هذا malware بالاتصال بالمهاجم.

---

-----
# Persistence Overview

## ما معنى Persistence

بعد **Initial Access** (الاختراق الأولي)، المهاجم أمام خيارين:

### النوع الأول من الهجمات

هجمات سريعة مثل:

Data Stealers

خطواتها تكون:

1️⃣ اختراق الجهاز  
2️⃣ سرقة البيانات  
3️⃣ إرسال البيانات للخادم  
4️⃣ الخروج

كل هذا يحدث خلال **دقائق**.

---

### النوع الثاني من الهجمات

هجمات تحتاج وقت طويل مثل:

- Advanced Persistent Threat (APT)
    
- Ransomware attacks
    
- Corporate espionage
    

في هذه الحالة المهاجم يريد:

Maintain access for days or months

أي الاحتفاظ بالوصول للجهاز **أيام أو شهور**.

---

# تعريف Persistence

Persistence يعني:

Maintaining long-term access to the compromised system

أي:

الحفاظ على **وصول دائم للنظام** حتى لو حدث:

- Restart للجهاز
    
- تغيير كلمة المرور
    
- إغلاق الاتصال
    

---

# لماذا يحتاج المهاجم Persistence

لأن طريقة الدخول الأولى غالبًا **غير موثوقة**.

مثال:

- RDP session
    
- Phishing shell
    
- Reverse shell
    

هذه الطرق قد تختفي إذا:

OS reboot  
Password change  
Network change

لذلك المهاجم ينشئ **Backdoor Access**.

---

# مثال Persistence في النص

النص يذكر مثال مهم:

المهاجم يدخل عبر:

RDP

بسبب:

- weak password
    
- exposed service
    
- vulnerability
    

لكن بدلاً من الاعتماد على RDP فقط، يقوم بإنشاء **طريقة وصول أخرى**.

---

# طرق Persistence المذكورة

النص ذكر طريقتين.

---

# الطريقة الأولى

إنشاء ثغرة جديدة داخل النظام.

مثلاً:

Backdoor  
Web Shell

مثال:

Upload webshell.php

داخل السيرفر.

---

# الطريقة الثانية (الأكثر شيوعًا)

إنشاء مستخدم جديد.

وهذا يسمى في MITRE:

T1136 Create Account

ثم إعطاؤه صلاحيات Administrator.

وهذا يسمى:

T1098 Account Manipulation

---

# كيف يتم إنشاء المستخدم

هناك طريقتان.

---

# الطريقة الأولى GUI

باستخدام:

Computer Management

أو تشغيل:

lusrmgr.msc

ثم:

Local Users and Groups

---

# الطريقة الثانية CMD / PowerShell

وهي الأكثر استخدامًا من المهاجمين.

---

# إنشاء المستخدم

CMD:

net user "mr.backd00r" "p@ssw0rd!" /add

المعنى:

Create new user

اسم المستخدم:

mr.backd00r

كلمة المرور:

p@ssw0rd!

---

PowerShell:

New-LocalUser "mr.backd00r" -Password [...]

---

# إعطاء صلاحية Administrator

CMD:

net localgroup Administrators "mr.backd00r" /add

المعنى:

Add user to Administrators group

---

PowerShell:

Add-LocalGroupMember "Administrators" -Member "mr.backd00r"

---

# لماذا يضيفه Administrator

لأن المستخدم العادي لا يستطيع:

- تسجيل الدخول RDP
    
- التحكم بالنظام
    
- تنفيذ أوامر إدارية
    

لذلك يتم إضافته إلى:

Administrators  
Remote Desktop Users

---

# Detection (اكتشاف الهجوم)

أفضل مكان للتحقيق هو:

Windows Security Logs

---

# Event ID 4720

الصورة الأولى توضح:

Security 4720

المعنى:

A user account was created

---

### ماذا يظهر في اللوج

في الصورة:

Subject:  
THM-PC\Administrator

أي أن:

Administrator created the account

---

المستخدم الجديد:

New Account:  
THM-PC\itsupport

أي تم إنشاء مستخدم:

itsupport

---

# Event ID 4732

الصورة الثانية.

Security 4732

المعنى:

A member was added to a security group

---

في الصورة:

المستخدم:

itsupport

تم إضافته إلى:

BUILTIN\Administrators

أي أصبح:

Administrator

---

# Event ID 4724

الصورة الثالثة.

Security 4724

المعنى:

Attempt to reset password

---

في الصورة:

المستخدم:

Administrator

قام بمحاولة تغيير كلمة مرور:

j.adams

---

# لماذا يستخدم المهاجم Reset Password

بدلاً من إنشاء حساب جديد قد يقوم بـ:

Takeover existing account

خصوصًا:

- حساب قديم
    
- حساب غير مستخدم
    
- حساب موظف ترك الشركة
    

---

# كيف يحقق محلل SOC

عند رؤية Event 4720 يجب التحقيق في:

---

### 1️⃣ من أنشأ الحساب

Subject Account

هل هذا الشخص فعلًا أنشأ المستخدم؟

---

### 2️⃣ وقت الإنشاء

Date and Time

هل حدث في وقت طبيعي؟

مثلاً:

3 AM

غالبًا مشبوه.

---

### 3️⃣ عنوان الـ IP

Source IP

هل هو جهاز الشركة؟

---

### 4️⃣ الأحداث الأخرى في نفس الجلسة

مثلاً:

4624 logon  
4732 add to admin  
4724 password reset

---

# تسلسل هجوم Persistence كامل

غالبًا يكون كالتالي:

Initial Access  
↓  
Create new user (4720)  
↓  
Add user to admin group (4732)  
↓  
Login via RDP  
↓  
Maintain access

---

# خلاصة الصورة

الصورة تلخص **3 أحداث مهمة جدًا في التحقيقات**.

|Event ID|المعنى|
|---|---|
|4720|إنشاء مستخدم|
|4732|إضافة المستخدم إلى Admin|
|4724|تغيير كلمة مرور|

---

# معلومة مهمة للمحللين

هذه الثلاثة أحداث تعتبر **Golden Events** في التحقيقات.

أي عندما تراها معًا غالبًا يكون:

Persistence attack
![[Pasted image 20260310162255.png]]
------
----------
---------
سأشرح لك **Persistence via Tasks & Services** بالتفصيل خطوة-بخطوة، لأن هذه الطريقة من **أكثر طرق البقاء داخل النظام شيوعًا في الهجمات الحقيقية** ويجب على محلل **SOC أو DFIR** فهمها جيدًا.

---

# Malware Persistence

بعد أن يخترق المهاجم الجهاز، هدفه يصبح:

**الحفاظ على الاتصال بالجهاز حتى بعد:**

- إعادة تشغيل النظام 🔄
    
- حذف الملف الخبيث
    
- إغلاق الـ session
    

لذلك يحتاج إلى **Malware يعمل تلقائيًا بعد كل Restart**.

الفكرة هي:

```
System Boot
     ↓
Malware runs automatically
     ↓
Connect to C2 server
     ↓
Attacker keeps control
```

---

# طرق Persistence في Windows

يوجد **عشرات الطرق** للبقاء داخل النظام، لكن النص يركز على **طريقتين شائعتين جدًا**:

1️⃣ **Windows Services**  
2️⃣ **Scheduled Tasks**

---

# الطريقة الأولى: Windows Service Persistence

## ما هو الـ Service

الـ **Service** هو برنامج يعمل في الخلفية داخل Windows.

أمثلة خدمات النظام:

- Windows Update
    
- DNS Client
    
- Security Center
    

هذه الخدمات تعمل تلقائيًا عند تشغيل النظام.

---

## ماذا يفعل المهاجم

يقوم بإنشاء **Service خبيثة**.

عند تشغيل الجهاز:

```
Windows starts services
      ↓
Malicious service runs malware.exe
      ↓
Malware connects to C2
```

---

# مثال الهجوم

الأمر الذي يستخدمه المهاجم:

```cmd
sc create "BadService" binpath= "C:\malware.exe" start= auto
```

### شرح الأمر

```
sc create
```

إنشاء Service جديدة.

```
BadService
```

اسم الخدمة.

```
binpath=
```

مسار البرنامج الذي سيتم تشغيله.

```
start= auto
```

تشغيل الخدمة تلقائيًا عند **Startup**.

---

# ماذا يحدث بعد Restart

```
System Boot
↓
Service Manager
↓
BadService starts
↓
malware.exe runs
↓
C2 connection
```

---

# اكتشاف Service Persistence

يمكن اكتشافها بثلاث طرق.

---

## الطريقة 1: Sysmon Event ID 1

عند تشغيل الأمر:

```
sc.exe
```

سيظهر في اللوج:

```
Sysmon Event ID 1
Process Create
```

ويظهر أن:

```
Process: sc.exe
CommandLine: sc create BadService
```

---

## الطريقة 2: Security Event ID 4697

هذا الحدث يعني:

```
A service was installed in the system
```

أي تم إنشاء Service جديدة.

---

## الطريقة 3: System Event ID 7045

يظهر أيضًا عندما يتم تثبيت خدمة.

---

## الطريقة 4 (مهمة جدًا في التحقيق)

تحليل **Process Tree**.

قد ترى مثلًا:

```
services.exe
     ↓
malware.exe
```

وهذا يدل أن البرنامج يعمل كـ **Service**.

---

# الطريقة الثانية: Scheduled Task Persistence

## ما هي Scheduled Tasks

هي وظيفة داخل Windows تسمح بتشغيل برنامج **في وقت محدد**.

أمثلة شرعية:

- تحديث البرامج
    
- Backup
    
- System cleanup
    

---

## ماذا يفعل المهاجم

ينشئ **Scheduled Task خبيثة**.

عند تشغيل النظام:

```
Task Scheduler starts
        ↓
BadTask runs malware.exe
        ↓
C2 connection
```

---

# مثال الهجوم

الأمر:

```cmd
schtasks /create /tn "BadTask" /tr "C:\malware.exe" /sc onstart /ru System
```

---

## شرح الأمر

```
schtasks /create
```

إنشاء مهمة جديدة.

```
/tn "BadTask"
```

اسم المهمة.

```
/tr "C:\malware.exe"
```

البرنامج الذي سيتم تشغيله.

```
/sc onstart
```

تشغيل المهمة عند **Startup**.

```
/ru System
```

تشغيلها بصلاحيات **SYSTEM**.

---

# لماذا Scheduled Tasks خطيرة

لأنها:

- سهلة الإنشاء
    
- سهلة الإخفاء
    
- تعمل تلقائيًا
    
- يمكن تشغيلها بصلاحيات عالية
    

ولهذا السبب هي **أكثر Persistence Method استخدامًا** في الهجمات.

وقد استخدمتها مجموعات مثل:

- APT28
    
- APT41
    

---

# اكتشاف Scheduled Tasks

يمكن اكتشافها بثلاث طرق.

---

# الطريقة 1: Sysmon Event ID 1

عند تشغيل:

```
schtasks.exe
```

سيظهر في:

```
Sysmon Event ID 1
```

مثال:

```
Process: schtasks.exe
CommandLine: schtasks /create ...
```

---

# الطريقة 2: Security Event ID 4698

هذا الحدث يعني:

```
A scheduled task was created
```

أي تم إنشاء مهمة جديدة.

---

# الطريقة 3: تحليل Process Tree

غالبًا سيظهر:

```
svchost.exe -s Schedule
       ↓
malware.exe
```

لأن **Task Scheduler service** يعمل داخل:

```
svchost.exe
```

---

# الفرق بين Services و Tasks

|Feature|Services|Scheduled Tasks|
|---|---|---|
|Startup|عند تشغيل النظام|يمكن تحديد وقت|
|إخفاء|أصعب|أسهل|
|الاستخدام في الهجمات|شائع|الأكثر شيوعًا|
|الصلاحيات|عالية|يمكن SYSTEM|

---

# ماذا يفعل محلل SOC عند التحقيق

عند رؤية:

```
4697
4698
sc.exe
schtasks.exe
```

يجب التحقيق في:

1️⃣ من أنشأ الخدمة أو المهمة  
2️⃣ ما هو البرنامج الذي سيتم تشغيله  
3️⃣ مكان الملف في النظام  
4️⃣ هل الملف Malware

---

# مثال Attack Chain

هجوم كامل قد يبدو هكذا:

```
Phishing Email
      ↓
User runs attachment
      ↓
Malware runs
      ↓
Create Scheduled Task
      ↓
System reboot
      ↓
Task runs malware again
      ↓
C2 connection
```

---

# أهم Event IDs في هذا الدرس

|Event ID|المعنى|
|---|---|
|Sysmon 1|Process creation|
|4697|Service created|
|7045|Service installed|
|4698|Scheduled task created|

---

✅ **الخلاصة المهمة**

أكثر طريقتين يستخدمهما المهاجمون للبقاء داخل النظام:

```
Windows Services
Scheduled Tasks
```

ويمكن اكتشافهم عبر:

```
Sysmon logs
Security logs
Process tree analysis
```

---

سأشرح **Persistence via Run Keys و Startup Folder** بالتفصيل خطوة-بخطوة، لأن هذه الطرق **منتشرة جدًا في malware الحقيقي** مثل **stealers و trojans** وغالبًا تظهر في **Digital Forensics و SOC investigations**.

---

# الفكرة العامة

الطرق السابقة مثل:

- **Services**
    
- **Scheduled Tasks**
    

تعمل عند:

```text
System Boot
```

لكنها غالبًا تحتاج **Administrator privileges**.

السؤال هنا:

❓ ماذا لو أراد المهاجم تشغيل malware **فقط عندما يسجل المستخدم الدخول**؟

الحل هو استخدام **User Persistence Methods**.

وأشهر طريقتين:

1️⃣ **Startup Folder**  
2️⃣ **Run Registry Keys**

كلاهما يعمل عند:

```text
User Logon
```

---

# الطريقة الأولى: Startup Folder Persistence

## ما هو Startup Folder

هو مجلد في Windows يحتوي على البرامج التي يتم تشغيلها **تلقائيًا عند تسجيل الدخول**.

الفكرة بسيطة:

```text
User Login
     ↓
Explorer loads startup programs
     ↓
Programs inside Startup folder run
```

---

# مسار Startup Folder

للمستخدم الحالي:

```text
C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\
```

لكل المستخدمين:

```text
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
```

---

# كيف يستخدمه المهاجم

يقوم بنسخ malware داخل هذا المجلد.

مثال:

```cmd
copy C:\malware.exe "%AppData%\Microsoft\Windows\Start Menu\Programs\Startup\malware.exe"
```

---

# ماذا يحدث بعد ذلك

عند تسجيل الدخول:

```text
User Login
     ↓
Explorer loads Startup Folder
     ↓
malware.exe runs
     ↓
Malware connects to C2
```

---

# لماذا يستخدمه المهاجمون

لأنه:

- بسيط جدًا
    
- لا يحتاج صلاحيات Admin
    
- يعمل تلقائيًا عند Login
    

---

# ملاحظة مهمة

النص يوضح نقطة مهمة جدًا:

Startup Folder غالبًا **فارغ**.

معظم البرامج الحديثة **لا تستخدمه**.

لذلك إذا وجدت ملفًا داخله، غالبًا يجب التحقيق فيه.

---

# اكتشاف Startup Persistence

أفضل طريقة هي مراقبة **File Creation Events**.

الحدث:

```text
Sysmon Event ID 11
```

وهو يعني:

```text
File Created
```

إذا رأيت إنشاء ملف داخل:

```text
Startup folder
```

فهذا **Suspicious Indicator**.

---

# Process Tree في هذه الحالة

غالبًا ستجد:

```text
explorer.exe
     ↓
malware.exe
```

لأن **Explorer** هو الذي يشغل برامج Startup.

لكن هذا يجعل التمييز بين **برنامج شرعي و malware** أصعب.

---

# الطريقة الثانية: Run Registry Keys

## ما هو Run Key

هو **Registry Key** في Windows يسمح بتشغيل برنامج عند تسجيل الدخول.

الفكرة:

```text
User Login
     ↓
Windows reads Run Registry Keys
     ↓
Programs inside Run keys start
```

---

# مكان Run Keys في Registry

للمستخدم الحالي:

```text
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

لكل المستخدمين:

```text
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
```

---

# كيف يستخدمه المهاجم

يقوم بإضافة قيمة جديدة داخل Run Key.

مثال:

```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v BadKey /t REG_SZ /d "C:\malware.exe"
```

---

# شرح الأمر

```text
reg add
```

إضافة قيمة داخل Registry.

```text
/v BadKey
```

اسم القيمة.

```text
/d "C:\malware.exe"
```

مسار البرنامج الذي سيتم تشغيله.

---

# ماذا يحدث بعد Login

```text
User Login
      ↓
Windows reads Run Keys
      ↓
C:\malware.exe executes
      ↓
C2 connection
```

---

# لماذا يستخدم المهاجم Run Keys

لأنها:

- لا تحتاج Admin أحيانًا
    
- مخفية داخل Registry
    
- سهلة التنفيذ
    
- تعمل عند كل Login
    

---

# اكتشاف Run Key Persistence

يتم عبر مراقبة **Registry Changes**.

الحدث:

```text
Sysmon Event ID 13
```

وهو يعني:

```text
Registry Value Set
```

أي تم إنشاء أو تعديل قيمة داخل Registry.

---

# كيف يحقق محلل SOC

عند رؤية Event ID 13 يجب التحقق من:

1️⃣ هل التغيير حدث داخل:

```text
CurrentVersion\Run
```

2️⃣ ما هو البرنامج الذي سيتم تشغيله؟

3️⃣ هل المسار طبيعي أم مشبوه؟

مثال مشبوه:

```text
C:\Users\Public\svchost.exe
```

---

# الفرق بين Startup Folder و Run Keys

|Feature|Startup Folder|Run Keys|
|---|---|---|
|الموقع|File System|Registry|
|طريقة الإضافة|Copy file|Add registry value|
|سهولة الإخفاء|أقل|أعلى|
|الاستخدام في malware|شائع|شائع جدًا|

---

# تسلسل هجوم Persistence باستخدام Run Keys

هجوم كامل قد يبدو هكذا:

```text
Phishing Email
      ↓
User runs malware
      ↓
Malware executes
      ↓
Registry Run Key added
      ↓
User reboot / logout
      ↓
User login
      ↓
Malware runs automatically
      ↓
Connect to C2
```

---

# أهم Event IDs في هذا الدرس

|Event ID|المعنى|
|---|---|
|Sysmon 11|File created|
|Sysmon 13|Registry value created|

---

# خلاصة الدرس

هناك طريقتان **User Persistence** تعملان عند Login:

```text
Startup Folder
Run Registry Keys
```

ويتم اكتشافهما عبر:

```text
Sysmon Event ID 11
Sysmon Event ID 13
```

---

✅ **معلومة مهمة جدًا في التحقيقات**

إذا رأيت في نفس الـ timeline:

```text
Phishing execution
↓
Registry Run Key added
↓
Malware connection to C2
```

فهذا غالبًا يدل على:

```text
Persistence setup by malware
```

---
-----
----
---

# أولاً: لماذا يحتاج المهاجم Persistence

سؤال مهم يطرحه النص:

❓ لماذا لا يسرق المهاجم البيانات ويغادر مباشرة؟

السبب أن كثير من الهجمات **ليست سريعة** بل تحتاج **وجود طويل داخل النظام**.

الأسباب الرئيسية ثلاثة.

---

# 1️⃣ تحويل الجهاز إلى Botnet

أحيانًا المهاجم لا يهتم ببيانات الضحية أصلاً.

بل يريد استخدام الجهاز كـ:

```text
Bot
```

داخل شبكة أجهزة مخترقة.

هذه الشبكة تسمى:

```text
Botnet
```

ويتم استخدامها في:

- DDoS attacks
    
- Cryptomining
    
- Spam campaigns
    
- Malware distribution
    

---

### مثال ذكره النص

```text
Kraken Botnet
```

وهو يجمع بين:

- Crypto miner
    
- Data stealer
    
- C2 connection
    

أي أن الجهاز المخترق يصبح **آلة ربح للمهاجم**.

---

# 2️⃣ التجسس (Cyber Espionage)

بعض الهجمات تكون **مدعومة من دول**.

الهدف ليس المال بل **التجسس**.

مثال ذكره النص:

```text
Volt Typhoon
```

هذه المجموعة:

- بقيت داخل شبكة **US Electric Grid**
    
- لمدة **تقريبًا سنة كاملة**
    
- بدون اكتشاف
    

هذا مثال واضح على **Advanced Persistent Threat (APT)**.

---

# 3️⃣ استخدام الجهاز كنقطة دخول للشبكة

أحيانًا المهاجم يخترق **جهاز واحد فقط**.

لكن الهدف الحقيقي هو:

```text
Entire network
```

مثل:

- Active Directory
    
- Domain Controllers
    
- File Servers
    
- Databases
    

اختراق شبكة كاملة قد يستغرق:

```text
29 days
```

كما ذكر النص.

وهذا يسمى:

```text
Lateral Movement
```

أي الانتقال بين أجهزة الشبكة.

---

# Active Directory ولماذا هو هدف رئيسي

معظم الشركات تستخدم:

```text
Active Directory (AD)
```

وهو نظام لإدارة:

- المستخدمين
    
- الأجهزة
    
- الصلاحيات
    
- السيرفرات
    

إذا سيطر المهاجم على **Domain Admin** يصبح لديه:

```text
Full control of the network
```

---

# أخطر نتيجة للهجوم: Ransomware

أخطر هجوم على الشركات اليوم هو:

```text
Ransomware
```

الفكرة:

1️⃣ اختراق الشبكة  
2️⃣ سرقة البيانات  
3️⃣ تشفير السيرفرات  
4️⃣ طلب فدية

---

# لماذا Ransomware مرعب للشركات

لأنه يمكن أن يوقف **شركة كاملة**.

مثال حقيقي ذكره النص:

```text
McLaren Hospitals
```

النتيجة:

- تأثر **743,000 مريض**
    
- توقف الأنظمة الطبية
    
- تسريب البيانات
    

بل حتى:

```text
Ransom notes printed on printers
```

أي أن رسالة الفدية تطبع تلقائيًا في كل مكاتب الشركة.

---

# كيف تبدأ هذه الهجمات الكبيرة

النص يؤكد نقطة مهمة جدًا:

⚠️ كل هجوم ضخم يبدأ بـ:

```text
Single breach
```

أي **اختراق بسيط واحد فقط**.

مثلاً:

- Phishing email
    
- Weak RDP password
    
- Vulnerable website
    
- Malware attachment
    

---

# تسلسل الهجوم الكامل

الهجوم غالبًا يسير بهذا الشكل:

```text
Initial Access
↓
Execution
↓
Persistence
↓
Privilege Escalation
↓
Credential Access
↓
Lateral Movement
↓
Data Exfiltration
↓
Impact (Ransomware)
```

هذا التسلسل مبني على:

```text
MITRE ATT&CK Framework
```

---

# ما الذي تعلمته في هذه الغرفة

النص يقول إنك تعلمت اكتشاف المراحل التالية.

---

# 1️⃣ Initial Access

كيف يدخل المهاجم النظام.

مثل:

- Phishing
    
- RDP brute force
    
- Exploits
    

---

# 2️⃣ Discovery

جمع معلومات عن النظام مثل:

- المستخدمين
    
- الشبكة
    
- البرامج
    

---

# 3️⃣ Collection

جمع البيانات المهمة مثل:

- Cookies
    
- Passwords
    
- Files
    
- Wallets
    

---

# 4️⃣ Persistence

كيف يبقى المهاجم داخل النظام مثل:

- Users
    
- Services
    
- Scheduled Tasks
    
- Run Keys
    
- Startup Folder
    

---

# 5️⃣ Command and Control

إنشاء قناة اتصال مع المهاجم.

مثل:

```text
C2 server
```

---

# 6️⃣ Impact

المرحلة الأخيرة مثل:

- Ransomware
    
- Data destruction
    
- Service shutdown
    

---

# لماذا تعلم هذه المراحل مهم

لأن محلل الأمن السيبراني يجب أن يوقف الهجوم **قبل الوصول إلى Impact**.

أفضل نقطة للإيقاف هي:

```text
Initial Access
```

أو

```text
Persistence stage
```

قبل أن يصل المهاجم إلى:

```text
Domain Admin
```

---

# الخلاصة

الهجمات الكبيرة مثل **Ransomware** تبدأ دائمًا بخطوة صغيرة.

والتسلسل عادة يكون:

```text
Breach
↓
Persistence
↓
Network movement
↓
Data theft
↓
Ransomware
```

ومهمة محلل الأمن هي:

```text
Detect the attack early
```

قبل أن يحدث:

```text
Impact
```

---
