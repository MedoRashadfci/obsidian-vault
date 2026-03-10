
تعتبر الـ **Web Attacks** من أكثر الوسائل شيوعاً التي يستخدمها المهاجمون (**Attackers**) لاختراق الأنظمة المستهدفة (**Target Systems**). نظراً لأن المواقع والتطبيقات التي تواجه الجمهور (**Public-facing websites**) تقع غالباً كواجهة أمامية لقواعد البيانات (**Databases**) والبنى التحتية الأخرى، فإنها تشكل أهدافاً مغرية جداً.

في هذا القسم، ستتعلم كيفية تحديد هذه التهديدات (**Threats**) باستخدام طرق كشف (**Detection methods**) عملية وأدوات قياسية في المجال (**Industry-standard tools**).

---

## الأهداف التعليمية (Objectives)

إليك تفصيل للأهداف التي سيتم تناولها في هذا السياق:

- **تعلم أنواع الهجمات (Attack Types):**
    
    - **Client-side attacks:** الهجمات التي تستهدف المتصفح أو جهاز المستخدم (مثل XSS).
        
    - **Server-side attacks:** الهجمات التي تستهدف الخادم أو قاعدة البيانات مباشرة (مثل SQL Injection).
        
- **فهم الـ Log-based Detection:**
    
    - دراسة الفوائد (**Benefits**) التي توفرها سجلات النظام في تتبع الهجمات.
        
    - معرفة القيود (**Limitations**) التي قد تمنع السجلات من كشف كل شيء.
        
- **استكشاف الـ Network Traffic–based Detection:**
    
    - البحث في طرق الكشف التي تعتمد على مراقبة وتحليل حركة مرور البيانات في الشبكة (Network Traffic) لرصد الأنماط المشبوهة.
        
- **فهم دور الـ Web Application Firewalls (WAF):**
    
    - دراسة كيف ولماذا يتم استخدام جدران حماية تطبيقات الويب (**WAF**) لصد الهجمات الموجهة لبروتوكول HTTP/HTTPS.
        
- **الممارسة العملية (Practical Practice):**
    
    - تدريب فعلي على تحديد الـ **Common web attacks** باستخدام كافة الطرق والأساليب التي تم ذكرها سابقاً.


----------------------------------------------------------------------------



تخيل أنك تفتح كتاباً تقنياً يشرح كواليس ما يحدث في متصفحك؛ هذا القسم مخصص لفهم الـ **Client-Side Attacks**، حيث يركز المهاجمون على استغلال نقاط الضعف في سلوك المستخدم أو في جهازه الشخصي.

---

## مفهوم هجمات جهة العميل (Client-Side Attacks)

تعتمد هذه الهجمات بشكل أساسي على إساءة استخدام الثغرات في المتصفحات (**Browsers**) أو خداع المستخدم للقيام بإجراءات غير آمنة (**Unsafe actions**). الهدف غالباً هو الوصول إلى الحسابات أو سرقة المعلومات الحساسة المخزنة على جهاز المستخدم.

مع تزايد الطلب على تطبيقات الويب الديناميكية، أصبحت الشركات تدمج المزيد من الإضافات الخارجية (**Third-party plugins**)، مما أدى إلى توسيع مساحة الهجوم (**Attack surface**) داخل المتصفحات.

> **مثال توضيحي:** بينما تتصفح موقع تسوق وتضغط على صورة منتج، قد يكون هناك نافذة خبيثة مخفية (**Invisible window**) تعمل في الخلفية وتقوم بتشغيل كود لسرقة ملفات تعريف الارتباط الخاصة بجلسة تسجيل الدخول (**Session cookies**)، مما يسمح للمهاجم بانتحال شخصيتك (**Impersonate you**) دون أن تلاحظ أي شيء غريب.

---

## قيود مركز العمليات الأمنية (SOC Limitations)

يواجه المحلل الأمني في الـ **SOC** تحدياً كبيراً هنا؛ فالأدوات التقليدية مثل سجلات الخادم (**Server-side logs**) أو التقاط حركة مرور الشبكة (**Network traffic captures**) لا توفر رؤية واضحة لما يحدث داخل متصفح المستخدم.

- **Execution Location:** بما أن الهجوم يتم على نظام المستخدم، فإنه ينفذ الأكواد الخبيثة دون توليد طلبات **HTTP** مشبوهة يمكن للـ SOC رؤيتها.
    
- **Visibility:** الكشف عن هذه الهجمات من منظور الـ SOC غالباً ما يكون صعباً أو مستحيلاً دون وجود ضوابط أمنية على مستوى المتصفح أو مراقبة الأجهزة النهائية (**Endpoint monitoring**).
    

---

## أشهر هجمات جهة العميل (Common Client-Side Attacks)

|الهجوم|الوصف المختصر|
|---|---|
|**Cross-Site Scripting (XSS)**|هو الهجوم **الأكثر شيوعاً**؛ حيث يتم تشغيل سكربتات خبيثة داخل موقع موثوق وتنفيذها في متصفح المستخدم. إذا كان الموقع لا يقوم بتصفية المدخلات (**Filter input**)، يمكن للمهاجم حقن كود مثل `<script>alert('Hacked');</script>` لسرقة البيانات.|
|**Cross-Site Request Forgery (CSRF)**|يتم فيه خداع المتصفح لإرسال طلبات غير مصرح بها (**Unauthorized requests**) نيابة عن المستخدم الموثوق به.|
|**Clickjacking**|يقوم المهاجم بوضع عناصر غير مرئية (**Invisible elements**) فوق محتوى شرعي، مما يجعل المستخدم يعتقد أنه يتفاعل مع شيء آمن بينما هو ينفذ أمراً آخر تماماً.|

-------------------------------------------------------------------------


# 💣 يعني إيه Server-Side Attack؟

هو هجوم بيستهدف:

- 🖥 Web Server
    
- 🗄 Database
    
- ⚙ Backend logic
    
- 🔐 Authentication system
    

مش بيهاجم المستخدم…  
بيهاجم السيرفر نفسه.

---

# 🎯 الفرق بين Client-Side و Server-Side

|Client-Side|Server-Side|
|---|---|
|يهاجم المستخدم|يهاجم السيرفر|
|XSS مثال|SQLi مثال|
|يعتمد على الضحية|يعتمد على ضعف في التطبيق|
|صعب أحيانًا يتسجل|دايمًا تقريبًا له Logs|

---

# 🧨 أشهر Server-Side Attacks

---

## 1️⃣ Brute Force Attack

![https://www.researchgate.net/publication/379624465/figure/fig2/AS%3A11431281234681580%401712414089963/Work-architecture-of-Brute-Force-Attack7.ppm](https://www.researchgate.net/publication/379624465/figure/fig2/AS%3A11431281234681580%401712414089963/Work-architecture-of-Brute-Force-Attack7.ppm)

![https://discover.strongdm.com/hubfs/brute-force-attack.jpg](https://discover.strongdm.com/hubfs/brute-force-attack.jpg)

![https://user-images.githubusercontent.com/314009/101202310-b6e83980-362e-11eb-878b-953b510a5231.gif](https://user-images.githubusercontent.com/314009/101202310-b6e83980-362e-11eb-878b-953b510a5231.gif)

4

### الفكرة:

المهاجم يجرب:

- آلاف اليوزرنيم
    
- آلاف الباسوردات
    

باستخدام Tools زي:

- Hydra
    
- Burp Intruder
    

### هتشوف في الـ Logs:

POST /login  
status: 401  
POST /login  
status: 401  
POST /login  
status: 401

🔎 عدد محاولات فشل متكرر = Suspicious

### مثال واقعي:

اختراق T-Mobile سنة 2021 بسبب brute-force  
وأثر على 50+ مليون عميل.

---

## 2️⃣ SQL Injection (SQLi)

![https://www.spanning.com/blog/sql-injection-attacks-web-based-application-security-part-4/SQL-injection-attack-example.png](https://www.spanning.com/blog/sql-injection-attacks-web-based-application-security-part-4/SQL-injection-attack-example.png)

![https://portswigger.net/support/images/owasp_injection_12.png](https://images.openai.com/static-rsc-1/qXYeCJDKLEH1aIjhFoZ_8Ly2V8okkZOLbcUz5BPyfLVLdIJtwPWewMRiBlO0h5Gs-UgTdd0DB_tY1DuYuGY1htaabrNWgdhHducoPkX6fzDD0-rNbc9Zs55sfSRY0PjL0vYSdXKGxNbDqkg2PN6OPQ)

![https://www.researchgate.net/publication/363888650/figure/fig1/AS%3A11431281086740869%401664334291944/A-typical-illustration-of-SQL-injection-attack.png](https://www.researchgate.net/publication/363888650/figure/fig1/AS%3A11431281086740869%401664334291944/A-typical-illustration-of-SQL-injection-attack.png)

4

### بتحصل إزاي؟

لو الكود بيعمل كده:

SELECT * FROM users WHERE username = '$user' AND  password = '$pass'; 

المهاجم يكتب في خانة الباسورد:

' OR 1=1 --

فتبقى الاستعلام:

SELECT * FROM users WHERE username = 'admin' AND password = '' OR 1=1 --';

كده دخل بدون باسورد 🔥

### علامات في Logs:

- وجود:
    
    - `'`
        
    - `--`
        
    - `UNION`
        
    - `SELECT`
        
    - `OR 1=1`
        

### حادثة مشهورة:

ثغرة في MOVEit سنة 2023  
أثرت على 2700+ مؤسسة.

---

## 3️⃣ Command Injection

![https://portswigger.net/web-security/images/os-command-injection.svg](https://images.openai.com/static-rsc-1/FZIxx9gwHGabYAPDktrHUZUHLuysMApPnjNJ2Po8Dx0cLIz3MWXWyMJKP8YApiGeogEKJkwR3veo5eO-RS4fULAg2UNdz_HaekVv12QhZnIcEDe9s_Vlnql4eLHPBzTTaGezgcRSrd54X-mlVDhKAA)

![https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41598-024-74350-3/MediaObjects/41598_2024_74350_Fig1_HTML.png](https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41598-024-74350-3/MediaObjects/41598_2024_74350_Fig1_HTML.png)

![https://www.cobalt.io/hs-fs/hubfs/command-injection-vulnerability-example-3.png?height=376&name=command-injection-vulnerability-example-3.png&width=872](https://www.cobalt.io/hs-fs/hubfs/command-injection-vulnerability-example-3.png?height=376&name=command-injection-vulnerability-example-3.png&width=872)

4

### بتحصل لما:

الموقع يعمل حاجة زي:

system("ping " . $_GET['ip']);

المهاجم يكتب:

8.8.8.8; cat /etc/passwd

السيرفر ينفذ الأمرين 😱

### تلاحظ في Logs:

- وجود:
    
    - `;`
        
    - `&&`
        
    - `|`
        
    - `cat`
        
    - `whoami`
        
    - `id`
        

---

# 🛰 كيف نكتشف Server-Side Attacks؟

## 1️⃣ Web Logs

مثال Apache:

/var/log/apache2/access.log

راقب:

- Repeated login attempts
    
- Suspicious payloads
    
- Long query strings
    
- Unexpected HTTP methods
    

---

## 2️⃣ Network Traffic

باستخدام:

- Wireshark
    
- Zeek
    
- Suricata
    

هتشوف:

- High request rate
    
- Strange POST payloads
    
- Suspicious SQL keywords
    

---

# 🛡 ليه Server-Side Attacks ميزة للـ Blue Team؟

لأنها:

- بتسيب Logs
    
- بتعدي على Firewall
    
- بتعدي على IDS
    
- بتتسجل في SIEM
    

بعكس بعض Client-side attacks اللي ممكن تبقى أصعب.

---

# 🧠 ربطها بشغلك في SOC / Wazuh

لو أنت شغال على Wazuh:

- تعمل Rule detect:
    
    - 5 failed logins in 30 seconds
        
- تعمل Alert لو ظهر:
    
    - `UNION SELECT`
        
- تعمل Alert لو:
    
    - فيه `; cat /etc/passwd`
        

---

# 🔥 ملخص سريع

| الهجوم            | الهدف    | العلامة            |
| ----------------- | -------- | ------------------ |
| Brute Force       | حسابات   | محاولات كتير فاشلة |
| SQLi              | Database | `' OR 1=1 --`      |
| Command Injection | OS       | `;`, `&&`, `       |


# Log-Based Detection

الـ Logs تعتبر أداة مهمة جدًا لاكتشاف هجمات الويب.

كل طلب (Request) بيتبعت إلى السيرفر:

- بيسيب أثر في access logs
    
- أو error logs
    

من خلال مراجعة الـ logs، المدافعين يقدروا يلاحظوا أنماط تشير إلى:

- Scanning
    
- محاولات استغلال
    
- أو هجمات مختلفة
    

في المهمة دي:

- هتتعلم شكل access log
    
- وتشوف إزاي الهجمات بتظهر جواه
    
- وبعد كده تطبق ده في سيناريو هجوم حقيقي
    

---

# Access Log Format

رغم إن شكل الـ logs بيختلف حسب السيرفر، غالبًا بيحتوي على المعلومات دي:

## 1️⃣ Client IP Address

- لو IP معروف إنه خبيث
    
- أو جاي من منطقة جغرافية غير متوقعة
    

## 2️⃣ Timestamp and Requested Page

- طلبات في وقت غير معتاد
    
- أو طلبات متكررة جدًا في وقت قصير
    

## 3️⃣ Status Code

- تكرار 404 معناه إن الصفحة مش موجودة
    

## 4️⃣ Response Size

- حجم رد صغير جدًا أو كبير جدًا مقارنة بالطبيعي
    

## 5️⃣ Referrer

- صفحة إحالة مش منطقية أو مش ضمن التنقل الطبيعي للموقع
    

## 6️⃣ User-Agent

- متصفح قديم جدًا
    
- أو أدوات هجوم مثل:
    
    - sqlmap
        
    - wpscan
        

---

# Attacks in Logs

في المثال المذكور، المعروض هو طلبات المهاجم فقط.  
لكن في الواقع، الطلبات الطبيعية بتكون الأغلبية، لذلك لازم تقدر تميز النشاط الخبيث وسط الطبيعي.

في المثال:

- الـ SQL Injection query string ظاهر بالكامل في اللوج.
    

---

## تسلسل الهجوم في المثال

### 1️⃣ Directory Fuzzing

المهاجم بيجرب مجلدات وصفحات مختلفة.

- وجود Status Code 200 معناه إنه لقى صفحة موجودة.
    

---

### 2️⃣ Brute Force على login.php

- تكرار POST requests بسرعة
    
- أغلبها بفشل
    
- آخر طلب كان Status Code 302 Found
    

302 هنا معناها:

- تسجيل الدخول نجح
    
- وتم إعادة التوجيه إلى `/account`
    

---

### 3️⃣ SQL Injection على /search

بعد ما دخل الحساب:

- جرب Payloads زي:
    
    - `' OR '1'='1`
        
    - `1' OR 'a'='a`
        

لو التطبيق بيبني استعلامات SQL بطريقة ديناميكية بدون parameterised queries:

- ممكن المهاجم يسحب بيانات قاعدة البيانات
    

---

# Log Limitations

رغم إن access logs مفيدة، ليها حدود.

مثال:

10.10.10.100 [12/Aug/2025:14:32:10] "POST /login HTTP/1.1" 200 532 "/home.html" "Mozilla/5.0"

اللي ظاهر:

- نوع الطلب (POST)
    
- الصفحة المطلوبة
    
- Status code
    

اللي مش ظاهر:

- بيانات تسجيل الدخول
    
- محتوى الـ POST body
    

الـ access logs:

- لا تسجل بيانات الـ POST الفعلية
    
- ممكن تسجل query string في GET
    
- وأحيانًا لا تسجلها حسب إعدادات السيرفر
    

يعتمد ذلك على:

- نوع السيرفر
    
- إعدادات الـ logging



------------------------------------------------------------------

# Network-Based Detection

تحليل حركة الشبكة (Network Traffic Analysis) بيسمح للمحللين إنهم يشوفوا البيانات الخام المتبادلة بين:

- Client
    
- Server
    

عن طريق التقاط وتحليل الـ Packets، المحلل يقدر يشوف:

- سلوك الهجوم بتفاصيل أدق
    
- بروتوكولات النقل المستخدمة
    
- بيانات التطبيق نفسها
    

الـ Network Captures بتكون أكثر تفصيلاً من الـ Server Logs، لأنها ممكن تظهر:

- HTTP Headers كاملة
    
- POST body
    
- Cookies
    
- ملفات تم رفعها أو تحميلها
    

---

## ملاحظة عن التشفير

البروتوكولات المشفرة زي:

- HTTPS
    
- SSH
    

بتمنع رؤية محتوى الـ Payload بدون مفاتيح فك التشفير.

في المثال هنا، التركيز على HTTP فقط.

---

# Attacks in Network Traffic

بيتم مقارنة تسلسل الهجوم السابق داخل Wireshark.

تم استخدام فلتر:

- عنوان الوجهة 10.10.20.200
    
- وفلتر http.user_agent
    

---

## تسلسل الأحداث

1️⃣ Directory Fuzz  
2️⃣ Brute Force (الباكيت 13 كان تسجيل دخول ناجح)  
3️⃣ SQL Injection

---

# تحليل Brute Force في الشبكة

في اللوجات شفنا:

- POST متكرر على login.php
    

لكن في تحليل الشبكة:

- نقدر نشوف اسم المستخدم وكلمة المرور الفعلية
    

في الباكيت 13:

- تم العثور على كلمة المرور الصحيحة: password123
    
- وكانت لحساب Admin
    

---

# تحليل SQL Injection

عند فحص الباكيت:

- يظهر Payload المستخدم بوضوح
    
- مثال: `' OR '1'='1`
    

النتيجة:

- تم عرض بيانات جدول Users
    
- First name و Surname ظهروا كنص واضح
    

تم التنبيه أيضًا إن:

- بروتوكول MySQL يمكن تحليله في Wireshark
    
- ويمكن رؤية الـ Payload والنتائج






# Web Application Firewall (WAF)

الـ **WAF** هو خط الدفاع الأول للمواقع وتطبيقات الويب.

وظيفته:

- فحص طلبات الويب بالكامل
    
- تحليلها قبل ما توصل للسيرفر
    
- السماح بالطلب أو منعه
    

هو بيشتغل كـ Gatekeeper بين:

- المستخدم
    
- والسيرفر
    

وبيفحص الطلبات بشكل مشابه لـ Wireshark،  
لكن يقدر كمان يفك تشفير TLS ويصفي الترافيك قبل ما يدخل على التطبيق.

---

# Rules (القواعد)

الـ WAF بياخد قراره (يسمح أو يمنع) بناءً على قواعد محددة مسبقًا.

أنواع القواعد:

## 1️⃣ Block common attack patterns

- يمنع Payloads معروفة بأنها خبيثة
    
- مثال: منع User-Agent يحتوي على sqlmap
    

---

## 2️⃣ Deny known malicious sources

- يستخدم سمعة الـ IP
    
- أو Threat Intelligence
    
- أو Geo-blocking
    

مثال:

- منع IPs من حملات Botnet حديثة
    

---

## 3️⃣ Custom-built rules

- قواعد مخصصة حسب احتياجات التطبيق
    

مثال:

- السماح فقط بطلبات GET و POST على /login
    

---

## 4️⃣ Rate-limiting & abuse prevention

- يحد من عدد الطلبات
    
- لمنع الاستغلال أو الهجوم
    

مثال:

- السماح بـ 5 محاولات تسجيل دخول في الدقيقة لكل IP
    

---

# مثال عملي

لو لاحظت:

- طلبات GET متكررة على /changeusername
    
- User-Agent = sqlmap/1.9
    
- والطلبات تحتوي على SQL Injection Payloads
    

يمكن إنشاء قاعدة:

If User-Agent contains "sqlmap"  
then BLOCK

تم توضيح إن:

- هذا مثال بسيط
    
- والـ WAF الحديث يكتشف هذه الأنماط تلقائيًا
    

---

# Challenge-Response Mechanisms

الـ WAF مش دايمًا بيمنع الطلب مباشرة.

ممكن:

- يطلب CAPTCHA
    
- عشان يتأكد إن الطلب من إنسان مش Bot
    

وده مهم لأن:

- 37% من حركة الويب عالميًا هي Bot Traffic
    

هذا الأسلوب مفيد عندما:

- القاعدة ممكن تمنع مستخدمين حقيقيين
    

---

# مثال لقواعد مخصصة

- قاعدة تمنع IP 10.10.10.100
    
- قاعدة تمنع User-Agent = Hydra
    
- قاعدة تتحدى (Challenge) الطلبات القادمة من خارج أمريكا والاتحاد الأوروبي
    

---

# Integrating Known Indicators and Threat Intelligence

الـ WAF الحديث:

- يحتوي على قواعد جاهزة لمعالجة مخاطر OWASP Top 10
    
- يستخدم Threat Intelligence Feeds
    
- يمنع IPs معروفة بأنها خبيثة
    
- يمنع User-Agents مشبوهة
    
- يتم تحديثه باستمرار ضد:
    
    - تهديدات جديدة
        
    - مجموعات APT
        
    - CVEs حديثة
        

تم ذكر مثال:  
Cloudflare يحتفظ بقوائم IPs محدثة من:

- Botnets
    
- VPNs
    
- Anonymizers
    
- Malware
    

بناءً على Threat Intelligence عالمي.