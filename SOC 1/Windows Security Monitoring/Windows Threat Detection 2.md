
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
