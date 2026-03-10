https://tryhackme.com/room/networkdiscoverydetection
## Network Discovery – الفكرة العامة

**Network Discovery** هي أول مرحلة تقريبًا في أي هجوم سيبراني.  
المهاجم (Attacker) بيبدأ بمحاولة **فهم الشبكة** قبل ما يهاجمها.

> الفكرة الأساسية:  
> **"You can’t hack what you don’t know."**

---

## 🧨 Attackers and Network Discovery (من منظور المهاجم)

### 🎯 Attack Surface

المهاجم في البداية بيدوّر على **Attack Surface**  
يعني كل حاجة exposed على الإنترنت وتقدر تتشاف أو تتهاجم.

أثناء مرحلة الـ **Discovery Phase**، الـ attacker بيحاول يعرف:

### 1️⃣ What assets can be accessed?

- Web servers
    
- Mail servers
    
- VPN gateways
    
- APIs
    
- Cloud services
    
- Remote access services (RDP, SSH)
    

يعني أي **Asset** ظاهر للـ Public Internet.

---

### 2️⃣ IP addresses, Ports, OS, Services

وده اللي إنت أكيد عارفه كويس:

- IP Addresses (Internal / External)
    
- Open Ports (80, 443, 22, 3389…)
    
- Operating System (Linux, Windows, BSD…)
    
- Running Services (Apache, Nginx, IIS, MySQL…)
    

مثال:

`192.168.1.10 Port 80 → Apache OS → Ubuntu Linux`

---

### 3️⃣ Service Versions & Vulnerabilities

دي مرحلة خطيرة جدًا 🔥

المهاجم بيسأل:

- What **version** of the service is running?
    
- Is this version **vulnerable**?
    
- Is there a **known CVE** for it?
    

مثال:

`Apache 2.4.49 → CVE-2021-41773 (Path Traversal)`

💡 هنا المهاجم يبدأ يقول:

> "Aha! Found my entry point."

---

### 🧠 In Short (Attacker Goal)

**The attacker is trying to find ANY opening**  
أي حاجة:

- Misconfiguration
    
- Unpatched vulnerability
    
- Weak service
    
- Exposed credentials  
    تخليه يدخل الشبكة.
    

---

## 🛡️ Network Scanning – Attacker vs Defender

نقطة مهمة جدًا:

> **Network Scanning مش دايمًا حاجة شريرة**

---

## 🛡️ Defenders and Network Discovery (من منظور المدافع)

الـ **Defenders / Blue Team / SOC** بيعملوا Network Discovery لكن لهدف عكسي تمامًا.

### أهداف الـ Defender:

### 1️⃣ Asset Inventory

- يعرف كل الأجهزة الموجودة في الشبكة
    
- Servers
    
- Routers
    
- Switches
    
- Endpoints
    

> "If you don’t know it exists, you can’t protect it."

---

### 2️⃣ Reduce Exposure

- تأكد إن مفيش IP أو Port مفتوح من غير سبب
    
- أي Service شغالة لازم تكون:
    
    - Needed
        
    - Secured
        
    - Monitored
        

مثال:

`Why is FTP (21) open? Is it really needed?`

---

### 3️⃣ Patch Vulnerabilities

- سد الثغرات المعروفة
    
- Patch critical & exploitable vulnerabilities
    
- حتى لو مش كل الثغرات، على الأقل **High Risk ones**
    

---

### 🧠 In Short (Defender Goal)

**Reduce the attack surface as much as possible**  
يعني:

- أقل Ports
    
- أقل Services
    
- أقل Misconfigurations
    

---

## ⚠️ The Challenge: Detecting Network Discovery

هنا بقى المشكلة الكبيرة 🔴

### ليه؟

لأن:

- Attackers scan networks
    
- Defenders scan networks
    
- Search engines scan networks (Shodan, Censys)
    
- Research orgs scan networks
    
- Cloud providers scan networks
    

📌 من ناحية الـ SOC:

> "إزاي أفرق بين Scan طبيعي و Scan خبيث؟"

---

## 🧠 SOC Detection Techniques

### 1️⃣ Allowlisting

- Allow internal scanners
    
- Allow known benign external scanners
    
- عشان متطلعش Alerts غلط (False Positives)
    

---

### 2️⃣ Threat Intelligence Integration

- ربط الـ SIEM بـ Threat Intel feeds
    
- لو IP معروف إنه:
    
    - Malicious
        
    - Scanner bot
        
    - APT infrastructure  
        → alert فورًا
        

---

### 3️⃣ Risk-Based Alerting

بدل:

> Alert = ON / OFF

بيعملوا:

- Low severity scan → Low alert
    
- Known bad IP → High severity alert
    

Generic behavioral rules:

- Port scanning
    
- Network sweeping
    
- Unusual connection patterns
    

# 🔍 External vs Internal Scanning

(Network Discovery from SOC Perspective)

## 📌 Network Discovery Behavior

أثناء شغل الـ **SOC Team**، بيظهر نشاط اسمه **Scanning / Network Discovery**  
وده معناه إن في حد بيحاول:

> Map hosts  
> Discover services  
> Identify potential targets

والـ scanning ده نوعين أساسيين:

- **External Scanning**
    
- **Internal Scanning**
    

---

## 🌍 External Scanning Activity

### 🔹 يعني إيه External Scanning؟

External Scanning هو:

> Scanning activity coming **from outside the organization’s network**  
> targeting **internal or public-facing assets**

### 📍 From SOC Logs Perspective:

- **Source IP** → External / Public IP
    
- **Destination IP** → Organization IP (Public-facing asset)
    

مثال:

`Source IP: 203.0.113.25   (External) Destination IP: 192.168.230.145 (Org asset)`

---

### 🎯 What does this mean in MITRE ATT&CK?

هذا النوع من النشاط يقع ضمن:

## 🧠 Reconnaissance Phase – MITRE ATT&CK

> Reconnaissance = attacker is still outside  
> trying to understand the environment

يعني:

- ❌ No foothold yet
    
- ❌ No internal access
    
- ✅ Just probing & scanning
    

---

### 🚨 Severity Level

- **Low Severity**
    
- Still dangerous, but early stage
    

ليه؟

> Because attacker hasn’t compromised anything yet

---

### 🛡️ SOC Response to External Scanning

عادة الـ SOC analyst يعمل:

- Block source IP on **Perimeter Firewall**
    
- Add IP to blocklist / blacklist
    
- Monitor for repeated attempts
    

⚠️ Important note:

> Attacker may return using:

- VPN
    
- Proxy
    
- Tor
    
- Another IP
    

يعني الـ blocking مش حل نهائي.

---

## 🏢 Internal Scanning Activity

### 🔹 يعني إيه Internal Scanning؟

Internal Scanning هو:

> Scanning initiated **from inside the organization’s network**

وده أخطر بكتير 🔥

---

### 📍 From SOC Logs Perspective:

- **Source IP** → Internal / Private IP
    
- **Destination IP** → Internal / Private IP
    

مثال:

`Source IP: 192.168.1.50 Destination IP: 192.168.1.100`

---

### 🚨 What does this indicate?

ده معناه إن:

> ❗ Attacker already has a foothold inside the network

يعني:

- Malware
    
- Compromised host
    
- Stolen credentials
    
- Insider threat
    

---

### 🎯 MITRE ATT&CK Phase

This maps to:

## 🧠 Discovery Phase – MITRE ATT&CK

وفي بعض frameworks تسمى:

- **Internal Reconnaissance**
    

الهدف هنا:

- Enumerate internal hosts
    
- Find file servers
    
- Identify AD controllers
    
- Prepare for lateral movement
    

---

### 🔄 What comes next?

عادة بعد Internal Scanning:

- Lateral Movement
    
- Privilege Escalation
    
- Data Exfiltration
    

---

### 🚨 Severity Level

- **HIGH Severity**
    
- 🚨 Critical Alert
    

ليه؟

> Because the attacker is already inside your network

---

### 🛡️ SOC Response to Internal Scanning

هنا **Firewall blocking is NOT enough** ❌

SOC analyst لازم:

1. Verify it’s not authorized activity
    
2. Escalate the alert
    
3. Trigger **Incident Response (IR)**
    
4. Investigate the host
    
5. Perform **Root Cause Analysis**
    
6. Possibly isolate the system
    

---

## 🧠 Summary Comparison

|Aspect|External Scanning|Internal Scanning|
|---|---|---|
|Source IP|External / Public|Internal / Private|
|Destination|Org assets|Org assets|
|MITRE Phase|Reconnaissance|Discovery|
|Foothold|❌ No|✅ Yes|
|Severity|Low|High|
|SOC Action|Block IP|Incident Response|

---

## 🔎 Identifying Scanning in Firewall Logs

SOC analysts بيعتمدوا على **Logs** زي:

- Firewall logs
    
- Zeek logs
    
- SIEM exports (CSV)
    

---

### 📄 Example: Firewall / Zeek Log (CSV)

استخدام:

`head -n2 log-session-1.csv`

الغرض:

- Preview file
    
- Understand columns
    
- See source.ip & destination.ip
    

---

### 🧩 Important Fields to Focus On:

- `source.ip`
    
- `destination.ip`
    
- `source.port`
    
- `destination.port`
    
- `network.protocol`
    
- `conn_state`
    
- `event.dataset`
    

---

### ⚠️ CSV Parsing Challenge

لأن:

- Timestamp يحتوي على comma  
    ده بيخلي حساب الأعمدة tricky شوية.
    

علشان كده:

`cut -d',' -fX`

لازم تحسب الأعمدة صح.

---

## 🧠 SOC Mindset (Very Important)

> Not every scan is an attack  
> But every internal scan is suspicious until proven otherwise

# 🔍 Horizontal vs Vertical Scanning

(Port Scanning Techniques)

بعد ما الـ attacker يعرف **what hosts exist** في الشبكة، الخطوة الطبيعية اللي بعدها هي:

> **Which ports are open on these hosts?**

وده اسمه **Port Scanning**.

---

## 🟦 Horizontal Scanning

### 🔹 What is Horizontal Scanning?

Horizontal scan يعني:

> Scanning **the same port**  
> across **multiple destination IPs**

يعني:

- Same **source IP**
    
- Same **destination port**
    
- Different **destination IPs**
    

---

### 🎯 Why attackers do it?

الهدف:

- Identify which hosts expose a **specific service**
    
- Exploit a **known vulnerability** on that port
    

---

### 💣 Real-world example: WannaCry

- WannaCry exploited **SMBv1**
    
- SMB runs on **port 445**
    
- Malware scanned the network for:
    

`ANY host with port 445 open`

ده textbook example لـ **Horizontal Scan**.

---

### 🔍 How it looks in logs

في اللوجات هتشوف:

`Source IP: SAME Destination Port: SAME (e.g. 445) Destination IP: DIFFERENT`

📌 SOC Rule:

> Same src.ip + same dest.port + many dest.ip = Horizontal Scan

---

## 🟥 Vertical Scanning

### 🔹 What is Vertical Scanning?

Vertical scan يعني:

> Scanning **multiple ports**  
> on **a single host**

يعني:

- Same **source IP**
    
- Same **destination IP**
    
- Different **destination ports**
    

---

### 🎯 Why attackers do it?

الهدف:

- Footprinting a specific host
    
- Identify running services
    
- Find weakest service
    

Example:

- Public-facing web server
    
- Critical database server
    
- Domain controller
    

---

### 🔍 How it looks in logs

`Source IP: SAME Destination IP: SAME Destination Ports: DIFFERENT`

📌 SOC Rule:

> Same src.ip + same dest.ip + many ports = Vertical Scan

---

## 🟪 Mixed Scanning

Sometimes attackers combine both:

- Horizontal (spread)
    
- Vertical (depth)
    

وده أخطر لأنه:

- Maps network
    
- Profiles hosts
    

---

# 🛡️ SOC Detection – Practical Thinking

|Scan Type|Same Source|Same Dest IP|Same Dest Port|
|---|---|---|---|
|Horizontal|✅|❌|✅|
|Vertical|✅|✅|❌|

---

# 🔧 The Mechanics of Scanning

(كيف تتم عمليات Network Scanning فعليًا)

بعد ما فهمنا:

- External vs Internal
    
- Horizontal vs Vertical
    

دلوقتي نجاوب السؤال المهم:

> **How do scanners actually work on the network level?**

---

## 🟢 Ping Sweep (ICMP Scan)

### 🔹 What is Ping Sweep?

Ping Sweep هو:

> Sending ICMP Echo Request packets  
> to multiple hosts to check who is alive

يعني:

`Ping 192.168.1.1 Ping 192.168.1.2 Ping 192.168.1.3 ...`

---

### 🧠 Protocol Used

- **ICMP (Internet Control Message Protocol)**
    

### 🔁 Behavior

- Scanner sends → ICMP Echo Request
    
- If host is online → ICMP Echo Reply
    

---

### 🎯 Purpose

- Identify **live hosts**
    
- Host discovery only (no ports yet)
    

---

### ❌ Weakness

- ICMP often **blocked by firewalls**
    
- Easy to detect
    
- Easy to defeat
    

📌 That’s why modern attackers don’t rely heavily on ping sweeps.

---

### 🔍 How Ping Sweep looks in logs

- ICMP traffic
    
- Many destination IPs
    
- Same source
    
- Short time window
    

---

## 🔵 TCP SYN Scan (Stealth Scan)

### 🔹 TCP Three-Way Handshake (Important)

Normal TCP connection:

`1. SYN 2. SYN-ACK 3. ACK`

---

### 🔹 How TCP SYN Scan works

Scanner does:

1. Sends **SYN**
    
2. If receives **SYN-ACK** → port is OPEN
    
3. Scanner does NOT send ACK (connection not completed)
    

📌 Hence the name:

> **Half-open scan**

---

### 🎯 Purpose

- Identify:
    
    - Online hosts
        
    - Open TCP ports
        

---

### 🕵️ Why is it stealthy?

- Looks like normal TCP traffic
    
- No full session established
    
- Often blends with legit traffic
    

---

### 🔍 Log indicators

- Many SYN packets
    
- Many connections stuck in:
    
    - `S0`
        
    - `SYN_SENT`
        
- No completed handshake
    

---

### 🚨 SOC Insight

TCP SYN scans are:

- Harder to detect
    
- Very common in real attacks
    
- Often used by Nmap (`-sS`)
    

---

## 🟣 UDP Scan

### 🔹 How UDP Scan works

Scanner sends:

- Empty UDP packet to a port
    

Possible outcomes:

### 1️⃣ Port CLOSED

- Host replies:
    

`ICMP Port Unreachable`

➡️ Host is online, port closed

---

### 2️⃣ No response

- Scanner waits until timeout
    
- Marks port as:
    

`Open | Filtered`

⚠️ Not reliable

---

### 3️⃣ UDP response (rare)

- Confirms port is OPEN
    

---

### ⏳ Drawbacks of UDP Scan

- Slow
    
- Unreliable
    
- Depends on timeouts
    
- ICMP rate limiting affects results
    

---

### 🔍 Log indicators

- UDP packets
    
- ICMP Port Unreachable messages
    
- Long delays between events
    

---

## 🧠 Summary of Scan Types

|Scan Type|Protocol|Purpose|Reliability|Detectability|
|---|---|---|---|---|
|Ping Sweep|ICMP|Host discovery|Low|Easy|
|TCP SYN|TCP|Host + Port|High|Medium|
|UDP Scan|UDP + ICMP|Port discovery|Low|Hard|

---

## 🛡️ Internal Scans by Organizations

Important SOC concept 👇  
مش كل Scan = Attack ❗

### Organizations scan internally to:

- Find vulnerabilities
    
- Discover rogue assets
    
- Reduce attack surface
    
- Compliance checks
    

---

### SOC Best Practice

SOC analyst لازم يعرف:

- Scanner IPs
    
- Scan schedule
    
- Scan type
    

📌 And:

> Exclude these from detection rules (Allowlist)

علشان تقلل:

- False positives
    
- Alert fatigue