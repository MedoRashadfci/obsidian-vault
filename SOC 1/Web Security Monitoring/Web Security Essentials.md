

# 🌐 Why Web? — لماذا أصبح الويب هو الهدف الأكبر أمنيًا؟

## 📈 التحول من Desktop إلى Web

![https://cdn.renderhub.com/face-the-edge/90s-pc-desktop-style-01/90s-pc-desktop-style-01-03.jpg](https://cdn.renderhub.com/face-the-edge/90s-pc-desktop-style-01/90s-pc-desktop-style-01-03.jpg)

![https://www.cs.odu.edu/~tkennedy/cs300/development/Public/M02-HistoryOfEmail/image2.jpeg](https://www.cs.odu.edu/~tkennedy/cs300/development/Public/M02-HistoryOfEmail/image2.jpeg)

![https://cloudfront.codeproject.com/webservices/cloudsaas/main.jpg](https://cloudfront.codeproject.com/webservices/cloudsaas/main.jpg)

4

### 🖥️ التسعينات (1990s)

- تطبيقات Desktop هي الأساس
    
- تثبيت محلي
    
- تحديثات يدوية
    
- إنترنت محدود وبطيء
    

### 🌍 الألفينات (2000s)

- ظهور Web Applications ديناميكية
    
- Email عبر المتصفح
    
- Social Media
    
- Online Banking
    

### ☁️ 2010s → اليوم

- Cloud Computing
    
- SaaS
    
- كل شيء عبر المتصفح:
    
    - تعديل فيديو
        
    - تشغيل VMs
        
    - إدارة شركات كاملة
        

---

# 🔐 من منظور أمني

الويب جاب مميزات قوية:

✅ وصول من أي مكان  
✅ تحديثات فورية  
✅ لا حاجة لتثبيت  
✅ توافق مع كل الأجهزة

لكن في المقابل 👇

❌ التطبيق دائمًا Online  
❌ مكشوف للعالم 24/7  
❌ نقطة دخول مباشرة للمهاجمين  
❌ متصل بقواعد بيانات وأنظمة داخلية

---

# 🎯 لماذا تعتبر Web Apps هدفًا رئيسيًا؟

1️⃣ مكشوفة على الإنترنت  
2️⃣ تعمل دائمًا  
3️⃣ متصلة بقاعدة بيانات  
4️⃣ متصلة بأنظمة داخلية  
5️⃣ غالبًا تحتوي بيانات حساسة

أي ثغرة صغيرة ممكن تتحول إلى:

> Initial Access → Lateral Movement → Data Exfiltration

---

# ⚠️ المخاطر من منظورين

## 👨‍💼 كمالك Web App

- لازم تؤمن التطبيق 24/7
    
- مسؤول عن بيانات المستخدمين
    
- أي ثغرة = كارثة قانونية
    
- مواكبة تهديدات جديدة باستمرار
    

---

## 👤 كمستخدم Web App

- بياناتك مخزنة عند طرف ثالث
    
- اختراق المتصفح = خطر على كل حساباتك
    
- خطر سرقة الهوية
    
- خسائر مالية
    

---

# 💥 أمثلة حقيقية

## 🏦 2017 — اختراق Equifax

Equifax

- تم استغلال ثغرة في:
    
    - 🔎 **Apache Struts**
        
- تسريب بيانات ~150 مليون شخص
    
- أرقام ضمان اجتماعي
    
- بيانات مالية حساسة
    

📌 السبب: Web App vulnerability

---

## 💳 2019 — اختراق Capital One

Capital One

- سوء إعداد WAF
    
- الوصول للبنية السحابية
    
- تسريب بيانات أكثر من 100 مليون عميل
    

📌 السبب: Misconfigured Web Infrastructure

---

# 🧠 لماذا Web Security مهمة جدًا؟

لأن Web App غالبًا:

- أول نقطة دخول
    
- أكثر نقطة تعرض
    
- تحتوي أكثر بيانات قيمة
    

لذلك معظم الهجمات تبدأ من:

- SQL Injection
    
- XSS
    
- RCE
    
- File Upload
    
- SSRF



# 🌐 Web Infrastructure — كيف تعمل بنية الويب؟

عندما تزور أي موقع:

1️⃣ المتصفح يرسل **HTTP Request**  
2️⃣ السيرفر يعالج الطلب  
3️⃣ يتحقق من الصلاحيات  
4️⃣ يرجع **HTTP Response**

هذه الدورة (Request → Response) هي أساس الإنترنت كله.

لكن 👇  
أي خلل في هذه السلسلة = فرصة هجوم.

---

# 🔄 دورة الطلب والاستجابة

![https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rv6iqwhcqdw9349bq0hs.png](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rv6iqwhcqdw9349bq0hs.png)

![https://miro.medium.com/1%2A13oqfKMwgrZ_5fX1HaFicg.png](https://miro.medium.com/1%2A13oqfKMwgrZ_5fX1HaFicg.png)

![https://www.researchgate.net/publication/7032895/figure/fig2/AS%3A287841717374976%401445638221395/System-architecture-The-system-used-the-PHP-504-Apache-1333-Win32-and-MySQL.png](https://www.researchgate.net/publication/7032895/figure/fig2/AS%3A287841717374976%401445638221395/System-architecture-The-system-used-the-PHP-504-Apache-1333-Win32-and-MySQL.png)

4

مثال عملي:

- المتصفح يطلب `/login`
    
- السيرفر يمرر الطلب للتطبيق (PHP مثلاً)
    
- التطبيق يستعلم من قاعدة البيانات
    
- يتم إرجاع الصفحة أو النتيجة
    

---

# 🏗️ مكونات أي Web Service

أي موقع مثل tryhackme.com يتكون من 3 طبقات رئيسية:

---

## 1️⃣ Application (التطبيق)

هو الكود الذي يتحكم في:

- الواجهة (HTML / CSS)
    
- المنطق البرمجي (PHP / Node / Python)
    
- إدارة المستخدمين
    
- الاتصال بقاعدة البيانات
    

🎯 هذه الطبقة غالبًا هدف:

- SQL Injection
    
- XSS
    
- RCE
    

---

## 2️⃣ Web Server (خادم الويب)

هو الذي:

- يستقبل الطلبات
    
- يمررها للتطبيق
    
- يعيد الاستجابة
    

### أشهر Web Servers

## 🧱 Apache

Apache HTTP Server

- شائع جدًا
    
- يستخدم كثيرًا مع WordPress
    
- مرن وسهل الإعداد
    

---

## ⚡ Nginx

Nginx

- عالي الأداء
    
- ممتاز للترافيك الكبير
    
- يستخدمه Netflix و GitHub
    

---

## 🏢 IIS

Internet Information Services

- من Microsoft
    
- شائع في بيئات Windows
    
- مستخدم بكثرة في الشركات
    

---

## 3️⃣ Host Machine (الجهاز المستضيف)

هو نظام التشغيل الذي يشغل كل شيء:

- Linux
    
- Windows Server
    

🎯 هذه الطبقة معرضة لهجمات:

- Privilege Escalation
    
- Misconfiguration
    
- Exploitation of OS services
    

---

# 🎯 أين يمكن أن يهاجم المهاجم؟

|الطبقة|نوع الهجوم|
|---|---|
|Application|SQLi, XSS, SSRF|
|Web Server|Directory Traversal, Misconfig|
|Host Machine|RCE, Kernel Exploit|

---

# ⚠️ لماذا Web Servers هدف دائم؟

- مكشوفة للعالم 24/7
    
- تتعامل مع كل الطلبات
    
- أول نقطة دخول للشبكة الداخلية
    
- غالبًا متصلة بقاعدة بيانات حساسة
    

---

# 🧠 مثال عملي لمسار الطلب

المتصفح → الإنترنت → Nginx → PHP App → Database → Response

لو حدثت ثغرة في أي نقطة:

- ممكن يتم تنفيذ أوامر
    
- أو تسريب بيانات
    
- أو السيطرة على السيرفر
    

---

# 🔥 خلاصة مهمة

Web Infrastructure = 3 طبقات أساسية:

Application  
     ↓  
Web Server  
     ↓  
Host OS

أمان الموقع يعتمد على تأمين كل طبقة منهم.



# Protecting the Web — كيف نحمي بنية الويب؟

تأمين أي خدمة ويب يعتمد على حماية **الثلاث طبقات الأساسية**:

Application  
     ↓  
Web Server  
     ↓  
Host Machine

كل طبقة لها أدوات حماية مختلفة 👇

---

# 🧠 أولًا: حماية الـ Application

## 1️⃣ Secure Coding

- تجنب الدوال غير الآمنة
    
- لا تعرض رسائل خطأ تكشف تفاصيل داخلية
    
- لا تترك API keys أو كلمات مرور في الكود
    

🎯 يمنع:

- RCE
    
- Information Disclosure
    
- Logic Flaws
    

---

## 2️⃣ Input Validation & Sanitization

- تحقق من كل إدخال مستخدم
    
- فلترة المدخلات
    
- منع special characters الضارة
    

🎯 يمنع:

- SQL Injection
    
- XSS
    
- Command Injection
    

---

## 3️⃣ Access Control

- Role-Based Access Control (RBAC)
    
- مبدأ "المستخدم يرى ما يخصه فقط"
    

🎯 يمنع:

- IDOR
    
- Privilege Escalation
    

---

# 🌐 ثانيًا: حماية الـ Web Server

## 📝 Logging (مهم جدًا 🔥)

سيرفرات الويب مثل:

- Apache HTTP Server
    
- Nginx
    
- Internet Information Services
    

تقوم بإنشاء **Access Logs** لكل طلب.

---

## مثال Access Log طبيعي

![https://i.sstatic.net/zIUZW.png](https://images.openai.com/static-rsc-1/nB7ZWPjNuOV2SB9-AV7nPWZsjoYRtHBizUBw6T9RAbxnqsB8pOPNxSM1EUFVONuGlueTbiupgaiQi3GaqiMXkP4HDeSMfju-UVz8etA90mfycMS2nHQGXnD4V-8paSBAaRdYd-P-6FD8VpJYqQ1wqQ)

![https://signoz.io/img/blog/2022/12/nginx_access_log_example.webp](https://signoz.io/img/blog/2022/12/nginx_access_log_example.webp)

![https://www.researchgate.net/publication/265729824/figure/fig3/AS%3A668926455316503%401536495903826/Sample-of-Extended-common-log-format-Collected-from-web-server-of-BHU-website.jpg](https://www.researchgate.net/publication/265729824/figure/fig3/AS%3A668926455316503%401536495903826/Sample-of-Extended-common-log-format-Collected-from-web-server-of-BHU-website.jpg)

4

مثال تسلسل طبيعي:

1️⃣ GET /index.html  
2️⃣ GET /login.html  
3️⃣ POST /login.html  
4️⃣ GET /myaccount.html

كل سطر في اللوج يحتوي على:

- IP Address
    
- Timestamp
    
- Request Method (GET / POST)
    
- URL
    
- Status Code (200, 404, 500)
    
- Response Size
    
- User Agent
    

---

## لماذا الـ Logging مهم؟

يساعد في:

- اكتشاف Brute Force
    
- اكتشاف Directory Traversal
    
- تتبع Data Exfiltration
    
- إعادة بناء Timeline للهجوم
    

---

## 🛡️ Web Application Firewall (WAF)

WAF يعمل كطبقة حماية أمام السيرفر.

وظيفته:

- فلترة الطلبات
    
- منع SQLi و XSS
    
- منع Bot traffic
    

🎯 يمنع الهجوم قبل وصوله للتطبيق.

---

## ☁️ Content Delivery Network (CDN)

- يقلل التعرض المباشر للسيرفر
    
- يمتص هجمات DDoS
    
- أحيانًا يحتوي على WAF مدمج
    

---

# 💻 ثالثًا: حماية الـ Host Machine

## 1️⃣ Least Privilege

- تشغيل الخدمة بصلاحيات محدودة
    
- منع root execution
    

---

## 2️⃣ System Hardening

- تعطيل الخدمات غير الضرورية
    
- غلق البورتات المفتوحة
    
- إزالة الحزم غير المستخدمة
    

---

## 3️⃣ Antivirus / EDR

- كشف Malware
    
- منع Persistence
    
- حماية على مستوى النظام
    

---

# 🔒 نصائح أمان مشتركة

## Strong Authentication

- MFA
    
- كلمات مرور قوية
    
- عدم استخدام credentials افتراضية
    

---

## Patch Management

- تحديث:
    
    - التطبيق
        
    - Web Server
        
    - نظام التشغيل
        
    - Dependencies
        

⚠️ كثير من الاختراقات حصلت بسبب عدم التحديث.

---

# 🔍 أهمية Access Logs في التحقيق

مثال طبيعي:

|Step|Request|
|---|---|
|1|GET /index.html|
|2|GET /login.html|
|3|POST /login.html|
|4|GET /myaccount.html|

لو ظهر بدلًا من ذلك:

- 500 محاولة POST في دقيقة واحدة
    
- طلب `/../../etc/passwd`
    
- Status code 500 متكرر
    

ده مؤشر نشاط مشبوه.

---

# 🎯 خلاصة مهمة

تأمين الويب ليس أداة واحدة — بل طبقات:

|الطبقة|وسيلة الحماية|
|---|---|
|Application|Secure Coding + Validation|
|Web Server|Logging + WAF|
|Host|Hardening + Least Privilege|


# 🛡️ Defense Systems — أنظمة الحماية في بيئة الويب

خلينا نفهم أهم أدوات الدفاع اللي بتحمي المواقع والتطبيقات 👇

---

# 🌍 Content Delivery Network (CDN)

![https://blog.scaleflex.com/content/images/2022/12/content-and-media-delivery-networks.png](https://blog.scaleflex.com/content/images/2022/12/content-and-media-delivery-networks.png)

![https://www.cloudflare.com/img/learning/cdn/glossary/edge-server/cdn-edge-network-device.png](https://www.cloudflare.com/img/learning/cdn/glossary/edge-server/cdn-edge-network-device.png)

![https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AHTJdj5QedEsiaFlvqX1R3Q.png](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AHTJdj5QedEsiaFlvqX1R3Q.png)

4

الـ CDN عبارة عن شبكة سيرفرات موزعة عالميًا:

- السيرفر الأساسي (Origin Server)
    
- Edge Servers قريبة من المستخدمين
    
- المستخدم يتصل بأقرب Edge بدل السيرفر الأساسي
    

---

## 🔐 الفوائد الأمنية للـ CDN

### 1️⃣ IP Masking

- يخفي IP الحقيقي للسيرفر
    
- يقلل فرص الاستهداف المباشر
    

---

### 2️⃣ DDoS Protection

- يمتص كميات ضخمة من الترافيك
    
- يمنع إغراق السيرفر الأصلي
    

---

### 3️⃣ Enforced HTTPS

- تفعيل TLS تلقائيًا
    
- منع Man-in-the-Middle attacks
    

---

### 4️⃣ Integrated WAF

أمثلة على CDNs تحتوي WAF مدمج:

- Cloudflare
    
- Amazon CloudFront
    
- Azure Front Door
    

---

🎯 الخلاصة:  
الـ CDN ليس فقط لتحسين السرعة — بل طبقة حماية قوية أمام السيرفر.

---

# 🚧 Web Application Firewall (WAF)

WAF يعمل كـ "بواب" أمام السيرفر.

أي Request لازم يمر عليه قبل ما يوصل للتطبيق.

---

## 🔎 أنواع WAF

### 1️⃣ Cloud-Based (Reverse Proxy)

- أمام السيرفر
    
- سهل الإعداد
    
- عالي القابلية للتوسع
    

---

### 2️⃣ Host-Based

- Software مثبت على نفس السيرفر
    
- تحكم دقيق لكل تطبيق
    

---

### 3️⃣ Network-Based

- جهاز فعلي أو افتراضي على حدود الشبكة
    
- يستخدم في البيئات المؤسسية
    

---

## 🧠 كيف يكتشف WAF الهجمات؟

|Feature|طريقة الكشف|مثال|
|---|---|---|
|Signature-Based|يطابق أنماط معروفة|User-Agent = sqlmap/1.8.1|
|Heuristic-Based|تحليل السياق|search?q=%3Cscript%20|
|Behavioral Analysis|سلوك غير طبيعي|200 محاولة تسجيل دخول في دقيقة|
|IP Reputation|سمعة وعنوان IP|IP من دولة خارج نشاط الشركة|

---

## مثال عملي

لو ظهر:

GET /login.php?id=1' OR '1'='1

WAF سيمنعه لأنه:

- يحتوي SQL Injection pattern
    

---

## مثال لوحة تحكم

![https://cf-assets.www.cloudflare.com/zkvhlag99gkb/iyOx4HWAdpFyp0W6nECvi/5f67090ee17c9db87ce2c130f80d493a/image5.png](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/iyOx4HWAdpFyp0W6nECvi/5f67090ee17c9db87ce2c130f80d493a/image5.png)

![https://www.claudiokuenzler.com/graph/news/1222-blocked-by-cloudflare.png](https://www.claudiokuenzler.com/graph/news/1222-blocked-by-cloudflare.png)

![https://developers.cloudflare.com/_astro/security-analytics-sampled-logs.CwY4DcKL_ZQNND4.webp](https://developers.cloudflare.com/_astro/security-analytics-sampled-logs.CwY4DcKL_ZQNND4.webp)

4

لوحة تحكم مثل الموجودة في Cloudflare تظهر:

- إجمالي الطلبات
    
- الطلبات المحظورة
    
- الطلبات المخففة (Mitigated)
    

---

# 🦠 Antivirus (AV)

الـ AV يحمي:

- السيرفر
    
- أجهزة المستخدمين
    
- أي Endpoint
    

---

## كيف يعمل؟

معظم AVs تعتمد على:

### Signature-Based Detection

- مقارنة الملفات بقاعدة بيانات Malware معروفة
    

---

## لماذا مهم رغم أن الهجوم Web-Based؟

لأنه يمكنه كشف:

- Web Shell uploads
    
- Reverse Shell tools
    
- Post-exploitation scripts
    
- Malicious executables
    

---

# 🎯 Defense-in-Depth

لا تعتمد على أداة واحدة.

الطبقات المثالية:

User  
 ↓  
CDN  
 ↓  
WAF  
 ↓  
Web Server  
 ↓  
Host (AV + Hardening)

كل طبقة تقلل احتمال نجاح الهجوم.

---

# 🔥 مقارنة سريعة

| الأداة | تحمي من                | مستوى       |
| ------ | ---------------------- | ----------- |
| CDN    | DDoS + IP Exposure     | Network     |
| WAF    | SQLi, XSS, Brute Force | Application |
| AV     | Malware & Web Shell    | Host        |