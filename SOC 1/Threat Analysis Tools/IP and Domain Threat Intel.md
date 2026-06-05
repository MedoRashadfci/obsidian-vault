
---

# 🌐 IP Building Blocks

## 📥 Download Task Files

Network-based threat intelligence is a critical part of modern cybersecurity defense.  
بمعنى إنك كـ SOC analyst مش بس بتشوف IP أو domain، لكن بتحاول تفهم **النية (intent)** وراهم وهل في تهديد حقيقي ولا لأ.

The goal is to analyze IPs and domains beyond connectivity, to support **fast and safe triage for SOC L1 analysts**.  
يعني تحول الـ domain من مجرد اسم إلى **artifact غني بالمعلومات**.

---

# 🔍 Why DNS Matters in the SOC

Every time a user clicks a link or a system resolves a hostname, **DNS** comes into play.  
DNS هو اللي بيحول اسم زي `www.tryhackme.com` إلى IP address.

Because DNS is central to internet functionality, attackers frequently abuse it.  
وده بيخلي DNS مصدر مهم جدًا لاكتشاف الهجمات بدري.

👉 For SOC analysts:  
DNS is one of the **earliest warning data sources**  
لأن الدومين المشبوه بيظهر قبل ما نعرف الـ payload أو الـ hash.

Attackers often:

- Register domains quickly
    
- Change configurations
    
- Abandon them fast
    

وإنت شغلك:  
تحول الدومين إلى معلومات زي:

- مين المالك
    
- بيشاور على أنهي IP
    
- هل بيتغير بسرعة
    
- هل طبيعي ولا مشبوه
    

---

# 🧠 Core DNS Records for Triage

When enriching a domain, focus on these key records:

### 🌍 A / AAAA Records

Maps domain to IPv4 / IPv6 addresses.  
لو لقيت IPs بتتغير بسرعة → 🚨 suspicious behavior

👉 عمليًا:

- خد الـ IP من tools زي nslookup
    
- وحطه في VirusTotal
    

---

### 🧭 NS Records

Identify the nameservers controlling the domain.  
لو حصل تغيير قريب أو NS غريب → احتمال domain جديد أو malicious

---

### 📧 MX Records

حدد السيرفرات الخاصة بالإيميل.  
Attackers ممكن يستخدموها في phishing

👉 لو alert مش متعلق بالإيميل:  
بس سجل وجودها وخلاص

---

### 📝 TXT Records

Used for SPF / DKIM / verification.  
لو مش موجود أو misconfigured → risk أعلى في الإيميل

---

### 🏢 SOA Record

Contains primary authority info  
مفيد في معرفة ownership بشكل مبدئي

---

### ⏳ TTL (Time To Live)

Defines how long DNS results are cached.  
TTL قليل جدًا (seconds/minutes) → 🚨 frequent changes → suspicious

---

# 🧪 SOC Use Case

Example:  
SIEM generates alert for domain:  
`advanced-ip-sccanner[.]com`

Your job as L1 analyst:

- Collect DNS records
    
- Analyze behavior
    
- Decide if malicious or not
    

---

# ⚠️ Attack Techniques Using DNS

### 🔄 Fast Flux Hosting

Attackers rotate IPs بسرعة مع TTL قليل  
→ evade detection

👉 لو لاحظت IPs بتتغير بسرعة وعلى شبكات مختلفة → escalate

---

### ☁️ CDN Abuse

CDNs زي Cloudflare أو Akamai بتغير IPs برضه  
لكن داخل نفس الـ ASN

👉 لو طبيعي → بس اعمل reputation check

---

### 🪤 Typosquatting

Domains شبه الأصل:

- `paypa1.com`
    
- `micros0ft.net`
    

👉 ده high risk جدًا → escalate فورًا

---

### 🌐 IDN Attacks

استخدام Unicode لعمل domains شبه الأصل

Example:  
`xn--ppaypal-3ya.com`

👉 لازم تفك التشفير وتقارنه بالبراند الحقيقي

---

# ⚙️ SOC Analyst Workflow

### 1️⃣ Snapshot DNS

Collect:

- A, NS, MX, TXT, SOA, TTL
    

📌 خليك بسيط وواضح في التجميع

---

### 2️⃣ Ownership Check

Use WHOIS:

- registrar
    
- creation date
    
- contact info
    

ده بيديك فكرة عن المالك

---

### 3️⃣ Interpret Patterns

Analyze behavior:

- CDN طبيعي؟
    
- ولا malicious domain؟
    

---

### 4️⃣ Log Evidence

احفظ:

- screenshots
    
- JSON data
    

علشان audit أو escalation

---

### 5️⃣ Recommend Action

حسب التحليل:

- 🔴 Block → لو malicious
    
- 🟡 Monitor → لو مش واضح
    
- 🟢 Close → لو benign
    

---
----

---

# IP Enrichment: Geolocation and ASN

---

## IP Enrichment Within the SOC

Most alerts coming from SIEM or EDR usually contain at least one IP address.  
بس المشكلة إن الـ IP لوحده ملوش معنى واضح.

ممكن يكون:

- compromised home router
    
- CDN edge
    
- cloud service عليه آلاف المستخدمين
    

فلو اشتغلت عليه كده من غير enrichment:

- يا إما هتعمل overreaction وتقفل حاجة سليمة
    
- يا إما هتعمل underreaction وتسيب attacker شغال براحتو
    

---

## 💡 فكرة Enrichment

Enrichment means adding context to the IP:

- Ownership
    
- ASN
    
- Geolocation
    
- Services
    

علشان القرار يبقى مبني على evidence مش تخمين

وده شغل أساسي لأي SOC L1  
لأن الـ IPs أكتر حاجة بتشوفها في alerts

---

## The Role of RDAP

RDAP is the most reliable source for IP ownership.  
مش زي GeoIP tools اللي ممكن تبقى approximate

الـ RDAP data بيجي من:

- RIPE NCC
    
- ARIN
    
- APNIC
    

---

### من RDAP تقدر تجيب:

- **NetRange** → الرينج بتاع الـ IP
    
- **Organization** → الشركة المالكة
    
- **Remarks** → أحيانًا بيقولك usage
    
- **Abuse Contact** → الإيميل اللي تبلغ عليه
    

---

📌 مثال:  
لو عندك IP مشبوه  
ابدأ بـ RDAP وشوف تبع مين

ولو لقيت traffic غريب:  
pivot على domain أو certificate  
علشان تضيق النطاق بدل ما تعمم

---

## ⚠️ Tip مهمة

حاول تحفظ:

- RDAP JSON
    
- أو screenshot
    

علشان يبقى عندك evidence واضح

---

# Autonomous Systems and Heuristics

---

## يعني إيه AS؟

An Autonomous System is a group of IP ranges تحت إدارة جهة واحدة

وكل AS ليه رقم:  
👉 ASN

---

## ليه ASN مهم؟

لما تعرف ASN  
تقدر تفهم طبيعة الـ IP:

- attacker؟
    
- user عادي؟
    
- cloud service؟
    

---

## 🧠 أنواع الـ ASN

---

### 🏢 Hosting ASN

- شركات استضافة
    
- فيها tenants كتير
    

👉 غالبًا:

- بيستخدمها attackers
    

---

### 🏠 Residential ISP

- زي Vodafone أو WE
    

👉 لو فيه alert:

- غالبًا جهاز user مخترق
    

---

### ☁️ Cloud / CDN

- زي AWS أو Cloudflare
    

👉 خطر هنا:  
❌ متعملش block للرينج كله

---

## 🔥 أمثلة مهمة

- **AS32934 (Facebook/Meta)**  
    traffic منه غالبًا user activity  
    مش malicious hosting
    
- **AS16509 (Amazon AWS)**  
    attackers بيستخدموه كتير  
    بس برضه فيه خدمات شرعية
    
    👉 الحل:  
    block narrow scope بس
    
- **AS124888 (Vodafone)**  
    ISP  
    غالبًا device مخترق
    

---

# Geolocation: Value and Limitations

---

## 🌍 الفكرة

GeoIP tools بتديك:

- country
    
- city
    

---

## ⚠️ المشكلة

- country ممكن يبقى غلط
    
- cloud providers بيستخدموا global infra
    

👉 ممكن IP يظهر:

- US
    
- وهو شغال في أوروبا
    

---

## ⚠️ المدينة تحديدًا

مش reliable خالص

❌ مينفعش تاخد قرار block بناء عليها

---

## ✅ الاستخدام الصح

- خد location من مصدرين
    
- لو مختلفين → سجل ده
    

👉 اعتبره:  
Hint  
مش دليل

---

## 📌 نقطة قانونية

الدولة بتفرق في:

- takedown requests
    
- legal escalation
    

---

# SOC Analyst Workflow

---

## 1. Start with RDAP

شوف:

- netrange
    
- organization
    
- ASN
    
- abuse contact
    

---

## 2. Add ASN Context

استخدم tools زي:

- bgpview
    
- ipinfo
    

علشان تفهم:

- role بتاع الـ IP
    

---

## 3. Check Geolocation

- هات الدولة من مصدرين
    
- سجل أي اختلاف
    

---

## 4. Look for rDNS

reverse DNS ممكن يدي hint

مثال:

- domain فيه "broadband" → ISP
    

بس:  
❌ متعتمدش عليه لوحده

---

## 5. Check Internal Logs

شوف:

- هل الـ IP ظهر قبل كده؟
    
- كان بيعمل إيه؟
    

---

## 6. Classify the IP

حدد:

- Hosting
    
- Residential
    
- Cloud
    

واكتب السبب بتاعك

---

## 7. Plan Outreach

لو confirmed malicious:

- جهز report
    
- وابعت للـ abuse contact
    

---

---
---

---

# Service Exposure

---

## Services and Certificates

When you look at an IP or domain، مش بس تبص على المكان أو الـ ASN  
لكن كمان لازم تشوف: **هو بيشغل إيه services**

الـ exposed services بتقولك كتير عن:

- وظيفة السيستم
    
- والـ risk لو اتهاجم
    

مثال بسيط:  
لو لقيت IP فاتح **RDP على port 3389**  
ده غالبًا target سهل لـ brute-force attacks

---

## Shodan Reconnaissance

Shodan is one of the strongest tools for IP analysis  
بيعمل indexing لكل الأجهزة اللي على الإنترنت

تقدر منه تعرف:

- open ports
    
- running services
    
- system details
    

---

### مثال عملي

لو عندك IP زي:  
`69.197.185.26`

تحطه في Shodan  
وتبدأ تشوف:

---

### 🔹 Open Ports

دي أول حاجة تبص عليها

لو لقيت:

- web server (Nginx)
    
- أو rsync
    

يبقى فيه services ممكن تتهاجم

---

### 🔹 Service Banners

دي بتديك معلومات عن:

- نوع السيرفر
    
- software version
    
- أحيانًا cookies أو metadata
    

👉 دي بتساعدك تعرف:  
هل فيه vulnerability معروفة ولا لأ

---

## TLS Certificate Transparency

Tools زي:  
crt.sh

بتديك معلومات عن الـ certificates

ودي كنز بصراحة في التحليل

---

### أهم الحاجات اللي تبص عليها:

---

### 🔹 Issuer

مين اللي عمل signing للـ certificate

- Let's Encrypt → طبيعي
    
- Self-signed → ممكن يكون suspicious
    

---

### 🔹 Validity Period

مدة صلاحية الشهادة

- 90 يوم → طبيعي
    
- reissued كتير بسرعة → suspicious
    

---

### 🔹 SAN (Subject Alternative Names)

domains المرتبطة بالشهادة

👉 لو لقيت domains كتير ومش related  
→ ده ممكن يبقى phishing infra

---

## Censys Search

Censys زي Shodan  
بس أحيانًا أدق شوية

بيطلع:

- services على ports غريبة
    
- تفاصيل advanced أكتر
    

مثال:  
ممكن تلاقي port زي:  
`56003/SSH`

وده مش standard  
→ محتاج تاخده في اعتبارك

---

## SOC Analyst Workflow

---

### 1. Check Shodan / Censys

شوف:

- open ports
    
- services
    
- misconfigurations
    

---

### 2. Review TLS Certificates

سجل:

- issuer
    
- SANs
    
- validity
    

---

### 3. Look for anomalies

دور على حاجات غريبة زي:

- domains شبه brands
    
- certificates كتير في وقت قصير
    
- SANs مش مترابطة
    

---

### 4. Pivot

استخدم المعلومات اللي لقيتها:

- certificate
    
- banner
    

علشان توصل لـ infra تانية مرتبطة

---

### 5. Assess Blast Radius

دي أهم خطوة

---

#### 🔸 RDP / SSH على Residential ASN

→ غالبًا جهاز user مخترق

---

#### 🔸 TLS فيه SANs كتير على CDN

→ shared infra  
❌ متعملش block للـ IP

---

#### 🔸 Self-signed cert على IP صغير

→ غالبًا:

- attacker panel
    
- أو proxy
    

---
---
---

# 🌐 Reputation Checks and Passive DNS

---

## 🎯 Why Reputation and History Matter

At this stage of enrichment, we already know:

- Who owns the IP/domain
    
- What services it exposes
    

لكن لسه ناقص أهم نقطة 👇  
👉 **What this infrastructure has been doing over time**

---

### ⚠️ Important Concept

- File hashes → **Static**
    
- IPs & Domains → **Dynamic**
    

يعني:

- الدومين ممكن يبقى phishing النهاردة
    
- وبعد كام يوم يبقى benign
    

👉 نفس الكلام على الـ IP  
ممكن يتنقل بين users أو services بسرعة

---

## 💡 Conclusion

👉 Time context is critical  
السياق الزمني مهم جدًا في التحليل

---

# 🔍 Reputation Services

---

## 🟢 VirusTotal

One of the most important tools in SOC.

It provides:

- Detection ratio
    
- Relationships (IPs, domains, files)
    

👉 تقدر تعرف:  
هل الـ indicator ده معروف كـ malicious ولا لأ

---

## 🔵 Cisco Talos Intelligence

Provides:

- Reputation scores
    
- Category labels (benign / spam / malware)
    

---

### 📊 Talos Dashboard

Shows:

- Global email traffic
    
- Classification:
    
    - Legitimate
        
    - Spam
        
    - Malware
        

👉 Useful for:  
تحليل IP أو domain مرتبط بإيميلات

---

### 🧠 Additional Features

#### 🔹 Vulnerability Info

- CVEs
    
- CVSS scores
    
- Zero-day reports
    

👉 مهم جدًا في فهم المخاطر

---

#### 🔹 Reputation Center

- Search by IP أو Hash (SHA256)
    
- Used heavily in investigations
    

---

## 🟣 IP2Proxy

Used to detect:

- VPN
    
- Proxy
    
- Tor
    

👉 دي shared exit points  
يعني:  
❌ attribution ضعيف

---

# 🌍 Passive DNS

---

## 🎯 What is it?

Provides historical DNS data  
يعني:  
بيوريك الدومين كان بيشير على إيه قبل كده

---

## 🔑 Key Signals

---

### 1️⃣ First Seen / Last Seen

- First Seen → إمتى ظهر
    
- Last Seen → آخر نشاط
    

👉 لو domain جديد → suspicious

---

### 2️⃣ Number of IPs

- عدد IPs خلال فترة معينة
    

👉 لو بيتغير بسرعة:  
→ 🚨 Fast Flux / malicious

---

### 3️⃣ ASN Spread

- لو IPs من ASNs مختلفة جدًا  
    → 🚨 suspicious
    
- لو من ASN واحد  
    → غالبًا CDN أو stable
    

---

# 🔎 Additional Intelligence Sources

---

## 🔐 Certificate Transparency (CT Logs)

Shows:

- SSL certificates history
    

👉 Useful for:

- detecting phishing campaigns
    
- domains created in bulk
    

---

## 🕰️ Wayback Machine

Shows:

- old versions of websites
    

👉 مثال:

- كان blog عادي
    
- وبقى phishing فجأة
    

→ 🚨 High risk

---

# ⚙️ SOC Analyst Workflow

---

## 1️⃣ Check VirusTotal

- Detection ratio
    
- First Seen / Last Seen
    
- Community comments
    

---

## 2️⃣ Check Cisco Talos

- Reputation score
    
- Category
    
- Any recent changes
    

---

## 3️⃣ Check IP2Proxy

- هل VPN / Proxy / Tor
    

👉 Adjust severity بناءً على ده

---

## 4️⃣ Check Passive DNS

- First Seen
    
- Last Seen
    
- Number of IPs
    
- ASN spread
    

---

## 5️⃣ Check CT Logs

- certificate bursts
    
- suspicious domains
    

---

## 6️⃣ Check Wayback Machine

- هل الموقع اتغير؟
    

👉 benign → phishing = 🚨

---

## 7️⃣ Final Decision

Based on analysis:

- 🔴 Block → لو malicious
    
- 🟡 Monitor → لو مش واضح
    
- 🟢 Close → لو benign
    

👉 مع تحديد مدة (expiry)

---

# 🎯 Final Takeaway

👉 Reputation + History = Context

بدونهم:  
❌ التحليل ناقص

---

## 🧠 Golden Rule

> "Don't judge an IP/domain by its current state only…  
> judge it by its behavior over time."

---
---
---

# Operational Integration

في المرحلة دي، المطلوب من الـ analyst إنه يعرف يحوّل الـ intelligence لقرارات عملية من غير ما يبوّظ الدنيا.  
المشكلة الأساسية هنا إنك ممكن تعمل block زيادة عن اللزوم فتأثر على شغل حقيقي، أو العكس تفوّت attack.

الحل ببساطة:  
تشتغل بدقة + تخطط إن أي قرار يبقى له مدة ويتراجع بعد كده.

---

## Safe Integration Patterns

### Prefer hostnames when domains are stable

لو الدومين ثابت، استخدمه بدل الـ IP  
لأن الـ IPs بتتغير كتير خصوصًا مع CDNs و anycast

يعني:  
اشتغل بـ:

- DNS policies
    
- Proxy filtering
    
- SNI filtering
    

ده أدق بكتير من إنك تمسك IP

---

### Use narrow IPs for single-purpose VPS

لو عندك IP واضح إنه مخصص لهجوم (مثلاً C2 أو staging server)

ساعتها:  
اعمل block على /32  
يعني IP واحد بس

ده بيقلل الضرر الجانبي لأقل حاجة

---

### Set expiry on blocks

دي نقطة ناس كتير بتغلط فيها

الـ infrastructure بيتغير وبيت recycled  
فمينفعش تعمل block forever

الأفضل:

- 7 أيام
    
- أو 14 يوم
    

ولو الـ indicator ظهر تاني → يتجدد تلقائي

---

### Document evidence in SOAR

أي قرار تاخده لازم يكون وراه دليل

حط:

- screenshots
    
- RDAP data
    
- certificates
    
- reasoning بتاعك
    

عشان لو حد راجع وراك يفهم انت عملت كده ليه

---

## Geofencing Cautions

موضوع إنك تعمل block لدولة كاملة شكله مغري، بس في الحقيقة بيكسر شغل كتير

ليه؟

- ناس بتسافر
    
- services بتستخدم servers بره
    
- شركات بتعمل routing من دول تانية
    

فإحنا بنستخدم geolocation كـ:

- indicator يساعدنا نفهم  
    مش كـ قرار block مباشر
    

إلا لو القرار ده اتراجع مع business واتجرب كويس

---

## Cloud and Large Provider Pitfalls

أكبر غلطة ممكن تعملها:

إنك تعمل block لـ:

- AWS
    
- Microsoft
    
- Cloudflare
    

الـ providers دول بيستخدموا نفس الـ IPs لناس كتير

فلو عملت block:  
هتلاقي systems عندك وقعت فجأة

---

الحل الصح:

لو domain معين malicious:

- block الدومين
    
- أو حتى path معين
    

وممكن كمان:  
تبلغ الـ provider نفسه (abuse report)

---

## Legal and Provider Considerations

معرفة الـ provider والدولة بيساعدك تقرر:

- هل ينفع تعمل takedown بسرعة؟
    
- ولا الموضوع هيطول؟
    

في providers:

- بيردوا بسرعة
    
- وعندهم abuse teams قوية
    

وفي غيرهم:

- بياخد وقت
    
- أو القوانين عندهم معقدة
    

فلازم تسجل:

- RIR info
    
- abuse contacts
    

عشان لو احتجت escalation

---

## From Data to Decision

دي بقى الخلاصة العملية اللي تمشي عليها:

---

### 1. Verify

اتأكد إن الـ indicator فعلاً ظهر عندك في logs  
ومش حاجة مالهاش علاقة بالبيئة بتاعتك

---

### 2. Enrich

اجمع معلومات:

- ASN
    
- Geolocation
    
- Certificates
    
- Reputation
    
- History
    

---

### 3. Score

قيّم الموضوع

هل:

- malicious واضح
    
- ولا مش مؤكد
    

واكتب السبب

---

### 4. Decide

خد القرار:

- Block
    
- Monitor
    
- Allow
    

بس خليك دايمًا:

- دقيق
    
- حاطط expiry
    
- موثق كل حاجة
    

---
---

