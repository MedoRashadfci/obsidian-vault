
# 🛡️ What Is an IDS (Intrusion Detection System)?

**IDS = نظام كشف التسلل**

هو نظام أمني:

- يراقب حركة الشبكة أو الأجهزة
    
- يكتشف الأنشطة المشبوهة
    
- يولّد Alert
    
- لكنه لا يمنع الهجوم (Detection Only)
    

---

## 🔥 الفرق بين Firewall و IDS

|Firewall|IDS|
|---|---|
|يتحكم في الدخول والخروج|يراقب ما يحدث داخل الشبكة|
|يمنع الاتصال|لا يمنع — فقط ينبه|
|يعمل عند الحدود|يعمل داخل الشبكة|
|Rule-based|Signature + Anomaly|

---

## 🏢 مثال تشبيهي (مهم تحفظه)

- **Firewall** = حارس بوابة المبنى
    
- **IDS** = كاميرات المراقبة داخل المبنى
    

لو مهاجم عدى البوابة:

- الكاميرات تكتشفه
    
- ترسل تنبيه للإدارة
    

لكنها لا تمسكه بنفسها.

---

# 🧠 أنواع IDS

## 1️⃣ Network-Based IDS (NIDS)

يراقب حركة الشبكة كاملة.

مثال:

- 🔎 **Snort**
    
- 🔎 **Suricata**
    

📌 يوضع عادة خلف الـ Firewall أو على SPAN Port

---

## 2️⃣ Host-Based IDS (HIDS)

يراقب جهاز معين (Server أو Endpoint)

مثال:

- 🔎 **OSSEC**
    

يراقب:

- File changes
    
- Logins
    
- Process execution
    
- Registry changes
    

---

# 🧬 طرق الكشف (Detection Methods)

## 🟢 1. Signature-Based Detection

يعتمد على:

- قواعد معروفة
    
- أنماط هجوم محفوظة
    

زي Antivirus بالظبط.

📌 المشكلة: لا يكتشف Zero-Day.

---

## 🟢 2. Anomaly-Based Detection

يعتمد على:

- Baseline behavior
    
- يكتشف أي سلوك غير طبيعي
    

مثال:

- جهاز عادة يرسل 100 طلب في الدقيقة
    
- فجأة يرسل 10,000 → Alert
    

📌 ميزته:

- يكتشف هجمات جديدة
    

📌 عيبه:

- ممكن يسبب False Positives
    

---

# 🔎 كيف يعمل IDS عمليًا؟

1️⃣ يلتقط traffic  
2️⃣ يحلل الـ packets  
3️⃣ يقارنها بالقواعد  
4️⃣ يطلق Alert  
5️⃣ SOC Analyst يتدخل

---

# 🐷 أشهر IDS مفتوح المصدر: Snort

## 🔥 ما هو Snort؟

**Snort** هو:

- Open-source IDS
    
- يستخدم Rules
    
- يعمل كـ NIDS
    
- يمكن تحويله إلى IPS
    

---

## ⚙️ كيف يعمل Snort؟

Snort يحتوي على:

1. Packet Decoder
    
2. Preprocessor
    
3. Detection Engine (Rules)
    
4. Alerting / Logging
    

---

# 📝 أنواع Rules في Snort

## 1️⃣ Default Rules

موجودة مسبقًا

- SQL Injection
    
- XSS
    
- Port Scan
    
- Exploits
    

---

## 2️⃣ Custom Rules

يمكنك كتابة Rule خاصة بك

مثال:

`alert tcp any any -> any 80 (msg:"Possible HTTP Attack"; content:"/admin"; sid:1000001;)`

ده يولد Alert لو حد طلب `/admin`.

---

# 🎯 ماذا ستتعلم في هذا المسار؟

✔️ أنواع IDS  
✔️ كيفية عمل Snort  
✔️ قراءة القواعد الافتراضية  
✔️ كتابة Custom Rule  
✔️ تحليل Alerts

---

# ⚡ الفرق بين IDS و IPS (معلومة إضافية مهمة)

| IDS        | IPS            |
| ---------- | -------------- |
| Detect فقط | Detect + Block |
| Passive    | Active         |
| يولد Alert | يسقط الباكيت   |


# 🛡️ Types of IDS

تصنيف الـ IDS يعتمد على عاملين أساسيين:

1️⃣ **Deployment Mode (مكان التركيب)**  
2️⃣ **Detection Mode (طريقة الكشف)**

---

# أولاً: Deployment Modes

## 🖥️ 1️⃣ Host Intrusion Detection System (HIDS)

### 📌 الفكرة

يتم تثبيته على جهاز معين (Server / Endpoint) ويراقب هذا الجهاز فقط.

### 🔍 ماذا يراقب؟

- تغييرات الملفات (File Integrity)
    
- عمليات التشغيل (Processes)
    
- تسجيل الدخول
    
- سجلات النظام (Logs)
    
- Registry changes (في Windows)
    

### 🧰 أمثلة

- 🔎 **OSSEC**
    
- 🔎 **Wazuh**
    

### ✅ المميزات

- رؤية تفصيلية جدًا للجهاز
    
- يكتشف insider threats
    
- يراقب system-level attacks
    

### ❌ العيوب

- يحتاج تثبيت على كل جهاز
    
- Resource-intensive
    
- صعب الإدارة في شبكات كبيرة
    

---

## 🌐 2️⃣ Network Intrusion Detection System (NIDS)

### 📌 الفكرة

يراقب الترافيك الخاص بكل الشبكة من نقطة مركزية.

عادة يتم تركيبه:

- خلف Firewall
    
- على SPAN / Mirror Port
    

### 🧰 أمثلة

- 🔎 **Snort**
    
- 🔎 **Suricata**
    
- 🔎 **Zeek**
    

### ✅ المميزات

- رؤية شاملة لكل الشبكة
    
- إدارة مركزية
    
- لا يحتاج تثبيت على كل جهاز
    

### ❌ العيوب

- لا يرى أنشطة داخل الجهاز نفسه
    
- لا يفحص الترافيك المشفر إلا بآليات إضافية
    

---

## 🔎 الفرق بين NIDS و HIDS

|المقارنة|HIDS|NIDS|
|---|---|---|
|مكان التثبيت|على الجهاز|داخل الشبكة|
|نطاق الرؤية|جهاز واحد|كل الشبكة|
|تحليل الملفات|نعم|لا|
|تحليل الترافيك|محدود|شامل|
|الإدارة|موزعة|مركزية|

---

# ثانيًا: Detection Modes

---

## 🟢 1️⃣ Signature-Based IDS

### 📌 كيف يعمل؟

يعتمد على قاعدة بيانات تحتوي على:

- أنماط هجمات معروفة (Signatures)
    

لو الترافيك يطابق توقيع معروف → Alert.

### 🧠 مثال

لو Packet تحتوي على:

`' OR 1=1 --`

→ يكتشف SQL Injection.

### 🧰 مثال عملي

- 🔎 **Snort**
    

### ✅ المميزات

- سريع
    
- دقيق في الهجمات المعروفة
    
- Low False Positives
    

### ❌ العيوب

- لا يكتشف Zero-Day
    
- يعتمد على تحديث القواعد باستمرار
    

---

## 🟡 2️⃣ Anomaly-Based IDS

### 📌 كيف يعمل؟

1️⃣ يبني Baseline (سلوك طبيعي)  
2️⃣ يقارن الترافيك الحالي بالسلوك الطبيعي  
3️⃣ أي انحراف → Alert

### 🧠 مثال

جهاز عادة:

- يرسل 100 طلب/دقيقة
    

فجأة:

- يرسل 10,000 طلب → ممكن يكون DDoS أو Malware
    

### ✅ المميزات

- يكتشف Zero-Day
    
- يكتشف سلوك غير طبيعي
    

### ❌ العيوب

- False Positives عالية
    
- يحتاج Fine-Tuning
    

---

## 🔵 3️⃣ Hybrid IDS

### 📌 الفكرة

يجمع بين:

- Signature-Based
    
- Anomaly-Based
    

### 🎯 كيف يعمل؟

- لو التهديد معروف → يستخدم Signature
    
- لو تهديد جديد → يستخدم Anomaly Detection
    

### ✅ أفضل توازن بين:

- سرعة الكشف
    
- واكتشاف الهجمات الجديدة
    

---

# ⚖️ مقارنة سريعة بين Detection Modes

|النوع|يكتشف Known Attacks|يكتشف Zero-Day|False Positives|الأداء|
|---|---|---|---|---|
|Signature|✅ نعم|❌ لا|قليلة|سريع|
|Anomaly|✅ نعم|✅ نعم|عالية|أثقل|
|Hybrid|✅ نعم|✅ نعم|متوسطة|متوازن|

---

# 🎯 اختيار النوع المناسب يعتمد على:

- حجم الشبكة
    
- حساسية البيانات
    
- Threat surface
    
- Budget
    
- خبرة الفريق


# 🐷 IDS Example: Snort

## ما هو Snort؟

![https://miro.medium.com/1%2AgSBTurQqZ6IfCw9Q1_UdYQ.jpeg](https://miro.medium.com/1%2AgSBTurQqZ6IfCw9Q1_UdYQ.jpeg)

![https://www.researchgate.net/publication/325851146/figure/fig3/AS%3A733938725158912%401551996036502/Proposed-Snort-IDS-Architecture-with-Snort-Intelligent-Plug-in.png](https://www.researchgate.net/publication/325851146/figure/fig3/AS%3A733938725158912%401551996036502/Proposed-Snort-IDS-Architecture-with-Snort-Intelligent-Plug-in.png)

![https://www.researchgate.net/publication/322192252/figure/fig4/AS%3A578066247761928%401514833141472/Snort-rule-ICMP-alert-test.png](https://www.researchgate.net/publication/322192252/figure/fig4/AS%3A578066247761928%401514833141472/Snort-rule-ICMP-alert-test.png)

4

**Snort** هو:

- Open-source IDS
    
- ظهر سنة 1998
    
- يعتمد أساسًا على Signature-Based detection
    
- يمكنه دعم Anomaly detection عبر Preprocessors
    
- يستخدم Rule Files لاكتشاف الهجمات
    

---

# ⚙️ كيف يعمل Snort؟

Snort يمر بهذه المراحل:

1️⃣ Packet Decoder  
2️⃣ Preprocessors  
3️⃣ Detection Engine (Rules)  
4️⃣ Alerting & Logging

لو Packet طابقت Rule → يولد Alert.

---

# 📂 Built-in Rules vs Custom Rules

## 🟢 Built-in Rules

- تأتي جاهزة مع Snort
    
- تحتوي على توقيعات هجمات معروفة:
    
    - SQL Injection
        
    - XSS
        
    - Port Scanning
        
    - Malware C2
        

## 🟡 Custom Rules

تقدر تكتب Rule خاصة بك حسب بيئتك.

مثال:

alert tcp any any -> any 80 (msg:"Detect admin page access"; content:"/admin"; sid:1000001;)

---

# 🧭 Modes of Snort

Snort له 3 أوضاع تشغيل أساسية:

---

## 1️⃣ Packet Sniffer Mode

### ماذا يفعل؟

- يقرأ الترافيك
    
- يعرض الباكيتس فقط
    
- لا يحلل ولا يطبق Rules
    

### مثال استخدام

فريق الشبكات يريد تشخيص مشكلة Latency.

### تشغيله:

snort -v

📌 يشبه tcpdump لكن عبر Snort.

---

## 2️⃣ Packet Logging Mode

### ماذا يفعل؟

- يسجل الترافيك في ملفات
    
- يمكن حفظه بصيغة PCAP
    
- مفيد للتحليل الجنائي
    

### مثال استخدام

الـ Incident Response Team يريد تحليل هجوم سابق.

### تشغيله:

snort -dev -l /var/log/snort

📌 مفيد جدًا في Digital Forensics.

---

## 3️⃣ NIDS Mode (الأهم 🔥)

### ماذا يفعل؟

- يراقب الترافيك لحظيًا
    
- يطبق Rules
    
- يولد Alerts عند التطابق
    

### مثال استخدام

SOC Team يراقب الشبكة 24/7.

### تشغيله:

snort -c /etc/snort/snort.conf -i eth0

---

# 📊 الفرق بين Modes

|Mode|هل يطبق Rules؟|هل يولد Alerts؟|الاستخدام|
|---|---|---|---|
|Sniffer|❌ لا|❌ لا|Troubleshooting|
|Logging|❌ لا (يسجل فقط)|❌ لا|Forensics|
|NIDS|✅ نعم|✅ نعم|Security Monitoring|

---

# 🎯 متى أستخدم كل Mode؟

- عايز تشوف الترافيك بس → Sniffer Mode
    
- عايز تحفظ الترافيك للتحليل → Logging Mode
    
- عايز تكتشف هجمات → NIDS Mode



# 🐷 Snort Usage – عملي خطوة بخطوة

## 1️⃣ Promiscuous Mode (مهم جدًا)

بشكل افتراضي:

- كارت الشبكة يلتقط الترافيك الموجّه لجهازك فقط.
    

لو عايز تستخدم Snort كـ NIDS لمراقبة الشبكة كلها:

- لازم تفعل **Promiscuous Mode**
    

مثال:

sudo ip link set eth0 promisc on

ده يخلي الكارت يقرأ كل الباكيتس اللي بتمر في الشبكة.

---

# 📂 مكان ملفات Snort

كل ملفات Snort الأساسية موجودة في:

/etc/snort

اعرضها:

ls /etc/snort

### أهم الملفات:

|الملف|وظيفته|
|---|---|
|snort.conf|ملف الإعدادات الرئيسي|
|rules/|مجلد القواعد|
|local.rules|ملف القواعد المخصصة|
|classification.config|تصنيف الهجمات|

---

# 🧠 فهم صيغة Rule في Snort

مثال Rule:

alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)

## مكونات الـ Rule

|الجزء|المعنى|
|---|---|
|alert|الإجراء|
|icmp|البروتوكول|
|any any|المصدر (IP + Port)|
|->|اتجاه الترافيك|
|$HOME_NET|شبكة الهدف|
|any|بورت الهدف|
|msg|رسالة التنبيه|
|sid|رقم تعريف القاعدة|
|rev|رقم الإصدار|

---

# ✍️ إنشاء Rule مخصصة

افتح ملف القواعد المخصصة:

sudo nano /etc/snort/rules/local.rules

أضف هذه القاعدة:

alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)

احفظ التغييرات.

---

# 🚀 تشغيل Snort في وضع الكشف (NIDS Mode)

sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf

### شرح الخيارات:

|الخيار|وظيفته|
|---|---|
|-q|Quiet mode|
|-l|مسار حفظ اللوج|
|-i|واجهة الشبكة|
|-A console|عرض التنبيهات على الشاشة|
|-c|ملف الإعدادات|

---

# 🧪 اختبار القاعدة

نفّذ:

ping 127.0.0.1

لو القاعدة شغالة صح هتشوف:

Loopback Ping Detected

🎯 ده معناه إن Rule شغالة بنجاح.

---

# 📁 تحليل ملفات PCAP باستخدام Snort

مهم جدًا في Digital Forensics 🔥

لو عندك ملف PCAP:

sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf

### الفرق هنا:

- استخدمنا `-r` بدل `-i`
    
- بيحلل ملف محفوظ بدل real-time traffic
    

---

# 🛠️ الفرق بين Real-Time و PCAP Analysis

|Real-Time|PCAP|
|---|---|
|-i interface|-r file.pcap|
|كشف مباشر|تحليل جنائي|
|Monitoring|Investigation|

---

# 🎯 سيناريوهات عملية

| الحالة             | ماذا تستخدم؟ |
| ------------------ | ------------ |
| مراقبة الشبكة الآن | NIDS Mode    |
| تحليل هجوم سابق    | PCAP Mode    |
| تشخيص مشكلة شبكة   | Sniffer Mode |