
# 🌐 Task 1 — Introduction to Network Traffic Analysis (NTA)

## ✅ What is Network Traffic Analysis؟

### يعني إيه NTA؟

**Network Traffic Analysis (NTA)**  
هو عملية:

> **Capturing + Inspecting + Analyzing** البيانات وهي ماشية في الشبكة

🎯 الهدف:

- Full visibility على الشبكة
    
- نفهم **مين بيتكلم مع مين + بيقول إيه + ليه**
    

---

## ⚠️ Important Note

### نقطة مهمة جدًا

❌ NTA ≠ Wireshark فقط

ناس كتير تفتكر:

> "NTA يعني أفتح Wireshark وخلاص"

لكن الصح:  
NTA = Combination of:

- Logs correlation
    
- Deep Packet Inspection (DPI)
    
- Network Flow statistics (NetFlow)
    
- Monitoring tools
    

👉 يعني **منهجية كاملة مش مجرد Tool**

---

# 🎯 Why is NTA important؟

## ليه المهارة دي أساسية؟

### لأي Blue Team / SOC:

- Detect attacks
    
- Investigate incidents
    
- Validate alerts
    
- Understand abnormal behavior
    

### لأي Red Team:

- Understand detection methods
    
- Bypass monitoring
    

---

# 🧠 SOC L1 Perspective

كمحلل SOC Level 1 لازم تقدر:

### تعمل:

- Navigate huge traffic logs
    
- Identify baseline behavior
    
- Spot anomalies
    

### ببساطة:

👉 تعرف الفرق بين:  
Normal traffic ✅  
Suspicious traffic ❌


---

# 🌐 Why should we analyze Network Traffic?

## ليه نعمل تحليل لحركة الشبكة (Network Traffic Analysis)؟

### 💡 الفكرة ببساطة

**Logs لوحدها مش كفاية.**  
Firewall/DNS logs بتقولك _مين بعت لمين وإمتى_  
لكن **مش بتوريك المحتوى (content)**.

👉 وده معناه إن الهجوم ممكن يعدّي من غير ما نلاحظ.

---

# 🧠 Scenario: DNS Tunneling & Beaconing

### اللي حصل:

Host اسمه:

`WIN-016 (192.168.1.16)`

بيبعث DNS queries كتير لنفس الدومين:

`aj39skdm.malicious-tld.com msd91azx.malicious-tld.com cmd01.malicious-tld.com`

### 🚨 ده معناه إيه؟

- Same TLD + random subdomains
    
- Repeated queries
    
- TXT records
    

👉 غالبًا **DNS Tunneling or Beaconing (C2 Communication)**

---

# 📋 What DNS logs tell us (بس معلومات سطحية)

|Info|بنستفيد بإيه|
|---|---|
|Query & Type|نوع الطلب (A / TXT / etc)|
|Domain/Subdomain|نعمل reputation check بـ VirusTotal|
|Source IP|الجهاز المصاب|
|Destination IP|هل malicious|
|Timestamp|نعمل timeline|

❌ المشكلة:  
**Logs don't show the payload/content**

---

# 🔍 Why Traffic Analysis is Critical

## ليه لازم نفحص الباكيت نفسها؟

لأن المهاجم ممكن:

- يستخدم **TXT records** لإرسال Commands
    
- يعمل **Data Exfiltration via DNS**
    
- يعمل **C2 Communication مخفي**
    

### مثال:

`TXT: "SSBsb3ZlIHlvdXIgY3VyaW91c2l0eQ=="`

ده Base64 → فيه أوامر أو بيانات مخفية

👉 مستحيل تكتشف ده من اللوج بس  
👉 لازم Packet Capture (PCAP / Wireshark)

---

# 🎯 Main Goals of Network Traffic Analysis

## 🔹 بشكل عام:

- Monitor network performance
    
- Detect abnormalities
    
- Inspect suspicious communications
    
- Find hidden threats
    

## 🔹 في SOC:

- Detect malicious activity
    
- Reconstruct attacks (Incident Response)
    
- Validate alerts
    
- Investigate exfiltration & lateral movement
    

---

# 🧪 Real-world Use Cases

### Case 1:

System behavior changed →  
Traffic analysis →  
Found suspicious HTTP request →  
Extracted malicious ZIP

### Case 2:

Too many DNS requests →  
Traffic inspection →  
Found DNS tunneling →  
Data exfiltration detected

---

# 🔥 Final Takeaway (الخلاصة المهمة جدًا)

### Logs = Metadata only

### Traffic Analysis = Full Visibility

👉 لو اعتمدت على logs بس:  
❌ ممكن يفوتك الهجوم

👉 لو استخدمت PCAP + Traffic Analysis:  
✅ تشوف المحتوى الحقيقي  
✅ تفهم الهجوم  
✅ تثبت الـ IOC  
✅ ترد بسرعة

# 🌐 What Network Traffic Can We Observe?

## نقدر نشوف إيه جوه الترافيك؟

### 💡 الفكرة الأساسية

أفضل طريقة نفهم بيها الترافيك =  
👉 **TCP/IP Stack**

كل Layer:

- تضيف Header
    
- تحمل معلومات مهمة
    
- نقدر نحللها أمنيًا
    

📌 **Logs → جزء بسيط بس من الـ headers**  
📌 **Packet Capture (PCAP) → الصورة الكاملة**

---

# 🧱 TCP/IP Layers Overview

## 🔹 1) Application Layer

![https://imada.sdu.dk/u/jamik/dm557-19/images/wireshark/http/wireshark-http-1.png](https://imada.sdu.dk/u/jamik/dm557-19/images/wireshark/http/wireshark-http-1.png)

![https://scotthelme.co.uk/content/images/2015/02/nginx-server-header-source.png](https://scotthelme.co.uk/content/images/2015/02/nginx-server-header-source.png)

![https://www.wireshark.org/docs/wsug_html_chunked/images/ws-bytes-pane-popup-menu.png](https://www.wireshark.org/docs/wsug_html_chunked/images/ws-bytes-pane-popup-menu.png)

4

### بنشوف إيه؟

- Application headers
    
- Payload (البيانات الفعلية)
    

### مثال HTTP:

`GET /downloads/suspicious_package.zip`

نقدر نعرف:  
✅ file name  
✅ response code (200 OK)  
❌ مش شايفين محتوى الـ ZIP في اللوج

### ليه ده خطر؟

المهاجم ممكن:

- ينزل Malware
    
- يعمل Data exfiltration
    
- يبعت C2 commands
    

👉 **Only PCAP shows the real payload**

---

## 🔹 2) Transport Layer (TCP/UDP)

![https://static.afteracademy.com/images/what-is-a-tcp-3-way-handshake-process-three-way-handshaking-terminating-connection-6ea4a4c72d165361.jpg](https://static.afteracademy.com/images/what-is-a-tcp-3-way-handshake-process-three-way-handshaking-terminating-connection-6ea4a4c72d165361.jpg)

![https://www.wireshark.org/docs/wsug_html_chunked/images/ws-tcp-analysis.png](https://www.wireshark.org/docs/wsug_html_chunked/images/ws-tcp-analysis.png)

![https://cdn.shopify.com/s/files/1/0329/9865/3996/t/5/assets/what_is_firewall_logs-r1ffGT.True?v=1707792189](https://cdn.shopify.com/s/files/1/0329/9865/3996/t/5/assets/what_is_firewall_logs-r1ffGT.True?v=1707792189)

4

### بنشوف إيه؟

- Source/Destination ports
    
- Flags (SYN/ACK/PSH)
    
- Sequence numbers
    
- Window size
    

### مفيد في:

- Session tracking
    
- Detect hijacking
    
- Detect injections
    

### مثال:

`Seq jump huge → suspicious packet`

👉 ممكن يكون:  
**Session Hijacking / Packet Injection**

---

## 🔹 3) Internet Layer (IP)

![https://www.tcpipguide.com/free/diagrams/ipfragmentation.png](https://images.openai.com/static-rsc-1/A6l0dvgD45DGJAqtncC3POFgvao-EUWwRbXRYiDb9KxMZAEfmgBg623Tl6CIOAyuh6ymL-r4mtLYcPlJ09xUYSO3nzfOOibj9HodrOxX5EbK5PDM3nB8wa1thMp-7RvUW_sdhDBVqJLXgLKYJPJptQ)

![https://www.trueneutral.eu/pics/2015/wireshark-frags-1-4.png](https://www.trueneutral.eu/pics/2015/wireshark-frags-1-4.png)

![https://ars.els-cdn.com/content/image/1-s2.0-S0167404823000068-gr4.jpg](https://ars.els-cdn.com/content/image/1-s2.0-S0167404823000068-gr4.jpg)

4

### بنشوف إيه؟

- Source IP
    
- Destination IP
    
- TTL
    
- Fragment offset
    
- Total length
    

### مفيد في:

- Track attacker IP
    
- Detect fragmentation attacks
    
- Detect IDS evasion
    

### مثال هجوم:

- Tiny fragments
    
- Overlapping fragments
    

👉 لتفادي IDS

---

## 🔹 4) Link Layer (MAC / ARP)

![https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/ARP_Spoofing.svg/330px-ARP_Spoofing.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/3/33/ARP_Spoofing.svg/330px-ARP_Spoofing.svg.png)

![https://i.sstatic.net/B4Cfi.png](https://images.openai.com/static-rsc-1/nsO5Kfaq_kHr6iZDUcoN4PrgyRWBLXgDFQ9d2K03B5_0inSL5YuJbJv3VVklFcDQVJljDi2v19YewRMbNgPH_Zet4WLCchgJNZ_EqCBOjFo0OJe6Sia2w61bdpP-OIHRacnLOgLHxQNFq4UMF9S1Kg)

![https://www.ciscopress.com/content/images/chap7_9780136633662/elementLinks/07fig11_alt.jpg](https://www.ciscopress.com/content/images/chap7_9780136633662/elementLinks/07fig11_alt.jpg)

4

### بنشوف إيه؟

- MAC addresses
    
- ARP traffic
    
- Ethernet frames
    

### مفيد في:

- ARP poisoning
    
- MAC spoofing
    
- MITM detection
    

### مثال:

نفس MAC بيرد على كل الأجهزة  
👉 **Attacker doing ARP spoof**

---

# 🔥 Logs vs Traffic Analysis

|Feature|Logs|PCAP|
|---|---|---|
|Headers|Partial|Full|
|Payload|❌|✅|
|Attack visibility|Limited|Complete|
|Forensics|Weak|Strong|
# 🧠 حفظ سريع:

- Application → payload/content
    
- Transport → ports & sessions
    
- Internet → IP & fragmentation
    
- Link → MAC & ARP
    

👉 **Full packet = Full truth**

# 🌐 Network Traffic Sources and Flows

# مصادر الترافيك ومساراته داخل الشبكة

في التاسك اللي فات:  
اتكلمنا **نظريًا** عن Layers (TCP/IP)

لكن هنا:  
👉 بنتكلم **عمليًا (Real world monitoring)**

بدل ما أفكر:  
"Layer 3 / Layer 4"

أفكر:  
"الترافيك جاي من جهاز إيه؟ ورايح فين؟"

---

# 🧠 الفكرة الأساسية

في أي شركة / Corporate Network:

عندك:

## 1) Sources → مين بيولد الترافيك؟

## 2) Flows → الترافيك بيمشي فين؟

---

---

# 🔵 Sources (مصادر الترافيك)

النص قال:

> We can group the sources into two categories

يعني:  
كل الترافيك بيطلع من نوعين بس:

`Intermediary Endpoint`

---

# 🟡 1) Intermediary Sources

## أجهزة وسيطة (Infrastructure Devices)

### يعني إيه؟

أجهزة الترافيك **بيعدّي من خلالها** مش بيبدأ منها غالبًا

زي:

- Firewall
    
- Switch
    
- Router
    
- IDS / IPS
    
- Proxy
    
- Access Point
    
- WLC
    
- ISP infrastructure
    

---

## ⚙️ بتولد ترافيك إيه؟

مش user traffic  
لكن:  
👉 Control / Management traffic

زي:

### Routing protocols

- OSPF
    
- BGP
    
- EIGRP
    

### Management

- SNMP
    
- PING
    
- SSH
    
- Syslog
    

### Supporting

- ARP
    
- DHCP
    
- STP
    

---

## ليه مهمة أمنيًا؟

منها نقدر:

- نشوف logs مركزية
    
- نعرف مين كلم مين
    
- نكشف scanning
    
- نكشف DoS
    
- نكشف routing manipulation
    

👉 غالبًا دي **أفضل نقطة مراقبة في الشبكة**

---

---

# 🟢 2) Endpoint Sources

## الأجهزة النهائية (User/Server devices)

### دي اللي بيبدأ وينتهي عندها الترافيك

يعني:  
👉 أكبر مصدر للبيانات

---

## أمثلة:

- PCs / Hosts
    
- Servers
    
- VMs
    
- Cloud instances
    
- IoT
    
- Printers
    
- Mobiles
    
- Tablets
    

---

## ليه مهمة أمنيًا؟

لأن:  
🚨 معظم الهجمات تبدأ من هنا

زي:

- Malware infection
    
- Data exfiltration
    
- C2 beaconing
    
- Lateral movement
    

👉 90% من الترافيك المشبوه بيطلع من endpoints

---

---

# 🔥 خلاصة المصادر

|Type|Role|Traffic Volume|Risk|
|---|---|---|---|
|Intermediary|Pass traffic|Low|Monitoring point|
|Endpoint|Generate traffic|High|Main attack source|

---

---

# 🔴 Flows (مسارات الترافيك)

## يعني إيه Flow؟

> حركة الترافيك من نقطة لنقطة

زي:  
Client → Server

---

في الشركات:  
بنقسمها لنوعين كبار:

`North-South East-West`

---

---

# 🌍 1) North-South Traffic (NS)

## الترافيك اللي داخل/خارج الشركة

### التعريف:

> Traffic that exits or enters the LAN

يعني:

`LAN ↔ Internet (WAN)`

---

## بيمر منين؟

👉 دايمًا يعدّي على Firewall

---

## أمثلة بروتوكولات:

- HTTPS
    
- DNS
    
- SSH
    
- VPN
    
- SMTP
    
- RDP
    

---

## نوعين:

### Ingress → داخل

### Egress → خارج

---

## ليه مهم أمنيًا؟

ده المكان اللي:

- malware downloads
    
- data exfiltration
    
- C2 communication
    
- phishing
    

👉 معظم الهجمات تبدأ أو تنتهي هنا

---

## SOC Tip 🔥

لو عايز visibility قوية:  
✔️ ركز logging على الـ Firewall

---

---

# 🏢 2) East-West Traffic (EW)

## الترافيك الداخلي

### التعريف:

> Traffic that stays inside LAN

يعني:

`Host ↔ Host Server ↔ Server LAN ↔ Cloud`

---

## المشكلة:

❌ غالبًا مش بيتراقب كويس

لكن:  
🚨 أخطر بكتير

---

## ليه؟

لأن بعد الاختراق:  
المهاجم يعمل:  
👉 Lateral Movement

يعني:

- ينتقل بين الأجهزة
    
- يسرق credentials
    
- يصعد privileges
    

---

## خدمات داخلية مهمة:

- Active Directory
    
- SMB
    
- Kerberos
    
- File shares
    
- Backup servers
    
- Monitoring systems
    

---

## الهجمات هنا:

- Pass-the-hash
    
- Kerberoasting
    
- SMB exploitation
    
- Internal scanning
    

---

## SOC Tip 🔥

لو مش مراقب East-West:  
👉 هتكتشف الهجوم متأخر جدًا

---

---

# ⚙️ Flow Examples (أمثلة عملية)

خلينا نفهم 3 flows مشهورين جدًا:

---

# 1️⃣ HTTPS with Proxy Inspection

## اللي بيحصل:

بدل:  
Client → Website

يبقى:

`Client → Proxy → Website`

---

## البروكسي:

- يعمل TLS decryption
    
- يفحص المحتوى
    
- يمنع malware
    
- يرجع النتيجة للعميل
    

---

## أمنيًا:

نقدر:

- نشوف payload
    
- نحلل downloads
    
- نكشف malware
    

👉 ده اسمه TLS Inspection / SSL Inspection

---

---

# 2️⃣ External DNS Flow

## اللي بيحصل:

`Host → Internal DNS → Firewall → Internet DNS`

---

## ليه؟

عشان:

- cache
    
- filtering
    
- logging
    

---

## أمنيًا:

نقدر نكشف:

- DNS tunneling
    
- suspicious domains
    
- beaconing
    
- malware callbacks
    

---

---

# 3️⃣ SMB + Kerberos Flow

## لما تفتح:

`\\FILESERVER\MARKETING`

## الخطوات:

1. Authenticate مع Domain Controller
    
2. تاخد Ticket (Kerberos)
    
3. تستخدمه لفتح SMB session
    

---

## أمنيًا:

لو حد سرق ticket:

- Pass-the-ticket
    
- Lateral movement
    

👉 مهم جدًا نراقب SMB + Kerberos


---

# 🌐 How Can We Observe Network Traffic?

# نراقب الترافيك إزاي عمليًا؟

بعد ما عرفنا:

- What to observe (Layers)
    
- Where traffic flows (Sources & Flows)
    

دلوقتي:  
👉 **How to collect the data؟**

---

# 🧠 الفكرة الأساسية

النص بيقول:

> Network traffic analysis focuses on combining multiple sources

يعني:  
❌ مش مصدر واحد  
✅ مجموعة مصادر مع بعض

لازم تجمع:

`Logs + Packet Capture + Statistics`

الثلاثة دول مع بعض = Full Visibility

---

---

# 🔵 1) Logs (أول خط دفاع)

## التعريف:

> Logs are our first entry

يعني:  
اللوجز هي أول حاجة تبص عليها

### ليه؟

- سهلة
    
- خفيفة
    
- موجودة في كل الأجهزة
    
- مش محتاجة تخزين ضخم
    

---

## كل جهاز بيعمل Logging:

- Firewall
    
- Server
    
- Router
    
- Switch
    
- Web server
    
- OS
    

---

## ⚠️ مفيش Standard

كل Vendor بيصمم بطريقته:

### أمثلة:

- Windows → Event Logs
    
- Linux → Syslog
    
- Apache → CLF
    
- Firewall → custom format
    

---

## اللوج بيحتوي على إيه؟

غالبًا:

- Source IP
    
- Destination IP
    
- Port
    
- Time
    
- Action
    

لكن:  
❌ مش Payload  
❌ مش Full packet

---

## مثال Linux auth:

`Accepted password for gensane from 192.168.1.50`

### نفهم:

- مين دخل
    
- منين
    
- إمتى
    

---

## مثال Apache:

`GET /index.html 200`

### نفهم:

- طلب صفحة
    
- نجح
    

لكن:  
❌ مش شايف محتوى الصفحة

---

## متى تكفي Logs؟

- Failed login
    
- Port scanning
    
- Simple alerts
    

---

## متى مش كفاية؟

- Malware download
    
- Data exfiltration
    
- DNS tunneling
    
- Payload inspection
    

👉 وقتها نروح للـ PCAP

---

---

# 🔴 2) Full Packet Capture (PCAP)

## الملك الحقيقي للتحليل 👑

## التعريف:

> Capture the entire packet

يعني:  
تشوف:

- كل headers
    
- كل layers
    
- payload
    
- كل byte
    

---

## نستخدمه في:

- Malware extraction
    
- Forensics
    
- Deep investigation
    
- Reconstruct attacks
    

---

## إزاي نلتقط الباكيت؟

في طريقتين:

`Network TAP Port Mirroring (SPAN)`

---

---

# 🟡 A) Network TAP

## هو إيه؟

جهاز Hardware يتحط inline في الشبكة

### بيعمل:

- ينسخ كل الترافيك
    
- يبعته لجهاز monitoring
    

---

## مميزاته:

✅ Zero packet loss  
✅ Zero delay  
✅ Very accurate  
✅ مش بيأثر على الأداء

---

## عيوبه:

❌ غالي  
❌ محتاج تركيب فيزيائي

---

## يستخدم في:

- SOC
    
- Data centers
    
- High performance networks
    

---

---

# 🟠 B) Port Mirroring (SPAN)

## هو إيه؟

طريقة Software

Switch ينسخ الترافيك من بورت لبورت

---

## مثال Cisco:

`monitor session 1 source interface fa0/1 monitor session 1 destination interface fa0/2`

يعني:

- راقب fa0/1
    
- ابعت نسخة لـ fa0/2
    

---

## مميزاته:

✅ مجاني  
✅ سهل  
✅ مش محتاج أجهزة

---

## عيوبه:

❌ ممكن يضيع packets  
❌ يقلل performance  
❌ مش دقيق مع الترافيك العالي

---

## يستخدم في:

- Lab
    
- Small networks
    
- Testing
    

---

---

# ⚠️ Best Practices (مهم جدًا للامتحانات)

## 1) Placement

حط الـ TAP/Span في المكان الصح:

- قبل firewall؟
    
- بعد firewall؟
    
- داخل LAN؟
    

حسب هدفك

---

## 2) Duration

PCAP بياكل Storage جامد جدًا

### مثال:

1Gbps لمدة يوم = 10.8TB 😱

تخيل:  
10Gbps أو 40Gbps

---

## 3) TAP vs Mirror

لو:

- High traffic → TAP
    
- Small network → Mirror
    

---

---

# 🟣 Tools للتحليل

بعد ما تلتقط الترافيك:

### أشهر أدوات:

- Wireshark → GUI
    
- tcpdump → CLI
    
- Zeek → analysis
    
- Snort/Suricata → IDS/IPS
    

---

## في الكورس:

التركيز غالبًا على Wireshark

---

---

# 🟢 3) Network Statistics (Metadata / Flows)

## الفكرة:

بدل ما تحفظ كل Packet  
👉 احفظ ملخص عنها

يعني:

- مين كلم مين
    
- كام باكيت
    
- كام بايت
    
- قد إيه وقت
    

---

## ليه؟

- أخف
    
- أسرع
    
- مناسب للشبكات الكبيرة
    

---

---

# 🔥 NetFlow

## من Cisco

بيجمع:  
Flow metadata بس

مش packets كاملة

---

## مثال:

`Src IP → Dst IP Bytes Packets Duration`

---

## ممتاز لاكتشاف:

- C2 beaconing
    
- Data exfiltration
    
- Lateral movement
    
- Scanning
    

---

---

# 🔥 IPFIX

## نسخة أحدث من NetFlow

Vendor-neutral

### مميزاته:

- Flexible fields
    
- Works with any vendor
    
- More detailed
    

---

## الجميل:

معظم الأجهزة تدعمه جاهز  
بس تفعل feature وخلاص

---

---

# 🔥 مقارنة مهمة جدًا

|Method|Detail|Storage|Use case|
|---|---|---|---|
|Logs|Low|Very low|quick alerts|
|NetFlow/IPFIX|Medium|Low|anomalies|
|PCAP|Full|Huge|deep investigation|

---

---

# 🎯 الخلاصة الذهبية

## بالعربي:

- Logs → نظرة سريعة
    
- NetFlow → patterns
    
- PCAP → الحقيقة الكاملة
    

لازم تستخدم التلاتة مع بعض.

---

## بالإنجليزي:

Effective NTA combines logs for events, flows for behavior patterns, and full packet capture for deep forensic visibility.

---

# 🧠 جملة تحفظها:

> Logs tell you something happened, flows tell you how much happened, packets tell you exactly what happened.