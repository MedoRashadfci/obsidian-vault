## ما هو MITM؟

هجوم **Man-in-the-Middle (MITM)** هو هجوم سيبراني يقوم فيه المهاجم باعتراض الاتصال بين طرفين (مثل مستخدم وخدمة) بشكل سري، وقد يقوم بتعديل الاتصال دون علمهم.

يمكن للمهاجم أن:

- يتجسس لسرقة بيانات حساسة (Credentials – Credit Cards)
    
- يحقن محتوى خبيث
    
- يعدّل ردود المواقع
    
- ينشئ نماذج تسجيل دخول مزيفة
    

تستهدف هذه الهجمات الأفراد أو المؤسسات، خصوصًا عندما يكون **التشفير أو المصادقة ضعيفين**.

---

## كيف تعمل هجمات MITM؟

تتكون عادة من مرحلتين رئيسيتين:

### 1️⃣ Interception (الاعتراض)

يقوم المهاجم بإدخال نفسه داخل مسار الاتصال عن طريق استغلال ضعف في بروتوكولات الشبكة، باستخدام تقنيات مثل:

- ARP Spoofing
    
- DNS Spoofing
    
- IP Spoofing
    

---

### 2️⃣ Manipulation / Decryption (التلاعب أو فك التشفير)

بعد الاعتراض، يحاول المهاجم:

- الوصول إلى البيانات
    
- فك تشفير البيانات
    
- تعديل المحتوى
    
- حقن محتوى خبيث (مثل نماذج تسجيل دخول مزيفة)
    

---

# الأنواع الشائعة لهجمات MITM

- **Packet Sniffing**  
    التقاط البيانات غير المشفرة عبر الشبكة (خصوصًا Wi-Fi المفتوح).
    
- **Session Hijacking**  
    سرقة Session Tokens وانتحال شخصية المستخدم.
    
- **SSL Stripping**  
    تخفيض الاتصال من HTTPS إلى HTTP غير آمن.
    
- **DNS Spoofing**  
    إعادة توجيه المستخدم إلى نطاق مزيف عبر التلاعب بردود DNS.
    
- **IP Spoofing**  
    إنشاء حزم IP تبدو وكأنها صادرة من أنظمة موثوقة.
    
- **Rogue Wi-Fi Access Point**  
    إنشاء شبكة مزيفة لاعتراض الترافيك.
    

---

# أمثلة واقعية

- في عام 2017، تعرضت **Equifax** لاختراق كبير بسبب هجوم MITM، مما أدى إلى تسريب بيانات أكثر من 100 مليون مستخدم.
    
- هجمات أخرى تضمنت حقن أكواد من قبل مزودي خدمة الإنترنت وجهات حكومية عبر SSL spoofing.
    

---

# MITM وعلاقته بـ Cyber Kill Chain

الـ **Cyber Kill Chain** يتكون من سبع مراحل:

1. Reconnaissance
    
2. Weaponization
    
3. Delivery
    
4. Exploitation
    
5. Installation
    
6. Command & Control (C2)
    
7. Actions on Objectives
    

---

## موقع MITM داخل الـ Kill Chain

يُستخدم MITM بشكل أساسي في مرحلتين:

### 🔹 كـ Exploitation Technique

- يستغل ثقة وبنية بروتوكولات مثل ARP و DNS
    
- يعترض الجلسة
    
- يخترق سلامة الشبكة
    
- يمنح المهاجم موطئ قدم أولي للتجسس أو التلاعب
    

---

### 🔹 كـ Installation Vector

بعد تثبيت موقع MITM، يمكن للمهاجم:

- حقن Exploit في المتصفح
    
- إدخال Malware Dropper
    
- زرع RAT
    
- نشر أدوات خبيثة إضافية
    

وهذا يتوافق مع مرحلة Installation حيث يتم تثبيت أدوات أو تحقيق استمرارية.

---

# لماذا اكتشاف MITM مهم؟

اكتشاف MITM يعني أن المهاجم:

- موجود فعليًا في منتصف الهجوم
    
- يعمل في المراحل المتوسطة من الاختراق
    
- لم يصل بعد إلى الهدف النهائي
    

وهنا تكون فرصة الـ SOC لإيقاف سلسلة الهجوم قبل تنفيذ الأهداف النهائية مثل:

- تسريب البيانات
    
- التدمير
    
- الحركة الجانبية
    

---


# 🕵️ Detecting ARP Spoofing – Practical Investigation

## أولًا: فهم البروتوكول

### 🔹 ما هو ARP؟

ARP = Address Resolution Protocol  
وظيفته يربط:

> IP Address ⟀ MAC Address

لما جهاز عايز يكلم 192.168.10.1  
يسأل:  
**Who has 192.168.10.1? Tell 192.168.10.X**

الراوتر يرد:  
**192.168.10.1 is at AA:BB:CC:DD:EE:FF**

---

# 🚨 كيف يعمل ARP Spoofing؟

المهاجم يبعث رد مزيف:

> 192.168.10.1 is at 02:fe:BB:cd:55:55

رغم إن ده مش MAC الحقيقي للـ Gateway.

النتيجة:

- ARP Cache عند الضحية يتسمم
    
- كل الترافيك يمر عبر المهاجم
    
- يحصل MITM
    

---

# 🔎 التحقيق العملي في Wireshark

## 1️⃣ عزل بروتوكول ARP

`arp`

ده يفلتر كل ARP packets  
نشوف الحجم + الأنماط

---

## 2️⃣ تحليل ARP Requests

`arp.opcode == 1`

Opcode 1 = Who-has (Request)

نشوف:

- هل فيه flood؟
    
- هل فيه probing متكرر؟
    

---

## 3️⃣ تحليل ARP Replies

`arp.opcode == 2`

Opcode 2 = is-at (Reply)

⚠️ هنا تبدأ الشبهة.

لو شفت:

- Replies بدون Requests
    
- تكرار إعلان نفس الـ IP
    
- MAC مش معروف
    

ده مؤشر قوي.

---

## 4️⃣ كشف Gratuitous ARP

`arp.isgratuitous`

Gratuitous ARP = رد بدون طلب

المهاجم يستخدمه عشان:

- يحافظ على حالة التسمم
    
- يجدد Poisoning باستمرار
    

---

# 🎯 التحقيق على Gateway

المعلومات المعروفة:

|Role|IP|
|---|---|
|Gateway|192.168.10.1|

فلتر:

`arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1`

المفروض:  
IP ده يطلع من MAC واحد بس.

لكن ظهر:

- نفس IP
    
- من MAC مختلف (02:fe:...)
    

ده دليل انتحال.

---

## فلتر تأكيد مباشر

`arp.opcode == 2 && _ws.col.info contains "192.168.10.1 is at"`

ده بيعرض كل الإعلانات الخاصة بالـ Gateway IP.

لو لقيت:  
192.168.10.1 is at 02:fe:...

يبقى ده المهاجم.

---

# 🔬 تأكيد عبر Duplicate Detection

`arp.duplicate-address-detected || arp.duplicate-address-frame`

ده يكشف:  
IP واحد مربوط بأكثر من MAC.

وهنا تأكدنا إن:

- Gateway IP ظهر مع MAC شرعي
    
- وظهر مع MAC مهاجم
    

---

# 🧠 مؤشرات الإصابة اللي لاحظناها

✔ Duplicate IP → Multiple MAC  
✔ Gratuitous ARP Flood  
✔ High ARP response frequency  
✔ Gateway IP advertised by suspicious MAC  
✔ No corresponding Who-has requests

---

# 🧨 ماذا حدث فعليًا؟

المهاجم:

1. انتحل Gateway
    
2. سمم ARP cache
    
3. أصبح بين الضحية والراوتر
    
4. جهز نفسه لتنفيذ DNS Spoofing (الخطوة التالية في الهجوم)
    

---

# 🔐 كيف تمنع ARP Spoofing؟

الحلول الواقعية:

- Dynamic ARP Inspection (DAI)
    
- DHCP Snooping
    
- Static ARP Entries (في البيئات الحساسة)
    
- Port Security
    
- IDS/IPS monitoring


# 🕵️ Unmasking DNS Spoofing

## 1️⃣ فهم البروتوكول

DNS = نظام تحويل الأسماء (human-friendly) → IP (computer-friendly)  
مثال: `www.google.com` → `172.217.3.110`

**DNS Spoofing**: المهاجم يرسل ردود DNS مزيفة للضحية ليحولها إلى IP خاص به → MITM.

---

## 2️⃣ كيف يعمل الهجوم عمليًا

1. المهاجم موجود على الشبكة (عن طريق ARP Spoofing سابقًا).
    
2. الضحية يرسل استعلام DNS: `corp-login.acme-corp.local`.
    
3. المهاجم يرد بسرعة بـ IP مزيف → الضحية يثق بالرد ويخزن في الـ DNS cache.
    
4. أي اتصال لاحق → يمر عبر المهاجم → يمكنه سرقة البيانات (Username/Password).
    

---

## 3️⃣ مؤشرات DNS Spoofing

|Indicator|شرح|
|---|---|
|Multiple DNS responses for same query|رد من DNS الحقيقي + رد مزيف|
|DNS response from unexpected IP|IP للرد مش معروف كـ DNS Server (مش 8.8.8.8)|
|Short TTL|TTL منخفض جدًا (1-30s) للحفاظ على poisoning)|
|Unsolicited DNS responses|رد DNS بدون طلب من الضحية|

---

## 4️⃣ خطوات التحقيق في Wireshark

### Step 1: فلتر كل DNS Traffic

`dns`

- يعرض كل Requests وResponses.
    

### Step 2: فلتر الردود المشروعة

`dns.flags.response == 1 && ip.src == 8.8.8.8`

- يظهر الردود الطبيعية من الـ DNS server.
    

### Step 3: فلتر الردود من أي IP آخر

`dns.flags.response == 1 && ip.src != 8.8.8.8 && dns.qry.name == "corp-login.acme-corp.local"`

- كل رد غير متوقع → مؤشر قوي على DNS Spoofing.
    

### Step 4: فلتر استعلامات Domain معين

`dns && dns.qry.name == "corp-login.acme-corp.local"`

- يظهر كل استعلامات DNS للـ domain ده.
    

---

## 5️⃣ ماذا اكتشفنا عمليًا؟

1. الهجوم بدأ بـ **ARP Spoofing** للـ Gateway IP: 192.168.10.1
    
2. ثم قام المهاجم بإرسال **DNS Responses مزيفة** لـ `corp-login.acme-corp.local` → rerouting الضحية إلى IP المهاجم
    
3. كل هذا يؤكد **MITM multi-stage attack**
    

---

## 6️⃣ خطوات تالية للتحقيق

- التحقق من استخدام **SSL Stripping** (للتجسس على HTTPS)
    
- مراقبة أي بيانات تم اعتراضها → usernames/passwords
    
- تحليل TTL و Source IP لكل رد DNS مزيف



# 🔐 Spotting SSL Stripping in Action

## 📌 ما هو SSL Stripping؟

SSL Stripping هو هجوم MITM بيجبر الضحية تستخدم **HTTP بدل HTTPS**، بحيث:

- الضحية ↔️ المهاجم = HTTP (غير مشفر)
    
- المهاجم ↔️ السيرفر الحقيقي = HTTPS (مشفر)
    

وبالتالي المهاجم يشوف كل البيانات في النص الصريح.

---

# 🧠 كيف يعمل الهجوم (عمليًا في السيناريو ده)

### 1️⃣ الضحية يطلب الموقع

`https://corp-login.acme-corp.local`

### 2️⃣ المهاجم يسيطر على الشبكة

عن طريق:

- ARP Spoofing
    
- DNS Spoofing → يحول الدومين إلى IP المهاجم: `192.168.10.55`
    

### 3️⃣ يبدأ SSL Stripping

- المفروض يحصل TLS Handshake
    
- لكن بدل ما يحصل HTTPS
    
- الضحية تبدأ اتصال HTTP عادي
    

### 4️⃣ النتيجة

- POST Request فيه Username / Password واضحين
    
- Credentials في plaintext
    

---

# 🔎 خطوات التحقيق في Wireshark

---

## 🟢 Step 1 — عزل كل SSL Traffic

`tls || ssl`

ده يعرض كل TLS Handshakes وEncrypted Sessions.

---

## 🟢 Step 2 — التأكد إن الموقع فعلاً يستخدم TLS

`tls.handshake.type == 1 && tls.handshake.extensions_server_name == "corp-login.acme-corp.local"`

📌 ملاحظة مهمة:

- `tls.handshake.type == 1` = Client Hello
    
- وجود Handshake طبيعي مع السيرفر يؤكد إن الموقع أصلاً HTTPS
    

---

## 🟢 Step 3 — إثبات DNS Spoofing قبل الـ Stripping

`dns.flags.response == 1 && ip.src == 192.168.10.55 && dns.qry.name == "corp-login.acme-corp.local"`

📌 النتيجة:

- المهاجم رد على استعلام DNS
    
- وجه الضحية إلى IP مزيف
    

---

## 🟢 Step 4 — ملاحظة اختفاء TLS بعد التسميم

`http && ip.src == 192.168.10.10 && ip.dst == 192.168.10.55`

📌 هنا بقى أهم دليل:

- مفيش TLS handshake للـ domain
    
- في HTTP traffic مباشر
    
- في POST credentials واضحة
    

ده هو **الدليل القاطع على SSL Stripping**

---

# 🚨 أهم Indicators of SSL Stripping

|Indicator|ماذا يعني|
|---|---|
|HTTPS → HTTP downgrade|فجأة الاتصال بقى Port 80|
|اختفاء TLS handshake|بعد DNS spoof|
|Cleartext credentials|بيانات login بدون تشفير|
|No certificate exchange|مفيش Server Certificate|

---

# 📊 Timeline للهجوم

## 1️⃣ ARP Spoofing

المهاجم:

- أرسل unsolicited ARP is-at
    
- انتحل Gateway
    

---

## 2️⃣ DNS Spoofing

الضحية:

- استعلم عن `corp-login.acme-corp.local`
    

المهاجم:

- رد بـ 192.168.10.55
    

---

## 3️⃣ SSL Stripping

- الضحية تتصل HTTP
    
- لا يوجد TLS
    
- Login credentials في cleartext
    

---

# 🎯 النتيجة النهائية

الهجوم Multi-Stage MITM:

`ARP Poisoning       ↓ DNS Poisoning       ↓ SSL Stripping       ↓ Credential Theft`

---

# 🛡️ كيف يتم منعه؟

- HSTS (HTTP Strict Transport Security)
    
- DNSSEC
    
- Static ARP entries
    
- Network segmentation
    
- IDS/IPS detection rules
    
- Certificate pinning