
# 🧠 أولاً: تذكير سريع بالفكرة

## ما هو DNS Tunneling؟

![https://www.paloaltonetworks.com/content/dam/pan/en_US/images/cyberpedia/what-is-dns-tunneling/DNSTunneling2025_DNS.png?imwidth=480](https://www.paloaltonetworks.com/content/dam/pan/en_US/images/cyberpedia/what-is-dns-tunneling/DNSTunneling2025_DNS.png?imwidth=480)

![https://www.akamai.com/site/en/images/article/2023/what-is-dns-data-exfiltration.png](https://www.akamai.com/site/en/images/article/2023/what-is-dns-data-exfiltration.png)

![https://www.ibm.com/content/adobe-cms/us/en/think/topics/dns/jcr%3Acontent/root/table_of_contents/body-article-8/image.coreimg.png/1763402839315/how-data-queries-move-through-the-dns.png](https://www.ibm.com/content/adobe-cms/us/en/think/topics/dns/jcr%3Acontent/root/table_of_contents/body-article-8/image.coreimg.png/1763402839315/how-data-queries-move-through-the-dns.png)

4

DNS Tunneling هو استغلال بروتوكول:

Domain Name System

لتمرير بيانات داخل:

- Subdomain labels
    
- TXT records
    
- DNS Queries نفسها
    

بدل ما البيانات تخرج عبر HTTP/HTTPS، يتم تقسيمها وترميزها وإرسالها عبر DNS.

---

# 🎯 هدفنا في التحليل

تحليل ملف:

`/data_exfil/dns_exfil/dns_exfil.pcap`

والبحث عن:

- Domain مشبوه
    
- Subdomains طويلة جدًا
    
- High entropy
    
- Beaconing pattern
    
- TXT abuse
    
- NXDOMAIN flood
    

---

# 🛠 الخطوة 1: فتح الملف في Wireshark

`wireshark dns_exfil.pcap`

فلتر أولي:

`dns`

---

# 🔎 الخطوة 2: هل يوجد Domain مريب؟

في Wireshark:

`Statistics → Conversations → DNS`

أو:

`Statistics → Endpoints → DNS`

نبحث عن:

- Domain واحد بعدد طلبات مرتفع جدًا
    
- يختلف عن باقي الدومينات الطبيعية
    

🚨 لو لقيت:

> آلاف الطلبات إلى نفس الدومين

ده مؤشر قوي.

---

# 🔬 الخطوة 3: فحص طول الاستعلامات

فلتر مهم جدًا:

`dns.qry.name.len > 60`

لو ظهر:

- Subdomains بطول 80–150 حرف
    
- أسماء تحتوي على:
    
    `a9sd8f7as9df87as9df8a7sd9f...`
    

ده غالبًا Base32 أو Base64 encoded data.

---

# 🔍 الخطوة 4: تحليل الـ Entropy

ابحث عن:

- حروف عشوائية
    
- خليط أرقام + حروف
    
- عدم وجود كلمات مفهومة
    

مثال:

`jksd8fj2k3j4k2j3k4j2k3.attacker.com`

ده مش طبيعي أبدًا.

---

# 📡 الخطوة 5: البحث عن Beaconing

فلتر زمني:

`View → Time Display Format → Seconds Since Beginning`

لو وجدت:

- Query كل 5 ثواني
    
- أو كل 10 ثواني
    
- لنفس الدومين
    

ده Beaconing.

---

# 📦 الخطوة 6: Record Types غير طبيعية

فلتر:

`dns.qry.type == 16`

Type 16 = TXT

لو وجدت:

- TXT responses كبيرة
    
- أو كثيرة جدًا
    

ده ممكن يكون:

> Exfil via TXT record

---

# ❗ الخطوة 7: NXDOMAIN Flood

فلتر:

`dns.flags.rcode == 3`

Rcode 3 = NXDOMAIN

لو الهجوم يستخدم:

- Exfil-by-query فقط
    
- بدون استجابة حقيقية
    

هتشوف نسبة NXDOMAIN عالية.

---

# 🔥 سيناريو شائع في مثل هذا التحدي

عادةً في ملفات تدريب DNS exfil:

- جهاز داخلي يرسل مئات الطلبات
    
- إلى دومين غريب مثل:
    
    `something.badguy.com`
    

كل Query يحتوي:

- جزء من البيانات المشفرة
    
- يتم تجميعها في السيرفر
    

---

# 🧩 كيف تستخرج البيانات؟

لو تأكدنا أن:

`<encoded_data>.evil.com`

نقدر نصدر كل أسماء الاستعلامات:

`tshark -r dns_exfil.pcap -Y "dns.qry.name" -T fields -e dns.qry.name > queries.txt`

ثم:

- إزالة اسم الدومين
    
- تجميع الأجزاء
    
- فك الترميز Base32 أو Base64
    

---

# 🚨 مؤشرات الإصابة (IOC Summary)

|المؤشر|لماذا مهم|
|---|---|
|High query volume|Exfil chunks|
|Long subdomains|Encoded data|
|High entropy|مش بيانات DNS طبيعية|
|Regular timing|Beacon|
|Many TXT records|Payload channel|
|NXDOMAIN spike|Query-only exfil|

---

# 🛡 كيف نكشفه في بيئة حقيقية؟

- DNS logging enabled
    
- Entropy detection rules
    
- Alert on long FQDN
    
- Baseline comparison
    
- Block external DNS resolvers
    
- Force internal resolver inspection
    

------

# 🔎 أولاً: التحليل عبر Wireshark

## 1️⃣ فلترة حركة DNS بالكامل

`dns`

الهدف:

- عزل بروتوكول Domain Name System
    
- استبعاد باقي الترافيك
    

---

## 2️⃣ البحث عن Queries بدون Response

`dns.flags.response == 0`

🔍 لماذا مهم؟

في DNS Tunneling أحيانًا:

- المهاجم لا يحتاج Response
    
- فقط يرسل Query تحتوي بيانات
    
- النتيجة: عدد كبير من Queries بدون رد
    

🚨 مؤشر قوي لو العدد كبير.

---

## 3️⃣ اكتشاف الاستعلامات الطويلة

`dns && frame.len > 70`

أو الأفضل:

`dns.qry.name.len > 60`

### ماذا نبحث عنه؟

- Subdomain بطول غير طبيعي
    
- سلاسل عشوائية مثل:
    

`ajd83kd9sk3j9dk3j9dk3j9dk3.attacker.com`

⚠️ DNS الطبيعي لا يحتاج أسماء بهذا الطول.

---

## 4️⃣ عزل الدومين المشبوه

`dns && dns.qry.name contains suspiciousdomain.com`

### ماذا لاحظنا؟

✔ دومين خارجي واحد فقط  
✔ آلاف الطلبات  
✔ من عدة أجهزة داخلية  
✔ Subdomains مختلفة كل مرة

ده نمط **Chunked Data Exfiltration**.

---

# 📊 ماذا استنتجنا من الـ PCAP؟

- عدة أجهزة داخلية compromised
    
- كل جهاز يرسل البيانات على شكل أجزاء
    
- دومين خارجي واحد يستقبل البيانات
    
- Queries طويلة
    
- أحيانًا بدون استجابة
    

🎯 النتيجة: DNS Tunneling مؤكد تقريبًا.

---

# 🧠 الآن ننتقل إلى Splunk للتحقق على مستوى اللوجز

## فتح Splunk

نبحث في:

`index=data_exfil sourcetype=DNS_logs`

---

## 1️⃣ معرفة أكثر الأجهزة إرسالًا للطلبات

`index="data_exfil" sourcetype="DNS_logs"  | stats count by src_ip`

🔍 ماذا نبحث عنه؟

- جهاز واحد بعدد طلبات ضخم
    
- أو عدة أجهزة بنفس النمط
    

🚨 جهاز يرسل آلاف DNS requests = suspicious beacon/exfil.

---

## 2️⃣ أكثر الاستعلامات تكرارًا

`index="data_exfil" sourcetype="dns_logs"  | stats count by query  | sort -count`

### ماذا يظهر؟

- أسماء طويلة جدًا
    
- سلاسل غير مفهومة
    
- استعلامات غريبة تتكرر كثيرًا
    

---

## 3️⃣ فلترة الاستعلامات الطويلة

`index="data_exfil" sourcetype="DNS_logs"  | where len(query) > 30`

🎯 هذا الفلتر ممتاز لاستخراج:

- Encoded subdomains
    
- Data chunks
    

---

# 🚨 المؤشرات التي أكدت الهجوم

|المؤشر|لماذا خطير|
|---|---|
|DNS requests كثيرة جدًا|قناة إخراج بيانات|
|Query names طويلة|Data encoding|
|Requests بدون response|Query-only exfil|
|عدة أجهزة مصابة|انتشار داخلي|
|دومين خارجي واحد|C2 / Data receiver|

---

# 🛡 كيف كان يمكن منعه؟

- تفعيل DNS logging داخلي
    
- منع استخدام Public DNS resolvers
    
- تفعيل DNS inspection
    
- Alert على FQDN length
    
- Alert على Entropy عالي
    
- Baseline طبيعي لحجم DNS
    

---

# 🧩 الخلاصة

اللي حصل هو:

1. جهاز/أجهزة تم اختراقها
    
2. البيانات تم ترميزها (Base32/64 غالبًا)
    
3. تم تقسيمها إلى أجزاء
    
4. تم إرسالها داخل DNS Queries
    
5. السيرفر الخارجي يجمع البيانات
    

وده يسمى:

> Covert Channel عبر Domain Name System

---

# 🔥 ممتاز جدًا — جاهز للمرحلة التالية

بما إنك هتنتقل لتحليل:

> Data Exfiltration via FTP

خليني أجهزك ذهنيًا 👇

في FTP exfil هنبحث عن:

- Large file transfers
    
- Unusual outbound FTP sessions
    
- Anonymous login
    
- Credentials in clear text
    
- High volume data upload



# 📌 أولاً: تذكير سريع بالبروتوكول

## ما هو FTP؟

![https://images.prismic.io/exavault-website/b50a7b0e-4b81-4a19-8817-00deb57e648f_ftp-server-control-and-data-channels-graphic.png?auto=compress%2Cformat](https://images.prismic.io/exavault-website/b50a7b0e-4b81-4a19-8817-00deb57e648f_ftp-server-control-and-data-channels-graphic.png?auto=compress%2Cformat)

![https://www.mybluelinux.com/img/post/posts/0052/ftp-active-passive-mode.jpg](https://www.mybluelinux.com/img/post/posts/0052/ftp-active-passive-mode.jpg)

![https://images.prismic.io/exavault-website/2b2fb71f-3f93-410e-a5fc-e94367d3d16c_port21-connect-infographic.png?auto=compress%2Cformat](https://images.prismic.io/exavault-website/2b2fb71f-3f93-410e-a5fc-e94367d3d16c_port21-connect-infographic.png?auto=compress%2Cformat)

4

File Transfer Protocol  
هو بروتوكول قديم لنقل الملفات يعمل فوق TCP.

### مهم جدًا:

- Control Channel → Port 21
    
- Data Channel → Port عشوائي (Ephemeral)
    
- Credentials تنتقل **بنص واضح (Cleartext)**
    

⚠️ لذلك هو خطير جدًا لو لم يتم استبداله بـ SFTP أو FTPS.

---

# 🎯 كيف يستخدم المهاجم FTP في Exfiltration؟

1. استخدام حساب مخترق (Service / Guest)
    
2. رفع ملفات عبر STOR
    
3. نقل كميات كبيرة من البيانات
    
4. استخدام PASV لفتح قنوات بيانات خفية
    

---

# 🔎 التحليل في Wireshark

## 1️⃣ عزل جلسات FTP

`ftp || ftp-data`

ده يعزل:

- Control traffic
    
- Data channel
    

---

## 2️⃣ البحث عن Credentials

`ftp.request.command == "USER" || ftp.request.command == "PASS"`

### ماذا نبحث عنه؟

- Username مشبوه
    
- Guest account
    
- Password ضعيف
    
- Login من جهاز داخلي إلى IP خارجي
    

🚨 لو ظهر:

`USER guest PASS 12345`

ده مؤشر ضعف أمني.

---

# 📂 3️⃣ البحث عن عمليات رفع ملفات

`ftp contains "STOR"`

STOR = Upload file to server

⚠️ في Exfiltration نحن نهتم بالـ STOR أكثر من RETR.

---

## 🧠 Follow TCP Stream

Right-click → Follow → TCP Stream

نبحث عن:

- أسماء ملفات حساسة
    
- CSV
    
- PDF
    
- TXT
    
- XLS
    
- Backup files
    

---

## 4️⃣ فلترة ملفات CSV

`ftp contains "csv"`

### ماذا لاحظنا؟

- IP داخلي
    
- مستخدم Guest
    
- نقل ملفات CSV
    
- إلى IP خارجي غير موثوق
    

🎯 هذا Exfiltration واضح.

---

# 📏 5️⃣ البحث عن Payload كبير

`ftp && frame.len > 90`

أو أفضل:

`tcp.len > 1000 && ftp-data`

### لماذا مهم؟

- نقل ملفات حساسة غالبًا ينتج عنه Payload كبير
    
- خاصة لو كان CSV يحتوي بيانات عملاء
    

---

# 🧾 ماذا وجدنا في الـ Stream؟

يبدو أن:

- ملف يحتوي معلومات حساسة
    
- تم رفعه إلى IP خارجي
    
- خارج ساعات العمل (محتمل)
    

🚨 ده سلوك Exfiltration صريح.

---

# 🔍 مؤشرات الإصابة (IOCs)

|المؤشر|لماذا مهم|
|---|---|
|USER/PASS cleartext|سرقة حساب|
|STOR command|رفع بيانات|
|Guest account|Misconfiguration|
|Large payload|نقل ملفات|
|External IP غير معتاد|Data staging|
|PASV mode|فتح Data channel منفصل|

---

# 🛡 لماذا FTP سهل الاستغلال؟

لأنه:

- غير مشفر
    
- يسمح بحسابات Guest
    
- يفتح Data channel منفصل
    
- غالبًا لا يتم مراقبته جيدًا
    

---

# 🔥 الفرق بين DNS و FTP Exfil

|DNS|FTP|
|---|---|
|تهريب خفي|نقل مباشر|
|بيانات صغيرة مقسمة|ملفات كاملة|
|يصعب اكتشافه|أسهل لكن أخطر حجماً|
|يستخدم port 53|يستخدم port 21|

---

# 🧠 ماذا نفعل في SOC بعد الاكتشاف؟

1. عزل الجهاز المصاب
    
2. تعطيل الحساب المستخدم
    
3. تغيير كلمات المرور
    
4. حظر الـ IP الخارجي
    
5. مراجعة Logs أخرى
    
6. فحص وجود Backdoor
    

---

# 📌 ملاحظة مهمة

FTP ليس مشفرًا.  
لو كان الهجوم عبر:

Secure File Transfer Protocol

سيكون التحليل أصعب لأن:

- البيانات مشفرة
    
- نعتمد على Metadata فقط
    

---

# 🚨 الآن سؤالك الأخير مهم جدًا:

> Explore other streams to find more indicators of sensitive documents being exfiltrated through the HTTP protocol.

ده معناه إن فيه **مرحلة تانية من الهجوم عبر HTTP**.

في HTTP Exfil نبحث عن:

- POST requests كبيرة
    
- Upload إلى scripts مثل:
    
    - upload.php
        
    - api.php
        
- Base64 encoded body
    
- User-Agent مشبوه
    
- Large outbound traffic
    

فلتر مبدئي:

`http.request.method == "POST"`

ثم:

`tcp.len > 1000 && http`



---

# 🌐 ما هو HTTP Exfiltration؟

![https://cdn.prod.website-files.com/601959b8cde20c101809c86a/637272b714a6b0397b02c856_Data-Exfiltration-Figure-1-800.webp](https://cdn.prod.website-files.com/601959b8cde20c101809c86a/637272b714a6b0397b02c856_Data-Exfiltration-Figure-1-800.webp)

![https://storage.googleapis.com/gweb-cloudblog-publish/images/fig2-defining-cobalt-strike_0_pikz.max-1400x1400.png](https://storage.googleapis.com/gweb-cloudblog-publish/images/fig2-defining-cobalt-strike_0_pikz.max-1400x1400.png)

![https://osqa-ask.wireshark.org/upfiles/tcpdump_out_of_order.png](https://osqa-ask.wireshark.org/upfiles/tcpdump_out_of_order.png)

4

Hypertext Transfer Protocol  
بروتوكول شائع جدًا لنقل البيانات عبر الويب.

🎯 المهاجم يستغله لأنه:

- مسموح عبر الجدار الناري
    
- يبدو طبيعيًا
    
- يمكن إخفاء البيانات داخله بسهولة
    
- يمكن تشفيره عبر HTTPS
    

---

# 🧠 كيف يستخدم المهاجم HTTP في التهريب؟

### 1️⃣ POST Upload

رفع بيانات داخل Body الطلب.

### 2️⃣ GET with encoded data

وضع البيانات داخل:

- Query string
    
- URL path
    

### 3️⃣ Custom Headers

مثل:

`X-Data: <Base64>`

### 4️⃣ Chunked Transfer

تقسيم ملف كبير إلى أجزاء صغيرة.

### 5️⃣ استخدام خدمات مشهورة

مثل:

- Dropbox
    
- GitHub
    

لإخفاء النشاط.

---

# 📊 التحليل في Splunk

## 1️⃣ عرض كل HTTP logs

`index="data_exfil" sourcetype="http_logs"`

---

## 2️⃣ عزل POST requests

`index="data_exfil" sourcetype="http_logs" method=POST`

🎯 لأن POST هو الأكثر استخدامًا في الرفع.

---

## 3️⃣ تحليل حجم البيانات المرسلة

`index="data_exfil" sourcetype="http_logs" method=POST  | stats count avg(bytes_sent) max(bytes_sent) min(bytes_sent) by domain  | sort - count`

### ماذا نبحث عنه؟

- دومين غير معروف
    
- max(bytes_sent) عالي جدًا
    
- avg(bytes_sent) أعلى من الطبيعي
    

---

## 4️⃣ عزل الـ Payload الكبير

`index="data_exfil" sourcetype="http_logs" method=POST bytes_sent > 600  | table _time src_ip uri domain dst_ip bytes_sent  | sort - bytes_sent`

🎯 هنا ظهر Entry واحد مشبوه:

- Upload كبير
    
- إلى IP خارجي
    
- دومين غير معتاد
    

---

# 🔬 التحليل في Wireshark

افتح:

`/data_exfil/http_exfil/http_lab.pcap`

---

## 1️⃣ فلترة HTTP

`http`

---

## 2️⃣ عزل POST

`http.request.method == "POST"`

---

## 3️⃣ فلترة حسب الحجم

`http.request.method == "POST" and frame.len > 500`

ثم:

`http.request.method == "POST" and frame.len > 750`

🎯 النتيجة:  
Entry واحدة فقط — نفس اللي ظهرت في Splunk.

---

# 🔎 Follow HTTP Stream

Right-click → Follow → HTTP Stream

ماذا وجدنا؟

- Document واضح
    
- يحتوي بيانات حساسة
    
- تم رفعه إلى IP خارجي
    

🚨 تهريب مباشر للبيانات.

---

# 📌 مؤشرات الهجوم التي أكدناها

|المؤشر|لماذا خطير|
|---|---|
|POST بحجم كبير|Upload|
|Domain غير مألوف|C2 / staging|
|Payload كبير|ملف كامل|
|تطابق بين PCAP و Logs|تأكيد التحليل|
|لا يشبه سلوك مستخدم عادي|Anomaly|

---

# 🧩 الفرق بين طرق التهريب الثلاثة

|DNS|FTP|HTTP|
|---|---|---|
|خفي جدًا|مباشر وواضح|يختلط بالطبيعي|
|بيانات صغيرة|ملفات كاملة|ملفات أو chunks|
|يحتاج تحليل entropy|واضح عبر STOR|يحتاج تحليل حجم|
|port 53|port 21|port 80/443|

---

# 🚨 لماذا HTTP أخطر؟

لأنه:

- طبيعي جدًا في أي بيئة
    
- يمكن تشفيره عبر HTTPS
    
- يستخدم Cloud/CDN
    
- يمكن عمل Low-and-Slow
    

---

# 🛡 كيف نكشفه في بيئة حقيقية؟

### 1️⃣ Baseline

معرفة متوسط:

- bytes_sent
    
- POST frequency
    

### 2️⃣ Alert Rules

- POST > threshold
    
- Uncommon domains
    
- Beaconing pattern
    
- High outbound ratio
    

### 3️⃣ TLS Inspection

لو HTTPS

### 4️⃣ DLP Integration





-
__
------
-------------------------------------------------------------------------------------------------------------


دي آخر مرحلة في سلسلة الـ Exfiltration — وواحدة من أذكى الطرق: **Data Exfiltration via ICMP**.

خلينا نمشيها بأسلوب تحقيق عملي زي المرات اللي فاتت.

---

# 🌐 ما هو ICMP ولماذا يُساء استخدامه؟

![https://www.researchgate.net/publication/354334248/figure/fig5/AS%3A1063937256734722%401630673817959/CMP-Tunneling-Attackers-Manipulating-ICMP-Payload-to-the-Host-A-and-Receiving-Desired.ppm](https://www.researchgate.net/publication/354334248/figure/fig5/AS%3A1063937256734722%401630673817959/CMP-Tunneling-Attackers-Manipulating-ICMP-Payload-to-the-Host-A-and-Receiving-Desired.ppm)

![https://i.sstatic.net/nsUb0.jpg](https://images.openai.com/static-rsc-1/6fZGm0hg_zrXn94XtMiq2pX2VPyfyCis15wkHUk4YjGu0_doxhZbrLEGET37g_l26UWU5avfI-9neBUfxSleZ5BRE9r2_C6m1l8URmywrfq670lnSlqtCIGKJ3c4pHIJaNImh8YvSl49BKpNmL4QRw)

![https://learningnetwork.cisco.com/servlet/rtaImage?eid=ka06e000000dMym&feoid=00N3i00000D6DDX&refid=0EM6e000004EAJr](https://learningnetwork.cisco.com/servlet/rtaImage?eid=ka06e000000dMym&feoid=00N3i00000D6DDX&refid=0EM6e000004EAJr)

4

Internet Control Message Protocol  
هو بروتوكول Layer 3 يُستخدم لـ:

- Ping
    
- TTL Exceeded
    
- Network diagnostics
    

🎯 المشكلة؟

- غالبًا مسموح في الجدار الناري
    
- لا يتم فحصه بعمق
    
- يمكن وضع بيانات داخل الـ Payload
    

---

# 🧠 كيف يتم تهريب البيانات عبر ICMP؟

## الطريقة الشائعة:

1. تقسيم الملف إلى أجزاء
    
2. ترميزها (Base64 / Hex)
    
3. وضع كل جزء داخل ICMP Echo Request
    
4. إرسالها إلى سيرفر خارجي
    
5. السيرفر يعيد تجميعها
    

---

# 🔎 التحليل العملي في Wireshark

افتح:

`/data_exfil/icmp/exfil/icmp_lab.pcap`

---

## 1️⃣ عزل كل ICMP

`icmp`

نبحث عن:

- كثافة غير طبيعية
    
- Host واحد يرسل ICMP باستمرار
    

---

## 2️⃣ عزل Echo Requests

`icmp.type == 8`

Type 8 = Echo Request  
Type 0 = Echo Reply

🚨 لو جهاز داخلي يرسل مئات Echo Requests إلى IP خارجي → suspicious.

---

# 📏 3️⃣ اكتشاف الحزم الكبيرة

`icmp.type == 8 and frame.len > 100`

📌 طبيعيًا:

- Ping عادي ≈ 74 bytes
    

أي حاجة فوق 100 bytes تستحق التحقيق.

---

# 🚨 ماذا نبحث داخل الحزمة؟

افتح إحدى الحزم الكبيرة:

- Expand ICMP
    
- Inspect Data section
    

ابحث عن:

- Base64 patterns:
    
    `QWxhZGRpbjpPcGVuU2VzYW1l`
    
- Hex strings طويلة
    
- Entropy عالي (بيانات عشوائية)
    

---

# ⏱ 4️⃣ البحث عن Periodicity

غيّر Time format إلى:

`Seconds Since Beginning`

لو وجدت:

- Ping كل 2 ثانية
    
- نفس الحجم تقريبًا
    
- لنفس IP
    

🎯 ده Beaconing + Data chunking.

---

# 🧩 5️⃣ البحث عن Fragmentation

فلتر:

`ip.flags.mf == 1 || ip.frag_offset > 0`

لو الهجوم يرسل Payload كبير:

- سيتم تقسيمه إلى Fragments
    
- ثم إعادة تجميعه في الطرف الآخر
    

---

# 🚨 مؤشرات الإصابة (IOCs)

|المؤشر|لماذا خطير|
|---|---|
|ICMP بكثافة عالية|قناة اتصال|
|Payload أكبر من الطبيعي|تهريب بيانات|
|Base64 داخل Data|Encoded exfil|
|Regular timing|Beacon|
|IP خارجي غير معروف|C2|

---

# 🧠 مقارنة سريعة بين كل طرق التهريب

|البروتوكول|مستوى الخفاء|حجم البيانات|صعوبة الكشف|
|---|---|---|---|
|DNS|عالي جدًا|صغير|صعب|
|FTP|منخفض|كبير|سهل|
|HTTP|متوسط/عالي|متوسط/كبير|متوسط|
|ICMP|متوسط|صغير/متوسط|سهل إذا راقبت الحجم|

---

# 🛡 كيف تمنع ICMP Exfil في بيئة حقيقية؟

- حظر ICMP outbound لو غير ضروري
    
- تحديد max ICMP payload size
    
- Alert على ICMP > 100 bytes
    
- Monitor ICMP rate per host
    
- Deep Packet Inspection
    

---

# 🎯 لماذا ICMP سهل الاكتشاف نسبيًا؟

لأن:

- طبيعيًا لا يحمل بيانات كثيرة
    
- لا يستخدم لنقل ملفات
    
- أي حجم غير طبيعي واضح جدًا
    

بعكس:

- HTTP (طبيعي جدًا)
    
- DNS (يبدو طبيعيًا جدًا)