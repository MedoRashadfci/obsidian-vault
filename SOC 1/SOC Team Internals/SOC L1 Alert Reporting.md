
## Alert Flow in SOC

في البداية، **L1 Analyst** هو أول شخص يستقبل الـ **Alerts** من أنظمة زي **SIEM**, **EDR** أو **Ticket Management Platform**.  
معظم الـ Alerts بتكون **False Positives** وبتتقفل على مستوى **L1**، لكن الـ Alerts الخطيرة أو المعقدة بتتصنف كـ **True Positives** وبتحتاج تتحول لمستوى أعلى.

عادةً من كل عدد كبير من الـ Alerts، نسبة صغيرة فقط بتوصل لـ **L2 Analyst**، وأقل منهم اللي بيتحول لـ **Incident** ويحتاج **DFIR**.

---

## 1️⃣ Alert Reporting

**Alert Reporting** هو توثيق كل اللي عملته أثناء تحليل الـ Alert.  
مش مجرد Comment صغير، لكن تقرير كامل يشمل:

- خطوات التحليل
    
- الأدلة (Logs, Events, Screenshots)
    
- القرار النهائي (**True Positive / False Positive**)
    
- سبب القرار
    

ده مهم جدًا خصوصًا لو الـ Alert **True Positive** لأن التقرير ده هو اللي هيتبعت مع الـ Escalation.

📌 ينفع تحطه في الريپورت كده:

> The alert investigation was documented through Alert Reporting, including analysis steps, evidence, and final classification.

---

## 2️⃣ Alert Escalation

**Alert Escalation** بتحصل لما الـ **True Positive** يحتاج تحليل أعمق أو إجراءات علاج (**Remediation**).  
ساعتها الـ **L1 Analyst** بيحوّل الـ Alert لـ **L2 Analyst** حسب الـ **Escalation Procedures** المتفق عليها في الفريق.

الـ **Alert Report** هنا بيكون مهم جدًا، لأنه:

- بيدي الـ L2 Context جاهز
    
- بيقلل الوقت اللي هيبدأ فيه من الصفر
    
- بيساعد في اتخاذ قرار أسرع
    

📌 جملة مناسبة للريپورت:

> The True Positive alert was escalated to L2 following the escalation procedures with a detailed alert report.

---

## 3️⃣ Communication

**Communication** جزء أساسي في شغل الـ SOC.  
أحيانًا التحليل لوحده مش كفاية، وبتحتاج تتواصل مع فرق تانية زي:

- **IT Team** للتأكد من تغييرات حصلت فعلًا (زي Admin Privileges)
    
- **HR Department** لمعرفة معلومات عن موظف جديد
    
- **Management** في الحالات الحرجة
    

التواصل ده بيمنع **False Assumptions** وبيساعد في اتخاذ قرار صح.

📌 مثال للريپورت:

> Communication with the IT team was conducted to validate administrative access changes.

---

## مصطلحات مهمة للريبورت

- **Alert**
    
- **False Positive**
    
- **True Positive**
    
- **L1 Analyst**
    
- **L2 Analyst**
    
- **Incident**
    
- **DFIR**
    
- **SIEM**
    
- **EDR**
    
- **Alert Reporting**
    
- **Alert Escalation**
    
- **Communication**
    
- **Remediation**


## Reporting Guide – ليه L1 Analyst لازم يكتب Report؟

قبل ما نكمّل، مهم نفهم **ليه كتابة Report من L1 Analyst حاجة أساسية جدًا** ومش مجرد خطوة شكلية بعد تصنيف الـ Alert كـ **True Positive** أو **False Positive**.  
الموضوع ده لا يمكن الاستهانة بيه، لأن **Alert Report** بيأثر بشكل مباشر على سرعة الاستجابة وجودة التحقيق الأمني.

---

## Purpose of Alert Reporting

### 1️⃣ Provide Context for Escalation

كتابة **Alert Report** بشكل كويس بتوفّر **Context** واضح لأي حد هيستلم الـ Alert بعدك، خصوصًا **L2 Analyst**.

- بدل ما L2 يبدأ التحقيق من الصفر
    
- التقرير الجيد بيوفّر عليه وقت كبير
    
- وبيخلّيه يفهم بسرعة:
    
    - إيه اللي حصل
        
    - إزاي حصل
        
    - وإنت وصلت لإيه
        

📌 بمعنى تاني:  
**Good reporting = Faster escalation + Faster response**

---

### 2️⃣ Save Findings for the Records

في أغلب الأنظمة:

- **Raw SIEM logs** بتتخزن فترة محدودة (من **3 إلى 12 شهر**)
    
- لكن **Alerts** نفسها بتتخزن **indefinitely**
    

علشان كده:

- لو اعتمدت بس على الـ logs
    
- ممكن بعد فترة الأدلة تختفي
    
- لكن الـ Alert Report يفضل موجود
    

📌 عشان كده:

> Keeping all investigation context inside the alert is critical for future reference.

وده مهم جدًا في:

- **Audits**
    
- **Incident reviews**
    
- **Legal investigations**
    

---

### 3️⃣ Improve Investigation Skills

في مقولة مشهورة جدًا:

> _If you can't explain it simply, you don't understand it well enough._

كتابة **Alert Report**:

- بتجبرك تفهم اللي حصل فعلًا
    
- تلخّص الـ Alert بشكل منطقي
    
- تربط الأحداث ببعض
    

وده:

- بيطوّر مهارات **L1 Analyst**
    
- وبيخليك أحسن في:
    
    - Analysis
        
    - Correlation
        
    - Decision making
        

📌 كتابة التقرير نفسها تعتبر **Training عملي**.

---

## Report Format – How to Write a Good Alert Report

علشان تكتب **Structured Report** وسهل الفهم، يُنصح باستخدام أسلوب **5Ws Approach**.

تخيّل نفسك:

- **L2 Analyst**
    
- عضو في **DFIR Team**
    
- أو شخص من **IT Department**
    

وسأل نفسك:

> أنا محتاج أشوف إيه علشان أفهم الـ Alert بسرعة؟

---

## The Five Ws (5Ws Approach)

### 1️⃣ Who

**Who** قام بالنشاط المشبوه؟

- Which **user**
    
- Which **account**
    
- Which **process**
    

📌 مثال:

> The alert was associated with user `john.doe` who initiated the activity.

---

### 2️⃣ What

**What** اللي حصل بالضبط؟

- What action was performed
    
- What command was executed
    
- What file was downloaded
    
- What event sequence triggered the alert
    

📌 مثال:

> The user executed a PowerShell command that downloaded an external file.

---

### 3️⃣ When

**When** النشاط المشبوه بدأ وانتهى؟

- Start time
    
- End time
    
- Time zone لو مهم
    

📌 مثال:

> The suspicious activity started at 10:32 AM and ended at 10:35 AM UTC.

---

### 4️⃣ Where

**Where** النشاط حصل فين؟

- Which **device**
    
- Which **hostname**
    
- Which **IP address**
    
- Which **URL / website**
    

📌 مثال:

> The activity originated from endpoint `WIN-01` with IP address `192.168.1.10`.

---

### 5️⃣ Why (Most Important)

**Why** اعتبرت الـ Alert **True Positive** أو **False Positive**؟

ده أهم جزء في التقرير:

- reasoning
    
- logic
    
- evidence-based decision
    

📌 مثال:

> The alert was classified as a True Positive due to confirmed malicious behavior and lack of legitimate business justification.

---

## Why the "Why" Matters Most

- هو اللي يبرّر قرارك
    
- هو اللي L2 هيبني عليه خطواته
    
- وهو اللي يثبت إن التحليل كان منطقي مش عشوائي
    

📌 من غير **Why**:

> The report is incomplete.

---
## Escalation Guide (دليل التصعيد)

بعد ما تعمل **Verdict** (تصنيف التنبيه) وتكتب **Alert Report**، لازم تاخد قرار مهم:  
هل التنبيه ده محتاج **Escalation to L2** ولا لأ؟

الإجابة بتختلف من SOC للتاني، لكن القواعد الجاية تعتبر **Best Practice** في أغلب فرق الـSOC.

---

### ✅ متى تقوم بـ Escalation؟

لازم تصعّد التنبيه إلى **L2 Analyst** في الحالات دي:

1. **Major Cyberattack Indicator**  
    لو التنبيه بيشير لهجوم كبير محتاج تحليل أعمق أو تدخل فريق **DFIR (Digital Forensics and Incident Response)**.
    
2. **Remediation Required**  
    لو محتاج إجراءات علاجية زي:
    
    - Malware Removal
        
    - Host Isolation
        
    - Password Reset
        
3. **External or Internal Communication Needed**  
    لو الموضوع محتاج تواصل مع:
    
    - Customers
        
    - Partners
        
    - Management
        
    - Law Enforcement
        
4. **Lack of Understanding**  
    لو كـ **L1 Analyst** مش فاهم التنبيه بالكامل، وده طبيعي خصوصًا في البداية، الأفضل تطلب مساعدة من **Senior Analyst** بدل ما تقفله غلط.
    

---

## 🔁 Escalation Steps (خطوات التصعيد)

في أغلب الفرق، عملية التصعيد بسيطة وتشمل:

1. **Reassign the alert**  
    تعيد تعيين التنبيه إلى **L2 on shift**.
    
2. **Notify L2**  
    تعمل Ping لـ L2:
    
    - Corporate Chat
        
    - أو In person
        

🔹 بعض الفرق الكبيرة بتطلب:

- **Formal Escalation Request**
    
- Ticket فيه Fields كتير (لكن ده حسب سياسة الشركة)
    

---

### 📌 Example Scenario

- L1 escalates a **Phishing Alert**
    
- L2 investigates deeper
    
- L2 rotates the user’s credentials (Password Reset)
    

---

## 👥 What Happens After Escalation?

بغض النظر عن الطريقة، اللي بيحصل دايمًا:

1. **L2 receives the ticket**
    
2. يقرأ **Alert Report** بتاعك
    
3. لو في حاجة مش واضحة → يتواصل معاك
    
4. يبدأ:
    
    - Deeper Investigation
        
    - Validate True Positive
        
    - Communicate with other teams
        
    - Start Incident Response (لو Incident كبير)
        

---

## 🆘 Requesting L2 Support (طلب مساعدة من L2)

طلب المساعدة مش ضعف، بالعكس ده **Professional SOC Behaviour**.

خصوصًا في أول شهورك:

- اسأل
    
- ناقش
    
- افهم الـProcess
    

أفضل من إنك:  
❌ تقفل Alert مش فاهمه  
❌ أو تفوّت Incident حقيقي

---

### 🔄 Typical Support Flow

- **L1 asks for help**
    
- **L2 accepts**
    
- **Knowledge Sharing Session** يحصل أثناء التحقيق

## 🔔 Escalation & Reporting – Reality Check

مواضيع **Escalation** و **Reporting** غالبًا بتكون منطقية وسهلة في الشرح، لكن في الواقع العملي داخل الـSOC الموضوع بيكون أعقد شوية.  
دايمًا لازم تكون مستعد لسيناريوهات غير متوقعة وتعرف تتصرف خصوصًا في **Critical Cases**.

في أحسن الأحوال، فريق الـSOC بيكون عنده:

- **Crisis Communication Procedures**  
    وهي Guides وProcesses بتوضح:
    
- مين يتواصل مع مين
    
- وإمتى
    
- وإزاي
    

لو الإجراءات دي مش موجودة، لازم تعتمد على **Best Practices** زي الحالات الجاية.

---

## 📡 Communication Cases (حالات التواصل)

### 1️⃣ Critical Alert و L2 Unavailable

**Scenario**  
عندك **Urgent / Critical Alert**، لكن:

- L2 مش بيرد
    
- معدي 30 دقيقة بدون Response
    

**What to do**

- اعرف مكان **Emergency Contacts**
    
- الترتيب الصحيح:
    
    1. Call L2
        
    2. If no response → Contact L3
        
    3. If still nothing → Contact SOC Manager
        

⛔ ممنوع تسيب **Critical Alert** من غير Action.

---

### 2️⃣ Slack / Teams Account Compromise

**Scenario**  
Alert بيشير إلى **Chat Account Compromise**  
ومحتاج تتأكد من Login Activity مع المستخدم نفسه.

**What NOT to do**

- ❌ متكلمش المستخدم من نفس Slack / Teams  
    (لأن الحساب نفسه ممكن يكون مخترق)
    

**Correct Action**

- Use **Alternative Communication**
    
    - Phone Call
        
    - Corporate Email
        
    - Any secure channel
        

---

### 3️⃣ Alert Flood (عدد Alerts كبير فجأة)

**Scenario**  
وصلك عدد ضخم من Alerts في وقت قصير  
وبعضهم **Critical**

**What to do**

- اعمل **Prioritisation**
    
    - Critical → High → Medium → Low
        
- في نفس الوقت:
    
    - Notify L2 on shift
        
    - بلغّه إن في **Alert Flood**
        

الهدف:

- متفوتش Incident
    
- وما تشتغلش لوحدك في أزمة
    

---

### 4️⃣ Misclassification (غلطت في Verdict)

**Scenario**  
بعد أيام تكتشف إنك:

- صنفت Alert غلط
    
- أو قفلت Alert وكان في **Malicious Activity**
    

**Correct Action**

- Immediately contact L2
    
- Explain:
    
    - Your concerns
        
    - Why you think it was malicious
        

📌 مهم جدًا:  
Threat Actors ممكن يفضلوا **Silent** أسابيع قبل الهجوم الحقيقي.

---

### 5️⃣ SIEM Logs Issue

**Scenario**

- Logs مش:
    
    - Parsed correctly
        
    - Searchable
        

وده معطل **Alert Triage**

**What to do**

- ❌ متعملش Skip للـAlert
    
- Investigate أي حاجة تقدر عليها
    
- Report المشكلة فورًا إلى:
    
    - L2 on shift
        
    - SOC Engineer
        

---

## 🧠 Communication By L2

لما **L1 escalates** Alert مهم زي:

- **Data Leak Alert**
    

**L2 Actions**

- Initiate **DFIR**
    
- Contact:
    
    - Legal Department
        
    - PR Department
        

وده بيبين إن:

- دور L1 = Detection + Escalation
    
- دور L2 = Response + Coordination
    

---

## ✅ Key Takeaway (مهم جدًا)

- Communication مهارة أساسية في الـSOC
    
- Technical Skills لوحدها مش كفاية
    
- أي شك = Escalate
    
- أي مشكلة = Communicate