# Web Shell Overview

## ما هو Web Shell؟

الـ Web Shell هو:

- برنامج خبيث يتم رفعه على Web Server مستهدف
    
- يسمح للمهاجم بتنفيذ أوامر عن بُعد
    

يُستخدم كـ:

- وسيلة دخول أولي (Initial Access)
    
- أو كوسيلة استمرارية (Persistence)
    

بعد ما المهاجم يحصل على وصول للسيرفر:

- يقدر ينفذ أوامر
    
- يعمل استطلاع (Reconnaissance)
    
- يرفع صلاحيات
    
- يتحرك داخل الشبكة
    
- يسرّب بيانات
    

---

## مثال على Web Shell

تم ذكر مثال لملف:

- awebshell.php
    

موجود في:

- مجلد `/uploads`
    
- على السيرفر 10.10.10.100
    

الواجهة تحتوي على:

- مكان لإدخال أوامر
    
- زر تشغيل
    
- ناتج تنفيذ الأمر
    

---

# Web Shell Deployment

لكي يرفع المهاجم Web Shell ويشغّله، يحتاج إلى:

- ثغرة File Upload
    
- أو Misconfiguration
    
- أو وصول مسبق للنظام
    

الثغرة تحدث عندما:

- التطبيق لا يتحقق من نوع الملف
    
- أو الامتداد
    
- أو المحتوى
    
- أو مكان التخزين
    

الـ Web Shell يمكن أن يكون:

- وسيلة دخول أولي
    
- أو وسيلة بقاء طويل المدى بعد الاختراق
    

---

## مثال توضيحي

موقع يسمح برفع صور حيوانات أليفة.

لو تم تطويره بشكل غير آمن:

- يمكن للمهاجم رفع ملف مثل:
    
    - shell.php
        
    - mydog.aspx
        

وبالتالي يحصل على تنفيذ أوامر على السيرفر.

---

# Examples in the Wild

## Hafnium & ProxyLogon

مجموعة Hafnium:

- رفعت Web Shell بامتداد .aspx
    
- على خوادم Windows Exchange
    
- في مجلدات مثل:
    
    - \inetpub\wwwroot\aspnet_client\
        

كما قامت بـ:

- تعديل ملفات .aspx موجودة في:
    
    - \install_path\FrontEnd\HttpProxy\owa\auth\
        

بعد نشر الـ Web Shell:

- تنفيذ أوامر
    
- استطلاع
    
- استخراج بيانات اعتماد
    
- إنشاء حسابات جديدة للبقاء
    
- حركة جانبية
    
- تسريب بيانات
    
- إخفاء الآثار
    

---

## Conti Ransomware Attackers

مهاجمو Conti:

- استغلوا ثغرة مشابهة في Microsoft Exchange
    
- رفعوا ملف:
    
    - aspnetclient_log.aspx
        
- في نفس مجلد:
    
    - \aspnet_client\
        

وخلال دقائق:

- رفعوا Web Shell احتياطي
    
- حددوا أجهزة الشبكة
    
- حددوا Domain Controllers
    
- حددوا Domain Administrators



-----------------------------------------------------------------


# Anatomy of a Web Shell

## Legitimate Function Abuse

الـ Web Shell بيعتمد على إساءة استخدام دوال شرعية موجودة في لغة البرمجة.

في PHP مثلًا، فيه دوال لتنفيذ أوامر على النظام، مثل:

- `shell_exec()`
    
- `exec()`
    
- `system()`
    
- `passthru()`
    

الدوال دي أصلًا ليها استخدامات مشروعة،  
لكن لو استُخدمت بطريقة غير آمنة، المهاجم يقدر ينفذ أوامر على السيرفر.

---


![[Pasted image 20260228235532.png]]
# Under the Hood

المثال المعروض هو Web Shell بسيط مكتوب بلغة PHP.

طريقة عمله:

1️⃣ يتحقق هل فيه باراميتر اسمه `cmd` في الرابط  
مثال:  
`?cmd=whoami`

2️⃣ يخزن الأمر اللي المستخدم دخله في متغير اسمه `$cmd`

3️⃣ ينفذ الأمر باستخدام `shell_exec()`

4️⃣ يعرض ناتج التنفيذ على الصفحة

---

## واجهة المستخدم

الصفحة تحتوي على:

- مكان لإدخال الأمر
    
- زر لتنفيذه
    
- مكان يعرض الناتج
    

في المثال:  
تم تنفيذ أمر `whoami`  
والنتيجة كانت:  
`www-data`

وده معناه إن الأوامر بتنُفذ بصلاحيات مستخدم السيرفر.

---

# اختلاف أنواع Web Shells

الـ Web Shell ممكن يكون:

- سطر واحد بسيط ينفذ أوامر من خلال الرابط
    
- أو واجهة متكاملة فيها:
    
    - GUI
        
    - حماية بكلمة مرور
        
    - مدير ملفات مدمج
        

---

# A Web Shell in Action

تم نشر Web Shell على الجهاز الهدف في:

http://MACHINE_IP:8080/files/awebshell.php

يمكن الوصول له:

- من المتصفح مباشرة
    
- أو من سطر الأوامر باستخدام `curl`
    

لو هتستخدم سطر الأوامر:  
لازم تعمل URL Encoding للأوامر.

مثال:

ls -la

تصبح:

ls%20-la

ويمكن استخدام Cyberchef للمساعدة في الترميز.



---------------------------------------------------------------------------
# Log-Based Detection – Web Server Logs

لأن الـ Web Shell بيعتمد على استغلال الـ Web Server،  
فأول مكان نبدأ فيه البحث هو:

- Web Server Logs  
    خصوصًا في:
    
- Apache
    
- Nginx
    

فهم الفرق بين السلوك الطبيعي والمشبوه يساعد في كشف النشاط الخبيث.

---

# شكل Access Logs

رغم إن التنسيق يختلف حسب السيرفر، غالبًا يحتوي على:

- Client IP
    
- Timestamp
    
- Request
    
- Status Code
    
- Response Size
    
- Referrer
    
- User-Agent
    

حقل **Remote log name** غالبًا يكون `-` لأنه قديم ونادر الاستخدام.  
حقل **Authenticated user** أيضًا يكون `-` إلا إذا كان هناك تسجيل دخول مسبق.

---

# Web Indicators

## 1️⃣ Unusual HTTP Methods & Request Patterns

- GET متكرر بسرعة → ممكن يكون فحص (Recon)
    
- POST بعد سلسلة GET → محاولة رفع Web Shell
    
- تكرار طلب لنفس الملف → تفاعل مع Web Shell
    

---

## 2️⃣ Request Methods To Be Aware Of

|Method|الاستخدام الطبيعي|إساءة الاستخدام|
|---|---|---|
|GET|جلب مورد|Recon أو التفاعل مع Web Shell|
|POST|إرسال بيانات|رفع Web Shell أو استخدامه|
|PUT|رفع أو استبدال ملف|رفع Web Shell|
|DELETE|حذف مورد|تنظيف آثار|
|OPTIONS|معرفة الطرق المدعومة|Recon|
|HEAD|مثل GET بدون محتوى|كشف وجود ملفات|

---

# مثال تسلسل هجوم

الهجوم يمكن أن يكون كالتالي:

- فحص الموقع للبحث عن مجلد مفتوح
    
- العثور على مجلد صالح (200 OK)
    
- العثور على فورم يمكن استغلاله
    
- رفع Web Shell
    
- الوصول إليه
    

جميع الطلبات من:

- نفس الـ IP
    
- نفس User-Agent  
    ويجب ملاحظة:
    
- Response Codes
    
- Timestamps
    

---

# Suspicious User-Agents & IP Addresses

## User-Agent

- مختصر بشكل غير طبيعي
    
- قديم جدًا (مثل MSIE 6.0)
    
- أدوات مثل curl أو wget
    

## IP Address

- عنوان خارجي في شبكة يفترض أنها داخلية
    

---

# Query Strings

هي الجزء من الرابط بعد `?`

مثال:

example.php?query=value

مؤشرات خطورة:

- Query طويل جدًا
    
- وجود كلمات مثل:
    
    - cmd=
        
    - exec=
        
- Query مشفر (Base64 مثلًا)
    

---

# Missing Referrer

الـ Referrer يوضح الصفحة السابقة.

عدم وجود Referrer:

- قد يشير إلى Web Shell  
    لكن له أسباب مشروعة أحيانًا.
    

---

# مثال طلب مشبوه

يتضمن:

- IP غير موثوق
    
- وقت غير طبيعي
    
- POST إلى ملف خبيث
    
- بدون Referrer
    
- User-Agent غير طبيعي
    

---

# Auditd

أداة لينكس لتسجيل الأحداث وإنشاء Audit Trail.

يمكن إنشاء Rules لتسجيل:

- تشغيل برامج
    
- تعديل ملفات
    

مثال:  
استخدام ausearch للبحث عن rule باسم web_shell

النتيجة تظهر:

- وقت الحدث
    
- اسم الملف
    
- المستخدم (مثل www-data)
    

---

# Web & Auditd Correlation

لكشف Web Shell بفعالية:

يجب ربط:

- Web Access Logs
    
- Error Logs
    
- Auditd Logs
    

مثال:  
POST مشبوه في Web Logs  
مرتبط بحدث creat أو execve في auditd

الربط بينهم يعطي صورة أوضح للهجوم.

---

# SIEM Platforms

فوائد أنظمة SIEM:

- جمع مركزي للوجات
    
- ربط مصادر متعددة
    
- إنشاء استعلامات مخصصة
    
- تسهيل البحث والتحليل
    

تساعد في اكتشاف Web Shell والنشاط الخبيث بكفاءة أعلى.


--------------------------------------------------------
# 🔎 Beyond Logs

أحيانًا الـ Logs لوحدها مش كفاية.  
لازم نستخدم طرق إضافية زي:

- **File System Analysis**
    
- **Network Traffic Analysis**
    

---

# 📁 أولًا: File System Analysis

أي Web Shell لازم يتخزن كـ **ملف** في السيرفر (إلا في حالات CMS اللي بتخزن المحتوى في Database).

---

## 📂 أماكن شائعة لوجود Web Shell

### Apache (Linux)

/var/www/html/

### Nginx (Linux)

/usr/share/nginx/html/

لكن المهاجمين غالبًا يستهدفوا:

- /uploads/
    
- /images/
    
- /admin/
    
- /tmp/ (لو الصلاحيات ضعيفة)
    

---

## ⚠️ ملاحظة مهمة

في منصات زي:

- WordPress
    
- Django
    

المحتوى ممكن يكون داخل **Database**  
فالهجوم ممكن يكون:

- حقن كود في Theme
    
- إضافة كود في Post
    
- تعديل Settings
    

وفي الحالة دي مش هتلاقيه بسهولة في File System.

---

# 🚩 مؤشرات خطورة في الملفات

## 1️⃣ أسماء غريبة أو عشوائية

مثال:

xj72ks.php  
abc123.php

## 2️⃣ امتدادات تنفيذية

- .php
    
- .jsp
    
- .aspx
    

## 3️⃣ Double Extensions

مثال:

image.jpg.php  
report.pdf.php

---

# 🛠️ أدوات مفيدة

## 🔍 find

للبحث عن ملفات اتعدلت في فترة معينة:

find /var/www -type f -name "*.php" -newerct "2025-07-01" ! -newerct "2025-08-01"

ده يساعدك تلاقي ملفات PHP اتعملت وقت الهجوم.

---

## 🔎 grep

للبحث داخل الملفات عن دوال خطيرة زي:

- eval(
    
- base64_decode(
    
- shell_exec(
    
- system(
    

مثال:

grep -r "eval(" wp-content

لو لقيت eval مع base64 → مؤشر قوي جدًا على Web Shell.

---

# 🌐 ثانيًا: Network Traffic Analysis

هنا بنحلل الـ PCAP أو الترافيك مباشرة.

الميزة:  
نقدر نشوف **المحتوى الفعلي للطلب** مش بس الميتاداتا.

---

# 🎯 مؤشرات نبحث عنها في الترافيك

## 1️⃣ HTTP Methods غريبة

- PUT
    
- DELETE
    
- POST غير طبيعي
    

---

## 2️⃣ User-Agent مشبوه

- curl
    
- أدوات قديمة جدًا
    
- أدوات فحص
    

---

## 3️⃣ Encoded Payloads

- Base64
    
- URL Encoding
    

---

## 4️⃣ كود خبيث داخل Body

ممكن تشوف:

<?php system($_GET['cmd']); ?>

مباشرة داخل الـ POST Payload.

---

## 5️⃣ عمليات غير طبيعية

- Web server process يشغل:
    
    - bash
        
    - sh
        
    - nc
        
    - wget
        

---

# 🔎 Wireshark Filters مفيدة

## البحث حسب Method

http.request.method == "POST"

## البحث عن ملفات PHP

http.request.uri contains ".php"

## البحث عن User-Agent

http.user_agent

---

# 🎬 مثال تسلسل الهجوم في Wireshark

الهجوم ممكن يظهر كالتالي:

1. Directory Fuzzing
    
2. العثور على مجلد صالح
    
3. POST لملف upload.php
    
4. رفع webshell.php
    
5. GET /webshell.php?cmd=id
    

وفي الـ Packet المفتوح هتشوف:

- اسم الفورم المستغل
    
- User-Agent
    
- اسم الملف
    
- كود الـ PHP نفسه
    

وده دليل قوي جدًا إن الاستغلال حصل.

---

# 🧠 الفرق بين الثلاث طرق

| الطريقة         | بتكشف إيه            |
| --------------- | -------------------- |
| Web Logs        | الطلبات الخارجية     |
| File System     | هل الملف موجود فعلًا |
| Network Traffic | محتوى الهجوم نفسه    |