
Search :

with Number of packet  --> frame.number == num

with domain >> http.host == domain name

|                              |                                                                                                                                                             |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Filter                       | Description                                                                                                                                                 |
| `ip`                         | Show all IP packets.                                                                                                                                        |
| `ip.addr == 10.10.10.111`    | Show all packets containing IP address 10.10.10.111.                                                                                                        |
| `ip.addr == 10.10.10.0/24`   | Show all packets containing IP addresses from 10.10.10.0/24 subnet.                                                                                         |
| `ip.src == 10.10.10.111`     | Show all packets originated from 10.10.10.111                                                                                                               |
| `ip.dst == 10.10.10.111`     | Show all packets sent to 10.10.10.111                                                                                                                       |
| ip.addr **vs** ip.src/ip.dst | **Note:** The ip.addr filters the traffic without considering the packet direction. The ip.src/ip.dst filters the packet depending on the packet direction. |
## TCP and UDP Filters

|                       |                                                 |                       |                                                 |
| --------------------- | ----------------------------------------------- | --------------------- | ----------------------------------------------- |
| Filter                | Description                                     | Filter                | Expression                                      |
| `tcp.port == 80`      | Show all TCP packets with port 80               | `udp.port == 53`      | Show all UDP packets with port 53               |
| `tcp.srcport == 1234` | Show all TCP packets originating from port 1234 | `udp.srcport == 1234` | Show all UDP packets originating from port 1234 |
| `tcp.dstport == 80`   | Show all TCP packets sent to port 80            | `udp.dstport == 5353` | Show all UDP packets sent to port 5353          |

## Application Level Protocol Filters | HTTP and DNS
|                                 |                                                |                           |                          |
| ------------------------------- | ---------------------------------------------- | ------------------------- | ------------------------ |
| Filter                          | Description                                    | Filter                    | Description              |
| `http`                          | Show all HTTP packets                          | `dns`                     | Show all DNS packets     |
| `http.response.code == 200`     | Show all packets with HTTP response code "200" | `dns.flags.response == 0` | Show all DNS requests    |
| `http.request.method == "GET"`  | Show all HTTP GET requests                     | `dns.flags.response == 1` | Show all DNS responses   |
| `http.request.method == "POST"` | Show all HTTP POST requests                    | `dns.qry.type == 1`       | Show all DNS "A" records |
## Filter: "contains"

|                 |                                                                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Filter          | **contains**                                                                                                                                 |
| **Type**        | Comparison Operator                                                                                                                          |
| **Description** | Search a value inside packets. It is case-sensitive and provides similar functionality to the "Find" option by focusing on a specific field. |
| **Example**     | Find all "Apache" servers.                                                                                                                   |
| **Workflow**    | List all HTTP packets where packets' "server" field contains the "Apache" keyword.                                                           |
| **Usage**       | `http.server contains "Apache"`                                                                                                              |

## Filter: "matches"

|             |                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| Filter      | **matches**                                                                                                   |
| Type        | Comparison Operator                                                                                           |
| Description | Search a pattern of a regular expression. It is case insensitive, and complex queries have a margin of error. |
| **Example** | Find all .php and .html pages.                                                                                |
| Workflow    | List all  HTTP packets where packets' "host" fields match keywords ".php" or ".html".                         |
| **Usage**   | `http.host matches "\.(php\|html)"`                                                                           |
## Filter: "in"

|             |                                                                                |
| ----------- | ------------------------------------------------------------------------------ |
| Filter      | **in**                                                                         |
| Type        | Set Membership                                                                 |
| Description | Search a value or field inside of a specific scope/range.                      |
| Example     | Find all packets that use ports 80, 443 or 8080.                               |
| Workflow    | List all TCP packets where packets' "port" fields have values 80, 443 or 8080. |
| Usage       | `tcp.port in {80 443 8080}`                                                    |

## Filter: "upper"

|             |                                                                                                             |
| ----------- | ----------------------------------------------------------------------------------------------------------- |
| Filter      | **upper**                                                                                                   |
| Type        | Function                                                                                                    |
| Description | Convert a string value to uppercase.                                                                        |
| Example     | Find all "APACHE" servers.                                                                                  |
| Workflow    | Convert all  HTTP packets' "server" fields to uppercase and list packets that contain the "APACHE" keyword. |
| Usage       | `upper(http.server) contains "APACHE"`                                                                      |

## Filter: "lower"

|             |                                                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------------------------------- |
| Filter      | **lower**                                                                                                        |
| Type        | Function                                                                                                         |
| Description | Convert a string value to lowercase.                                                                             |
| Example     | Find all "apache" servers.                                                                                       |
| Workflow    | Convert all  HTTP packets' "server" fields info to lowercase and list packets that contain the "apache" keyword. |
| **Usage**   | `lower(http.server) contains "apache"`                                                                           |
## Filter: "string"

|             |                                                                                          |
| ----------- | ---------------------------------------------------------------------------------------- |
| Filter      | **string**                                                                               |
| Type        | Function                                                                                 |
| Description | Convert a non-string value to a string.                                                  |
| Example     | Find all frames with odd numbers.                                                        |
| Workflow    | Convert all "frame number" fields to string values, and list frames end with odd values. |
| Usage       | `string(frame.number) matches "[13579]$"`                                                |

## Nmap Scans

Nmap is an industry-standard tool for mapping networks, identifying live hosts and discovering the services. As it is one of the most used network scanner tools, a security analyst should identify the network patterns created with it. This section will cover identifying the most common Nmap scan types.

- TCP connect scans
- SYN scans
- UDP scans

It is essential to know how Nmap scans work to spot scan activity on the network. However, it is impossible to understand the scan details without using the correct filters. Below are the base filters to probe Nmap scan behaviour on the network. 

**TCP flags in a nutshell:**

|   |   |
|---|---|
|**Notes**|**Wireshark Filters**|
|Global search.|- `tcp`<br><br>- `udp`|
|- Only SYN flag.<br>- SYN flag is set. The rest of the bits are not important.|- `tcp.flags == 2`<br><br>- `tcp.flags.syn == 1`|
|- Only ACK flag.<br>- ACK flag is set. The rest of the bits are not important.|- `tcp.flags == 16`<br><br>- `tcp.flags.ack == 1`|
|- Only SYN, ACK flags.<br>- SYN and ACK are set. The rest of the bits are not important.|- `tcp.flags == 18`<br><br>- `(tcp.flags.syn == 1) and (tcp.flags.ack == 1)`|
|- Only RST flag.<br>- RST flag is set. The rest of the bits are not important.|- `tcp.flags == 4`<br><br>- `tcp.flags.reset == 1`|
|- Only RST, ACK flags.<br>- RST and ACK are set. The rest of the bits are not important.|- `tcp.flags == 20`<br><br>- `(tcp.flags.reset == 1) and (tcp.flags.ack == 1)`|
|- Only FIN flag<br>- FIN flag is set. The rest of the bits are not important.|- `tcp.flags == 1`<br><br>- `tcp.flags.fin == 1`|

#  Traffic Analysis

## Nmap Scans

Nmap is an industry-standard tool for mapping networks, identifying live hosts and discovering the services. As it is one of the most used network scanner tools, a security analyst should identify the network patterns created with it. This section will cover identifying the most common Nmap scan types.

- TCP connect scans
- SYN scans
- UDP scans

### ✔ TCP Connect Scan:

- **يكمل الـ 3-way handshake بالكامل**:
    
    1. SYN →
        
    2. ← SYN/ACK
        
    3. ACK →
        
- بعد ما يتأكد إن البورت مفتوح، يقوم بعمل **RST** لإنهاء الاتصال بسرعة.
    
- يستخدمه **اليوزر غير الروت (non-privileged)** لأنّه لا يحتاج صلاحيات raw sockets.
    
- في Nmap يظهر كـ:
    
    `nmap -sT`
    

### ✘ SYN Scan (Half-open):

- ميكملش الاتصال
    
- يحتاج root  
    (ده مش موضوعنا الآن)
    

---

# 🟢 ماذا يحدث لو البورت مفتوح؟

Sequence مفتوح:

1. **SYN →**
    
2. **← SYN/ACK**
    
3. **ACK →** (ده تأكيد إن البورت مفتوح فعلاً)
    
4. **RST →** لإنهاء الاتصال فورًا (لأنه Scan مش اتصال حقيقي)
    

---

# 🔴 ماذا يحدث لو البورت مغلق؟

Sequence مغلق:

1. **SYN →**
    
2. **← RST/ACK** (معناه البورت مقفول)
    

مفيش ACK بعدها لأن الاتصال فشل.

---

# 🎯 لماذا هذا الفلتر؟

`tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024`

## ✔ تفسير الفلتر:

### **1) tcp.flags.syn == 1**

معناه:  
**الحزمة فيها SYN →**  
ده هو بداية أي محاولة اتصال.

### **2) tcp.flags.ack == 0**

معناه:  
الحزمة ليست SYN/ACK بل **SYN فقط**  
وده يحدد إن الحزمة الأولى من الاتصال هي اللي بنشوفها.

يعني الفلتر يجيب **أول خطوة بس** من محاولة الاتصال — وهي أهم خطوة لتحديد إذا كانت عملية Scan.

### **3) tcp.window_size > 1024**

هنا المفيد جدًا…

ليه نستخدمه؟

لأن **TCP Connect Scan** عادة يكون:

- **طلب اتصال كامل**
    
- **الـ Window Size كبير** (أكبر من 1024)  
    ليه؟  
    لأن العميل يتوقع أن يستقبل بيانات فعلية من السيرفر بعد الاتصال، مش زي الـ SYN Scan اللي بيبقى Window صغير جدًا.
    

ده يجعل الفلتر يميز:

- بين **Traffic عادي**
    
- وبين **Scan محاولات اتصال سريعة ومتكررة**
    

---

# 🎯 ما الذي يعمله الفلتر عمليًا؟

➡ يجلب **كل الحزم من نوع SYN**  
➡ وغير المصاحبة لـ ACK  
➡ ومع Window Size كبير  
➡ يعني يجلب "محاولات بدء اتصال" والتي غالبًا تكون جزء من TCP Connect Scan

فبسهولة تقدر تشوف:

- IP الماسح (scanner)
    
- البورتات اللي بيحاول يوصل لها
    
- معدل المحاولات (سرعة scan)
    
- الأنماط المتكررة اللي تثبت إنها Scan

## ✅ **SYN Scan**

يعني بيبدأ الــ handshake وما يكملوش.

## ليه؟

علشان:

1. سريع جدًا
    
2. مش واضح في اللوجز (stealthy)
    
3. ما بيكملش الــ connection
    

---

# 🔵 **السيناريو الأول: لو البورت مفتوح**

الاتصال بيكون كده:

1. **Client → Server : SYN**
    
2. **Server → Client : SYN, ACK**
    
3. **Client → Server : RST** علشان يمسح نفسه وما يكملش الاتصال
    

🟢 **ليه RST؟**  
علشان يقول للسيرفر “سيبك، أنا مش عايز أكمل”، فميكملش الـ handshake.

---

# 🔴 **السيناريو الثاني: لو البورت مقفول**

الاتصال بيكون كده:

1. **Client → Server : SYN**
    
2. **Server → Client : RST, ACK**
    

وخلاص.

---

# ⚙️ **ليه الــ SYN Scan بيبقى window size صغير؟**

لأن الــ connection مش هيكتمل  
فمفيش data متوقعة  
علشان كده:  
🔹 **window size <= 1024**

---

# 🟡 **فهم الفلتر اللي في الآخر**

الفلتر:

`tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024`

## يعني إيه؟

🔸 **tcp.flags.syn==1**  
الـ packet فيها SYN → بداية الاتصال

🔸 **tcp.flags.ack==0**  
مافيش ACK → دي SYN حقيقية مش رد

🔸 **tcp.window_size <= 1024**  
دا يدل إن المسح → **SYN scan**

لأن:

- connect scan (full handshake) → window أكبر من 1024
    
- SYN scan → window صغير لأنه مش هيكمل الاتصال

---

# 🔵 **UDP Scan — شرح سريع**

الـ UDP مختلف عن TCP لأنه:

- **مفيش Handshake** (مفيش SYN/SYN-ACK/ACK)
    
- بالتالي:  
    **الـ Scanner مش بيقدر يعرف بسهولة هل البورت مفتوح ولا مقفول.**
    

---

# ✅ **إيه اللي بيحصل في UDP Scan؟**

## **1) لو البورت مفتوح:**

- هيستقبل الـ UDP packet
    
- وغالباً **مش هيرجع أي رد!**
    

ليه؟  
UDP protocol مفيهوش "acknowledgement" يعني الرد مش إجباري.

---

## **2) لو البورت مقفول:**

الـ Target بيرجع:

### **ICMP Type 3, Code 3**

📌 _Destination Unreachable – Port Unreachable_

وده هو **المؤشر الحقيقي للبورت المقفول**.

---

# 🔥 **إزاي نميّز مين سبب الـ ICMP error؟**

جوه الـ ICMP error packet  
هتلاقي:

- **الـ header الأصلي للـ UDP packet**
    
- **الـ source port و destination port**
    
- **البيانات الأساسية**
    

وده بيقولك الرسالة دي ردّ على أي Packet بالضبط.

علشان كده لازم تفتح:

`ICMP → Encapsulated data`

هتلاقي جوّاه الـ UDP packet الأصلية.

---

# 🔍 **فلتر Wireshark المهم للـ UDP Scan**

الفلتر اللي بيدور على كل البورتات المقفولة:

`icmp.type == 3 and icmp.code == 3`

ده بيجيب **كل الـ Port Unreachable errors**  
اللي معناها:  
**UDP Port Closed**


# ARP Poisoning/Spoofing (A.K.A. Man In The Middle Attack)
|                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Notes**                                                                                                                                                                                                                                           | **Wireshark filter**                                                                                                                                                                                                                                   |
| Global search                                                                                                                                                                                                                                       | - `arp`                                                                                                                                                                                                                                                |
| **"ARP" options for grabbing the low-hanging fruits:**<br><br>- Opcode 1: ARP requests.<br>- Opcode 2: ARP responses.<br>- **Hunt:**Arp scanning<br>- **Hunt:**Possible ARP poisoning detection<br>- **Hunt:**Possible ARP flooding from detection: | - `arp.opcode == 1`<br><br>- `arp.opcode == 2`<br><br>- `arp.dst.hw_mac==00:00:00:00:00:00`<br><br>- `arp.duplicate-address-detected or arp.duplicate-address-frame`<br><br>- `((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)` |

# DHCP Analysis

**DHCP** protocol, or **D**ynamic **H**ost **C**onfiguration **P**rotocol **(DHCP)****,** is the technology responsible for managing automatic IP address and required communication parameters assignment.

**DHCP investigation in a nutshell:**

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                         |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Notes**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | **Wireshark Filter**                                                                                                                                                                                                                    |
| Global search.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | - `dhcp`or`bootp`                                                                                                                                                                                                                       |
| Filtering the proper DHCP packet options is vital to finding an event of interest.   <br>  <br><br>- **"DHCP Request"**packets contain the hostname information<br>- **"DHCP ACK"**packets represent the accepted requests<br>- **"DHCP NAK"**packets represent denied requests<br><br>Due to the nature of the protocol, only "Option 53" ( request type) has predefined static values. You should filter the packet type first, and then you can filter the rest of the options by "applying as column" or use the advanced filters like "contains" and "matches". | - Request: `dhcp.option.dhcp == 3`<br><br>- ACK: `dhcp.option.dhcp == 5`<br><br>- NAK: `dhcp.option.dhcp == 6`                                                                                                                          |
| **"DHCP Request"**options for grabbing the low-hanging fruits:<br><br>- **Option 12:**Hostname.<br>- **Option 50:**Requested IP address.<br>- **Option 51:**Requested IP lease time.<br>- **Option 61:**Client's MAC address.                                                                                                                                                                                                                                                                                                                                        | - `dhcp.option.hostname contains "keyword"`                                                                                                                                                                                             |
| **"DHCP ACK"**options for grabbing the low-hanging fruits:<br><br>- **Option 15:**Domain name.<br>- **Option 51:**Assigned IP lease time.                                                                                                                                                                                                                                                                                                                                                                                                                            | - `dhcp.option.domain_name contains "keyword"`                                                                                                                                                                                          |
| **"DHCP NAK"**options for grabbing the low-hanging fruits:<br><br>- **Option 56:**Message (rejection details/reason).                                                                                                                                                                                                                                                                                                                                                                                                                                                | As the message could be unique according to the case/situation, It is suggested to read the message instead of filtering it. Thus, the analyst could create a more reliable hypothesis/result by understanding the event circumstances. |
## ICMP Analysis

Internet Control Message Protocol (ICMP) is designed for diagnosing and reporting network communication issues. It is highly used in error reporting and testing. As it is a trusted network layer protocol, sometimes it is used for denial of service (DoS) attacks; also, adversaries use it in data exfiltration and C2 tunnelling activities.

**ICMP analysis in a nutshell:**

Usually, ICMP tunnelling attacks are anomalies appearing/starting after a malware execution or vulnerability exploitation. As the ICMP packets can transfer an additional data payload, adversaries use this section to exfiltrate data and establish a C2 connection. It could be a TCP, HTTP or SSH data. As the ICMP protocols provide a great opportunity to carry extra data, it also has disadvantages. Most enterprise networks block custom packets or require administrator privileges to create custom ICMP packets.

A large volume of ICMP traffic or anomalous packet sizes are indicators of ICMP tunnelling. Still, the adversaries could create custom packets that match the regular ICMP packet size (64 bytes), so it is still cumbersome to detect these tunnelling activities. However, a security analyst should know the normal and the abnormal to spot the possible anomaly and escalate it for further analysis.

|                                                                                                                                                                    |                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| **Notes**                                                                                                                                                          | **Wireshark filters**      |
| Global search                                                                                                                                                      | - `icmp`                   |
| **"ICMP" options for grabbing the low-hanging fruits:**<br><br>- Packet length.<br>- ICMP destination addresses.<br>- Encapsulated protocol signs in ICMP payload. | - `data.len > 64 and icmp` |
|                                                                                                                                                                    |                            |
## DNS Analysis

Domain Name System (DNS) is designed to translate/convert IP domain addresses to IP addresses. It is also known as a phonebook of the internet. As it is the essential part of web services, it is commonly used and trusted, and therefore often ignored. Due to that, adversaries use it in data exfiltration and C2 activities.

**DNS analysis in a nutshell:**

Similar to ICMP tunnels, DNS attacks are anomalies appearing/starting after a malware execution or vulnerability exploitation. Adversary creates (or already has) a domain address and configures it as a C2 channel. The malware or the commands executed after exploitation sends DNS queries to the C2 server. However, these queries are longer than default DNS queries and crafted for subdomain addresses. Unfortunately, these subdomain addresses are not actual addresses; they are encoded commands as shown below:

**"encoded-commands.maliciousdomain.com"**

When this query is routed to the C2 server, the server sends the actual malicious commands to the host. As the DNS queries are a natural part of the networking activity, these packets have the chance of not being detected by network perimeters. A security analyst should know how to investigate the DNS packet lengths and target addresses to spot these anomalies.

|   |   |
|---|---|
|**Notes**|**Wireshark Filter**|
|Global search|- `dns`|
|**"DNS" options for grabbing the low-hanging fruits:**<br><br>- Query length.<br>- Anomalous and non-regular names in DNS addresses.<br>- Long DNS addresses with encoded subdomain addresses.<br>- Known patterns like dnscat and dns2tcp.<br>- Statistical analysis like the anomalous volume of DNS requests for a particular target.<br><br>**!mdns:** Disable local link device queries.|- `dns contains "dnscat"`<br><br>- `dns.qry.name.len > 15 and !mdns`|

## FTP Analysis

File Transfer Protocol (FTP) is designed to transfer files with ease, so it focuses on simplicity rather than security. As a result of this, using this protocol in unsecured environments could create security issues like:

- MITM attacks
- Credential stealing and unauthorised access
- Phishing
- Malware planting
- Data exfiltration

**FTP analysis in a nutshell:**

|                                                                                                                                                                                                                                                         |                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Notes**                                                                                                                                                                                                                                               | **Wireshark Filter**                                                                                                                                                                          |
| Global search                                                                                                                                                                                                                                           | - `ftp`                                                                                                                                                                                       |
| **"FTP"** options for grabbing the low-hanging fruits:<br><br>- **x1x series:** Information request responses.<br>- **x2x series:** Connection messages.<br>- **x3x series:** Authentication messages.<br><br>**Note:** "200" means command successful. | **---**                                                                                                                                                                                       |
| **"x1x" series options for grabbing the low-hanging fruits:**<br><br>- **211:** System status.<br>- **212:** Directory status.<br>- **213:** File status                                                                                                | - `ftp.response.code == 211`                                                                                                                                                                  |
| **"x2x" series options for grabbing the low-hanging fruits:**<br><br>- **220:** Service ready.<br>- **227:** Entering passive mode.<br>- **228:** Long passive mode.<br>- **229:** Extended passive mode.                                               | - `ftp.response.code == 227`                                                                                                                                                                  |
| **"x3x" series options for grabbing the low-hanging fruits:**<br><br>- **230:** User login.<br>- **231:** User logout.<br>- **331:** Valid username.<br>- **430:** Invalid username or password<br>- **530:** No login, invalid password.               | - `ftp.response.code == 230`                                                                                                                                                                  |
| **"FTP" commands for grabbing the low-hanging fruits:**<br><br>- **USER:** Username.<br>- **PASS:** Password.<br>- **CWD:** Current work directory.<br>- **LIST:** List.                                                                                | - `ftp.request.command == "USER"`<br><br>- `ftp.request.command == "PASS"`<br><br>- `ftp.request.arg == "password"`                                                                           |
| Advanced usages examples for grabbing low-hanging fruits:<br><br>- **Bruteforce signal:** List failed login attempts.<br>- **Bruteforce signal:** List target username.<br>- **Password spray signal:** List targets for a static password.             | - `ftp.response.code == 530`<br><br>- `(ftp.response.code == 530) and (ftp.response.arg contains "username")`<br><br>- `(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")` |


لو عندك ملف الsslkey.log ممكن تعمل فولدر جديد في ال C وتسميه temp وتروح وتضيف المسار ده باسم  SSKEYLOGS في edit environment variables for your account وبعد كده تفتح ال wireshark وتخش على  edit --> prefrence ---> protcols --> TLS وتضيف الملف اللي قلنا عليه sslkey.log 


# ✅ **ما معنى Decrypting HTTPS Traffic؟**

عند تصفح الإنترنت باستخدام HTTPS، يتم **تشفير** كل البيانات بين المتصفح والسيرفر باستخدام بروتوكول **TLS**.  
وبالتالي عند التقاط الترافيك في Wireshark، لن تستطيع رؤية الـ URL أو بيانات POST/GET أو محتوى الصفحات، لأنها ستكون مشفرة بالكامل.

لكي تُفك هذا التشفير، تحتاج إلى **مفاتيح الجلسة (Session Keys)**، وهي مفاتيح خاصة لكل اتصال HTTPS.

---

# 🔐 **لماذا نحتاج إلى فك تشفير HTTPS؟**

كمحلل أمن أو DFIR Investigator، تحتاج أحيانًا لمعرفة:

- هل الموقع الذي تم زيارته ضار؟
    
- هل تم إرسال بيانات حساسة؟
    
- هل هناك اتصال مشبوه Malware → C2؟
    
- هل تم تنزيل ملفات خبيثة؟
    

ولا يمكنك معرفة هذا طالما البيانات مشفرة.

---

# ⭐ **كيف تفك تشفير HTTPS في Wireshark؟**

الطريقة الأساسية: **SSLKEYLOGFILE**

وهي عبارة عن ملف نصي يحتوي على مفاتيح الجلسة التي يستخدمها المتصفح في تشفير/فك التشفير.

## ✨ **المتصفحات التي تدعم ذلك:**

✔ Chrome  
✔ Firefox  
❌ Edge (لا يدعم مباشرة)

---

# 🧰 **خطوات استخراج مفاتيح التشفير (SSLKEYLOGFILE):**

## **1️⃣ إنشاء ملف لتخزين المفاتيح**

على Windows:

1. أنشئ ملف فارغ وليكن:
    

`C:\sslkeys\sslkeylog.log`

2. اضبط متغير البيئة:
    

من Start → ابحث عن "Environment Variables" →  
System variables → New

`Variable name: SSLKEYLOGFILE Variable value: C:\sslkeys\sslkeylog.log`

3. **أعد تشغيل المتصفح**.
    

من الآن، كل اتصال HTTPS سيتم تخزين مفاتيحه في هذا الملف.

---

# **2️⃣ إضافة الملف إلى Wireshark**

افتح Wireshark →  
Edit → Preferences → Protocols → TLS →

ضع المسار في:  
**(Pre)-Master-Secret log filename**

أو:

Right-click on packet → Protocol Preferences → TLS → Add Key Log File

---

# **3️⃣ افتح الـ PCAP مرّة أخرى → سترى أن الترافيك اتفك تشفيره**

ستظهر لك:

- الـ URL كاملة
    
- الـ Headers
    
- الـ HTTP2 Streams
    
- محتوى الصفحات
    
- POST / GET data
    
- Cookies
    
- JSON / HTML Responses
    
- الملفات التي تم تحميلها
    

---

# 🔍 **فهم TLS Handshake في Wireshark**

## **Client Hello**

يظهر عبر الفلتر:

`tls.handshake.type == 1`

هنا العميل يرسل:

- البروتوكولات التي يدعمها
    
- cipher suites
    
- SNI (اسم الدومين) ← مهم جدًا للتحقيق
    

## **Server Hello**

الفلتر:

`tls.handshake.type == 2`

السيرفر يرسل:

- اختيار cipher
    
- الشهادة الرقمية
    
- بداية التشفير
    

---

# 🎨 **فلاتر مهمة للتحقيق في HTTPS**

### **عرض كل طلبات HTTP بعد فك التشفير:**

`http.request`

### **بحث عام:**

`tls`

### **Client Hello فقط:**

`tls.handshake.type == 1`

### **Server Hello فقط:**

`tls.handshake.type == 2`

### **استثناء SSDP (ضوضاء الشبكة):**

`!(ssdp)`

---

# 📌 **قبل إضافة Key Log File**

سترى:

- بيانات Encrypted Application Data
    
- لا يوجد URLs
    
- لا يوجد HTTP responses
    
- لا يمكن رؤية المحتوى
    

## **بعد إضافة Key Log File**

ستظهر:

- GET / POST requests
    
- HTML pages
    
- JSON responses
    
- Cookies
    
- Downloaded files
    
- Decompressed headers
    

---

# 🧠 **Data Formats بعد فك التشفير:**

Wireshark يعرض البيانات في:

- **Frame**
    
- **Decrypted TLS**
    
- **Decompressed Header**
    
- **Reassembled TCP**
    
- **Reassembled SSL**
    

مفيدة جدًا لتحليل البرمجيات الخبيثة وC2 communications.


Task 8 > 
1-What is the frame number of the "Client Hello" message sent to "accounts.google.com"?

tls.handshake.type == 1 and tls.handshake.extentions_server_name contanis "accounts.google.com"

2-Decrypt the traffic with the "KeysLogFile.txt" file. What is the number of HTTP2 packets?

after add KeysLogFile.txt filter with > http2

Go to Frame 322. What is the authority header of the HTTP2 packet? (Enter the address in defanged format.)