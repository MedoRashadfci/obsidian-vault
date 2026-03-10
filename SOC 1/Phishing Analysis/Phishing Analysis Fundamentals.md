
## أولًا: الفرق بين Spam و Phishing

### 🔹 Spam

- رسائل غير مرغوب فيها.
    
- غالبًا إعلانات أو تسويق.
    
- الهدف: إزعاج أو ترويج.
    

### 🔹 Phishing

- هجوم هندسة اجتماعية.
    
- الهدف: سرقة بيانات (Passwords – OTP – Credit Cards).
    
- ممكن يحتوي على:
    
    - رابط مزيف
        
    - ملف مرفق خبيث
        
    - صفحة تسجيل دخول مزورة
        

---

## ليه الإيميل خطير جدًا؟

لأن الإيميل:

- أسهل وسيلة للوصول للمستخدم.
    
- يعتمد على **العنصر البشري**.
    
- ممكن يتخطى كل أنظمة الحماية لو المستخدم ضغط بنفسه.
    

وده اللي بنسميه:

> Human is the weakest link 🔥

---

# 📧 مكونات عملية إرسال الإيميل

## 1️⃣ عميل البريد (Mail Client)

زي:

- Microsoft Outlook
    
- Mozilla Thunderbird
    

هو البرنامج اللي المستخدم بيكتب منه الرسالة.

---

## 2️⃣ بروتوكول الإرسال – SMTP

Simple Mail Transfer Protocol

هو المسؤول عن:

- إرسال الرسائل من جهازك لسيرفر البريد.
    
- نقل الرسائل بين السيرفرات.
    

يشتغل غالبًا على:

- Port 25
    
- Port 587
    
- Port 465 (SSL)
    

---

## 3️⃣ بروتوكولات الاستقبال

### 🔹 POP3

Post Office Protocol  
بيحمل الإيميل على الجهاز ويمسحه من السيرفر.

### 🔹 IMAP

Internet Message Access Protocol  
بيخلي الرسائل موجودة على السيرفر وبيعمل Sync.

---

# 📬 رحلة الإيميل عبر الإنترنت

![https://media2.dev.to/dynamic/image/width%3D1000%2Cheight%3D420%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3ekbn6ayq0o95mke6rmn.png](https://media2.dev.to/dynamic/image/width%3D1000%2Cheight%3D420%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F3ekbn6ayq0o95mke6rmn.png)

![https://www.zohowebstatic.com/sites/zweb/images/mail/help/how-email-works.jpg](https://www.zohowebstatic.com/sites/zweb/images/mail/help/how-email-works.jpg)

![https://www.researchgate.net/publication/41054977/figure/fig3/AS%3A667607283798016%401536181388019/SMTP-Client-Flowchart.png](https://www.researchgate.net/publication/41054977/figure/fig3/AS%3A667607283798016%401536181388019/SMTP-Client-Flowchart.png)

4

### الخطوات:

1. المهاجم يكتب الإيميل.
    
2. الإيميل يروح لسيرفر SMTP.
    
3. السيرفر يحدد MX Record بتاع الدومين المستهدف.
    
4. يوصل لسيرفر الضحية.
    
5. يدخل Inbox.
    

---

# 🛡 ليه الفلاتر مش دايمًا تمنع الهجوم؟

لأن المهاجم ممكن:

- يستخدم Domain شبيه (typosquatting)
    
- يعمل Spoofing
    
- يمرر SPF
    
- يستخدم Email Compromise
    
- يشفر الـ Payload
    

---

# 🎯 دورك كـ Security Analyst

لما يوصلك إيميل مشبوه، لازم تحلل:

## 1️⃣ Email Header

الـ Header هو:

> سجل كامل لرحلة الإيميل.

بيحتوي على:

- From
    
- Return-Path
    
- Received
    
- Message-ID
    
- SPF
    
- DKIM
    
- DMARC
    

---

## 2️⃣ أهم حاجات تراجعها

### 🔍 1. الـ IP الحقيقي

من سطر:

`Received:`

### 🔍 2. هل فيه Spoofing؟

- Compare:
    
    - From
        
    - Return-Path
        
    - Reply-To
        

### 🔍 3. تحقق من:

- SPF Result
    
- DKIM Signature
    
- DMARC Policy
    

---

# 🚨 مثال عملي لهجوم Phishing

مهاجم يبعث:

> "Your account has been suspended. Click here to verify."

الرابط يكون:

`micr0soft-support.com`

مش:

`microsoft.com`

وده اسمه:  
Typosquatting

الشخص اللي اخترع مفهوم البريد الإلكتروني واستخدم رمز **@** لأول مرة هو:

**Ray Tomlinson**

- سنة 1971 تقريبًا
    
- أثناء عمله على شبكة  
    **ARPANET**
    
- هو اللي قرر يستخدم علامة **@** عشان يفصل بين:
    
    - اسم المستخدم
        
    - واسم الجهاز / الدومين
        

ومن هنا بدأت فكرة الإيميل بالشكل اللي نعرفه.

---

# 📧 مكونات عنوان البريد الإلكتروني

أي Email Address بيتكوّن من 3 أجزاء:

`username@domain.com`

## 1️⃣ User Mailbox (Username)

مثال:

`billy`

ده يمثل:

- اسم الحساب
    
- صندوق البريد داخل السيرفر
    

---

## 2️⃣ علامة @

الفاصل بين:

- المستخدم
    
- الدومين
    

ومعناها حرفيًا:

> billy **at** johndoe.com

---

## 3️⃣ Domain

مثال:

`johndoe.com`

ده بيمثل:

- سيرفر البريد
    
- الشركة / المنظمة
    
- المكان اللي فيه صندوق البريد
    

---

# 🏠 تشبيه بسيط (Street Analogy)

زي ما انت ساكن في:

`Apartment 12, 10 Tahrir Street`

- **Tahrir Street** → هو الـ Domain
    
- **Apartment 12** → هو الـ Mailbox
    

البريد يوصل صح لأن العنوان كامل.

---

# 🔎 من منظور أمني (مهم ليك جدًا)

كـ Security Analyst لازم تبص على:

## ✅ هل الدومين حقيقي؟

مثال:

`support@micr0soft.com`

بدل:

`support@microsoft.com`

ده اسمه:  
Typosquatting

---

## ✅ هل فيه Subdomain مضلل؟

مثال:

`security.microsoft.verify-login.com`

الدومين الحقيقي هنا هو:

`verify-login.com`

مش microsoft.

---

## ✅ هل فيه Domain Spoofing؟

مثال:

- From: support@bank.com
    
- لكن في الـ Header تلاقي السيرفر الحقيقي جاي من دولة غريبة.



خلينا نفهم **Email Delivery** بشكل عملي + أمني.

---

# 📡 البروتوكولات الثلاثة المسؤولة عن الإيميل

## 1️⃣ SMTP – الإرسال

Simple Mail Transfer Protocol

- مسؤول عن **إرسال** الرسائل.
    
- ينقل الرسالة من:
    
    - Client → Mail Server
        
    - Mail Server → Mail Server
        

### 🔹 البورتات:

- 25 (تقليدي)
    
- 587 (Submission)
    
- 465 (SMTPS)
    

> مهم: SMTP لا يقوم بجلب الرسائل، فقط إرسالها.

---

## 2️⃣ POP3 – التحميل المحلي

Post Office Protocol

### بيعمل إيه؟

- ينزل الرسائل من السيرفر للجهاز.
    
- يخزنها على جهاز واحد فقط.
    
- غالبًا يحذفها من السيرفر بعد التحميل.
    

### البورتات:

- 110
    
- 995 (SSL)
    

### عيوبه:

- لو غيرت جهازك → مش هتلاقي الإيميلات.
    
- مش مناسب للبيئات المؤسسية الحديثة.
    

---

## 3️⃣ IMAP – المزامنة

Internet Message Access Protocol

### بيعمل إيه؟

- يخلي الرسائل على السيرفر.
    
- يسمح بالمزامنة بين عدة أجهزة.
    
- يخزن Sent mail على السيرفر.
    

### البورتات:

- 143
    
- 993 (SSL)
    

### ده المستخدم حاليًا في:

- Gmail
    
- Outlook
    
- بيئات الشركات
    

---

# 🔥 الفرق الأمني المهم بين POP3 و IMAP

|نقطة المقارنة|POP3|IMAP|
|---|---|---|
|التخزين|جهاز واحد|السيرفر|
|المزامنة|لا|نعم|
|التحقيق الجنائي|أصعب|أسهل|

💡 كـ DFIR Analyst:

- IMAP أسهل في التحقيق لأن الرسائل محفوظة على السيرفر.
    
- POP3 ممكن تضيع الأدلة لو المستخدم حذف الجهاز.
    

---

# 📬 رحلة الإيميل من Alexa إلى Billy

![https://cdn.prod.website-files.com/644fc991ce69ff211edbeb95/6784e2f1ada454ed0dee61b5_6784df6df41a3ae516b22e52_image3.png](https://cdn.prod.website-files.com/644fc991ce69ff211edbeb95/6784e2f1ada454ed0dee61b5_6784df6df41a3ae516b22e52_image3.png)

![https://www.zohowebstatic.com/sites/zweb/images/mail/help/how-email-works.jpg](https://www.zohowebstatic.com/sites/zweb/images/mail/help/how-email-works.jpg)

![https://afterlogic.com/mailbee-net/docs/send_with_relay.png](https://afterlogic.com/mailbee-net/docs/send_with_relay.png)

4

خلينا نمشي خطوة خطوة:

---

## 1️⃣ Alexa تكتب الرسالة

في Mail Client وتضغط Send.

---

## 2️⃣ SMTP Server يسأل DNS

يسأل:

> فين سيرفر البريد الخاص بـ johndoe.com؟

عن طريق:  
MX Record

---

## 3️⃣ DNS يرد بمعلومة الـ MX

يعني:

`mail.johndoe.com`

---

## 4️⃣ الرسالة تمشي عبر الإنترنت

تمر عبر:

- عدة SMTP Relay Servers
    
- أحيانًا Spam Filters
    
- أحيانًا Secure Email Gateways
    

---

## 5️⃣ توصل للسيرفر النهائي

Destination SMTP Server يستقبل الرسالة.

---

## 6️⃣ تتخزن في Mailbox

توضع في:

- IMAP Server  
    أو
    
- POP3 Server
    

---

## 7️⃣ Billy يعمل Login

Client يعمل Query للسيرفر.

---

## 8️⃣ الرسالة تتحمل

- IMAP → Copy
    
- POP3 → Download
    

---

# 🛡 من منظور SOC (مهم جدًا ليك)

في تحليل Email Incident:

### لازم تسأل نفسك:

- هل السيرفر اللي أرسل الرسالة هو نفس MX الحقيقي؟
    
- هل فيه Relay مشبوه؟
    
- هل الرسالة مرت عبر دولة غير متوقعة؟
    
- هل SMTP بدأ من IP غير مصرح به؟
    

---

# 🎯 نقطة مهمة ليك يا محمود

بما إنك شغال:

- Network Monitoring
    
- Wazuh
    
- Packet Analysis
    
- CTF Forensics
    

تحليل SMTP traffic في Wireshark بيكون مهم جدًا.

فلتر مفيد:

`smtp`

أو:

`tcp.port == 25`

---

# 📌 ملخص سريع

|البروتوكول|الوظيفة|
|---|---|
|SMTP|إرسال|
|POP3|تحميل|
|IMAP|مزامنة|

---

# 📧 Email Headers

وده الجزء اللي بيفرق بين User عادي و SOC Analyst محترف 🔥

---

# ✉️ تركيب الإيميل

أي رسالة إيميل بتتكوّن من جزئين:

## 1️⃣ Email Header

معلومات تقنية عن:

- مين بعت الرسالة
    
- عدّت على أنهي سيرفرات
    
- نتائج التحقق (SPF/DKIM/DMARC)
    
- IP المصدر
    

## 2️⃣ Email Body

- النص
    
- HTML
    
- روابط
    
- مرفقات
    

---

# 📜 صيغة الإيميل الرسمية

المعيار المستخدم هو:

Internet Message Format

وده بيحدد شكل وترتيب الـ Headers.

---

# 🔎 أول حاجات تبص عليها في التحليل

## الحقول الظاهرة للمستخدم:

- **From**
    
- **To**
    
- **Subject**
    
- **Date**
    

⚠️ مهم: الحقول دي ممكن تتزور بسهولة.

---

# 🧠 التحليل الحقيقي بيبدأ من Raw Header

لازم تفتح:

> View Raw Message  
> أو  
> Show Original

---

# 🛑 أهم الحقول الأمنية اللي تركز عليها

## 1️⃣ Received

ده أهم سطر في الهيدر.

كل سيرفر الإيميل مر عليه بيضيف سطر:

`Received: from ...`

📌 تقرأهم من **أسفل لأعلى**  
لأن أول سطر تحت هو بداية الرحلة.

---

## 2️⃣ X-Originating-IP

ده بيوضح:

> الـ IP الحقيقي اللي الرسالة خرجت منه

مثال:

`X-Originating-IP: [185.x.x.x]`

⚠️ لكن أحيانًا بيكون متلاعب فيه.

---

## 3️⃣ Authentication-Results

بيحتوي على نتائج:

- SPF
    
- DKIM
    
- DMARC
    

لو لقيت:

`spf=fail dkim=fail dmarc=fail`

🚨 ده مؤشر قوي على Spoofing

---

## 4️⃣ Reply-To

واحدة من أشهر حيل الـ Phishing.

مثال:

`From: newsletters@ant.anki-tech.com Reply-To: reply@ant.anki-tech.com`

لو المستخدم عمل Reply → هيروح لعنوان مختلف.

⚠️ في هجمات BEC بيكون:

- From = اسم CEO
    
- Reply-To = إيميل مهاجم خارجي
    

---

# 📊 شكل الهيدر بيكون عامل إزاي

![https://www.mailercheck.com/img/containers/gcs/articles/email-header-apple-mail.png/527d5acf28372c1ebc543dd63222dc43.png](https://www.mailercheck.com/img/containers/gcs/articles/email-header-apple-mail.png/527d5acf28372c1ebc543dd63222dc43.png)

![https://res.cloudinary.com/dbulfrlrz/images/w_759%2Ch_468%2Cc_scale/f_auto%2Cq_auto/v1714576273/wp-pme/visible-email-header_32466e1ac5/visible-email-header_32466e1ac5.png?_i=AA](https://res.cloudinary.com/dbulfrlrz/images/w_759%2Ch_468%2Cc_scale/f_auto%2Cq_auto/v1714576273/wp-pme/visible-email-header_32466e1ac5/visible-email-header_32466e1ac5.png?_i=AA)

![https://faq.cyberimpact.com/images/articles/image/v2-en/DMARC-SPF-and-DKIM-authentication.png](https://faq.cyberimpact.com/images/articles/image/v2-en/DMARC-SPF-and-DKIM-authentication.png)

4

أول مرة تشوفه هيكون مرعب 😅  
لكن ركّز على الحقول المهمة بس.

---

# 🎯 منهج تحليل احترافي (تمشي عليه في أي Incident)

## Step 1️⃣

قارن:

- From
    
- Return-Path
    
- Reply-To
    

هل نفس الدومين؟

---

## Step 2️⃣

راجع:  
Authentication-Results

هل:

- SPF Pass؟
    
- DKIM Pass؟
    
- DMARC Pass؟
    

---

## Step 3️⃣

راجع أول IP في Received

- هل جاي من دولة غريبة؟
    
- هل IP Consumer ISP؟
    
- هل IP مدرج في Blacklist؟
    

---

## Step 4️⃣

افحص الدومين

- هل حديث الإنشاء؟
    
- هل فيه Typosquatting؟
    
- هل MX طبيعي؟



# 📧 Email Body

لو الـ Header هو “الهوية التقنية”،  
فالـ **Body هو سلاح الهجوم** 🔥

---

# 📨 ما هو Email Body؟

هو الجزء اللي يحتوي على:

- نص عادي (Plain Text)
    
- HTML
    
- صور
    
- روابط
    
- أزرار (Buttons)
    
- مرفقات
    

---

# ✍️ نوع 1: Plain Text Email

![https://framerusercontent.com/images/7pK8P6npY7wcyfCxj9XOHfCVyOc.png?height=1964&width=3840](https://framerusercontent.com/images/7pK8P6npY7wcyfCxj9XOHfCVyOc.png?height=1964&width=3840)

![https://www.researchgate.net/publication/241636077/figure/fig2/AS%3A298686061006849%401448223714036/Sample-email-plain-text-view.png](https://www.researchgate.net/publication/241636077/figure/fig2/AS%3A298686061006849%401448223714036/Sample-email-plain-text-view.png)

![https://www.researchgate.net/publication/335201448/figure/fig2/AS%3A792363177885696%401565925511886/A-simple-text-based-phishing-email-with-hyperlinks-The-look-is-copied-from-subscription.ppm](https://images.openai.com/static-rsc-1/l5EfJXW84Qu23L0pYnQB_f0gWV1INO6RdWYnvHJHHk29iLcZULomjiy3VpQyfQvX-VG8vZT4ughufPByRw-DlhO73kDOhYEHBNENsW-dau6aJLI5YmxYjwd4HR-2nTN6S792-OwQN-E6WLNyNCX55A)

### خصائصه:

- بدون تنسيق
    
- بدون أزرار
    
- بدون صور
    
- أقل تعقيدًا في التحليل
    

💡 غالبًا يُستخدم في:

- Spam بسيط
    
- رسائل آلية
    

---

# 🌐 نوع 2: HTML Email

![https://pic.vicinity.nl/image/12932826/990412e644eb6cc029e35657a38bf802/image.png](https://pic.vicinity.nl/image/12932826/990412e644eb6cc029e35657a38bf802/image.png)

![https://www.trendmicro.com/content/dam/trendmicro/global/en/migrated/security-intelligence-migration-spreadsheet/trendlabs-security-intelligence/2017/07/bec-phishing-9.png](https://www.trendmicro.com/content/dam/trendmicro/global/en/migrated/security-intelligence-migration-spreadsheet/trendlabs-security-intelligence/2017/07/bec-phishing-9.png)

![https://robert365.com/pictures/edithtml-macro-email-html-editor.png](https://robert365.com/pictures/edithtml-macro-email-html-editor.png)

4

### خصائصه:

- أزرار مثل: "Verify Now"
    
- صور
    
- تنسيق احترافي
    
- روابط مخفية
    

⚠️ ده الأكثر استخدامًا في Phishing.

---

# 🧠 لماذا HTML خطير؟

لأن المهاجم يقدر:

### 🔹 يخفي الرابط الحقيقي

مثال:

`<a href="http://evil-site.com">https://paypal.com</a>`

المستخدم يشوف:

`https://paypal.com`

لكن لما يضغط يروح:

`evil-site.com`

---

# 👁️‍🗨️ ازاي تشوف HTML Source؟

في أي Webmail:

- Show Original
    
- View Source
    
- View HTML
    
- View Rendered HTML
    

مثال:  
ProtonMail  
بيوفر خيار:

> View rendered HTML

---

# 📎 المرفقات (Attachments)

أخطر جزء في الـ Email Body.

---

## 🔍 داخل الـ Source Code هتلاقي:

### 1️⃣ Content-Type

مثال:

`Content-Type: application/pdf`

أو:

`Content-Type: application/vnd.ms-excel`

---

### 2️⃣ Content-Disposition

`Content-Disposition: attachment;`

بيوضح إنه ملف مرفق.

---

### 3️⃣ Content-Transfer-Encoding

`Content-Transfer-Encoding: base64`

معناه إن الملف متحوّل لنص مشفّر Base64.

---

# 🔐 مثال واقعي

في رسالة مزورة من "Netflix":

- صورة Logo
    
- زرار “Update Payment”
    
- مرفق PDF
    

الـ PDF بيكون مشفر Base64 داخل الـ Source.

---

# 🛑 نقطة أمنية مهمة جدًا

⚠️ لا تعمل Double Click على المرفقات.

في SOC:

- نعمل Download
    
- نفحص بـ Sandbox
    
- نفحص Hash
    
- نفحص بـ VirusTotal
    

---

# 🧪 في التحقيق الجنائي

لو لقيت:

`Content-Type: application/pdf Content-Transfer-Encoding: base64`

تقدر:

1. تنسخ الـ Base64
    
2. تفك التشفير
    
3. تحفظ الملف
    
4. تحلله
    

---

# 🎯 حاجات لازم تراجعها في Body

|العنصر|ليه مهم|
|---|---|
|الروابط|هل الدومين مزور؟|
|الأزرار|رايحة فين فعليًا؟|
|الصور|External tracking pixel؟|
|المرفقات|نوع الملف حقيقي؟|

---

# 🧠 خدعة مشهورة

ملف اسمه:

`Invoice.pdf.exe`

لكن Windows يخفي الامتداد الأخير  
فيظهر:

`Invoice.pdf`

🚨 وهو في الحقيقة Executable.



# 🎣 Types of Phishing

## 1️⃣ Spam / MalSpam

- رسائل عشوائية جماعية.
    
- الهدف: نشر روابط أو Malware.
    
- MalSpam = Spam يحتوي على Payload خبيث.
    

🚨 غالبًا يحتوي على:

- مرفقات Office
    
- روابط مختصرة
    
- JavaScript
    

---

## 2️⃣ Phishing

انتحال صفة جهة موثوقة مثل:

- Amazon
    
- PayPal
    
- Microsoft
    

الهدف:

- سرقة Credentials
    
- بيانات بطاقة
    
- OTP
    

---

## 3️⃣ Spear Phishing

هجوم موجه لشخص محدد.

مثال:

> “Hi Mahmoud, regarding your recent Wazuh deployment…”

💡 المهاجم بيجمع معلومات مسبقًا (OSINT).

---

## 4️⃣ Whaling

نسخة متقدمة من Spear Phishing.

يستهدف:

- CEO
    
- CFO
    
- CISO
    

غالبًا:

- Business Email Compromise (BEC)
    
- طلب تحويل أموال
    

---

## 5️⃣ Smishing

SMS Phishing.

مثال:

> Your bank account is suspended. Click here.

---

## 6️⃣ Vishing

Voice Phishing.

المهاجم يتصل ويقول:

> I'm from IT support…

---

# 🎯 الهدف من الهجوم عادةً

1. Credential Harvesting
    
2. Malware Delivery
    
3. Financial Fraud
    
4. Initial Access (Foothold)
    

---

# 🚨 الخصائص المشتركة لرسائل Phishing

خلينا نحللها بعين SOC:

---

## ✅ 1. Email Spoofing

From يبدو شرعي  
لكن Header يكشف الحقيقة.

---

## ✅ 2. Urgency

كلمات مثل:

- Invoice
    
- Suspended
    
- Urgent
    
- Payment Failed
    

ده بيضغط نفسيًا على الضحية.

---

## ✅ 3. HTML يشبه شركة حقيقية

لوجو رسمي  
ألوان مطابقة  
زرار "Verify Now"

---

## ✅ 4. صياغة ركيكة

أخطاء إملائية  
Grammar سيء  
تنسيق غريب

---

## ✅ 5. محتوى عام

`Dear Customer Dear Sir/Madam`

مش مخصص.

---

## ✅ 6. روابط مختصرة

مثل:

- bit.ly
    
- tinyurl
    

أو دومين مشبوه.

---

## ✅ 7. مرفقات خبيثة

أمثلة:

- Invoice.docm
    
- Payment.pdf.exe
    
- ShippingDetails.html
    

---

# 🔐 Defanging (مهم جدًا في الشغل)

لازم تحول الرابط لشكل غير قابل للنقر.

مثال:

بدل:

`http://malicious-site.com/login`

تكتب:

`hxxp[://]malicious-site[.]com/login`

الأدوات المفيدة:

- CyberChef