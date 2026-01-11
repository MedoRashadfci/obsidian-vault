
---

## 🔹 مفهوم الـ Alert وأهميته في SOC

> _“An alert is a core concept for any SOC team…”_

الـ **Alert** هو فكرة أساسية جداً في أي فريق SOC.  
يعني إيه أساسية؟  
يعني شغل الـ SOC كله تقريباً بيدور حوالين الـ Alerts.

طريقة تعاملك مع الـ Alert هي اللي بتحدد:

- هل الاختراق هيتكشف بدري ويتمنع؟
    
- ولا هيتفوت، ويتحول لكارثة أمنية كبيرة؟
    

علشان كده، التعامل الصح مع الـ Alerts هو **قرار مصيري** في الأمن السيبراني.

الغرفة (room) دي موجهة لمستوى **Entry Level**، لكنها **ضرورية جداً** لأي SOC L1 Analyst، لأنها بتشرح:

- مفهوم الـ Alert
    
- دورة حياة الـ Alert (من لحظة حدوث الحدث لحد ما يتقفل)
    
- إزاي الـ Alert يتحل صح
    

---

## 🔹 أهداف التعلم (Learning Objectives)

الغرفة دي هدفها إنك تتعلم الحاجات دي بالتفصيل:

### 1️⃣ فهم مفهوم SOC Alert

يعني:

- إيه هو Alert؟
    
- بيطلع إزاي؟
    
- ليه موجود؟
    

---

### 2️⃣ استكشاف مكونات الـ Alert

زي:

- الحقول (Fields)
    
- الحالات (Statuses)
    
- التصنيف (Classification / Verdict)
    

---

### 3️⃣ تعلم Alert Triage كمحلل L1

يعني:

- إزاي تستلم Alert
    
- إزاي تحلله
    
- إمتى تصعّد
    
- إمتى تقفله
    

---

### 4️⃣ التدريب العملي على Alerts حقيقية

- Alerts شبيهة باللي بتحصل في SOC حقيقي
    
- باستخدام Dashboard حقيقي
    

---

### 5️⃣ الاستعداد لـ SOC Simulator و SAL1

- اللي بيختبروا شغلك كمحلل SOC L1 عملياً
    

---

## 🔹 المتطلبات المسبقة (Prerequisites)

علشان تفهم الغرفة دي كويس، لازم تكون:

- فاهم الهجمات الشائعة على:
    
    - Networks
        
    - Windows
        
    - Linux
        
- فاهم أدوار SOC:
    
    - SOC L1
        
    - SOC L2
        
    - شغل كل واحد فيهم
        

---

## 🔹 SOC Dashboard

تم إعطاؤك وصول لـ **SOC Dashboard** داخل TryHackMe SIEM.

الـ Dashboard ده:

- هو المكان اللي بتشوف فيه Alerts
    
- وبتتعامل معاها
    
- وبتنقلها بين الحالات المختلفة
    

مطلوب منك:

- تفتح الموقع
    
- تتعود على شكله
    
- تشوف الأعمدة والتنقلات
    

---

## 🔹 تخيل نفسك داخل SOC حقيقي

النص بيطلب منك تتخيل سيناريو حقيقي:

أنت:

- SOC Intern
    
- واقف جنب Mentor (L1 أو L2)
    
- شايف شاشة مليانة Alerts
    

تشوف:

- Alerts كتير باسم  
    **Email Marked as Phishing**
    
- شوية Alerts باسم  
    **Unusual Gmail Login Location**
    
- Alert خطير جداً باللون الأحمر:  
    **Unapproved Mimikatz Usage**
    

وهنا الأسئلة الطبيعية:

- يعني إيه Alerts دي؟
    
- جات منين؟
    
- نعمل إيه معاها؟
    

وده اللي باقي الغرفة بيشرحه.

---

## 🔹 من Events إلى Alerts

### أولاً: Event

الـ Event هو أي نشاط طبيعي يحصل على النظام، زي:

- User Login
    
- Process اشتغل
    
- File اتعمله Download
    

---

### ثانياً: Logging

النظام:

- Windows
    
- Firewall
    
- Cloud Provider
    

بيسجل الـ Event في **Logs**.

---

### ثالثاً: Log Shipping

الـ Logs دي:

- بتتجمع
    
- وتتبعت لـ:
    
    - SIEM
        
    - EDR
        

---

### رابعاً: حجم البيانات

SOC ممكن يستقبل:

- ملايين Logs يومياً
    
- من آلاف الأنظمة
    

وأغلب الأحداث:

- طبيعية
    
- مش محتاجة تدخل
    

---

### خامساً: دور الـ Alert

الـ Alert هو:

> Notification بتطلع لما Event أو Sequence of Events يطابق Rule مشبوهة

فايدته:

- ينقذك من مراجعة Logs يدوياً
    
- يخليك تراجع Alerts بس
    

بدل:

- ملايين Logs
    
- تراجع عشرات Alerts
    

---

## 🔹 منصات إدارة الـ Alerts

### 🟦 SIEM

أمثلة:

- Splunk ES
    
- Elastic SIEM
    

الوصف:

- أفضل اختيار لمعظم SOCs
    
- إدارة Alerts قوية ومركزية
    

---

### 🟦 EDR / NDR

أمثلة:

- Microsoft Defender
    
- CrowdStrike
    

الوصف:

- عندها Dashboards خاصة بيها
    
- لكن يفضل تجميع Alerts في SIEM أو SOAR
    

---

### 🟦 SOAR

أمثلة:

- Splunk SOAR
    
- Cortex SOAR
    

الوصف:

- SOCs الكبيرة
    
- تجميع Alerts من مصادر متعددة
    
- أتمتة الردود
    

---

### 🟦 ITSM

أمثلة:

- Jira
    
- TheHive
    

الوصف:

- إدارة التذاكر
    
- متابعة Alerts
    
- توثيق
    

---

## 🔹 دور SOC L1 في Alert Triage

SOC L1:

- أول خط دفاع
    
- أكتر واحد بيتعامل مع Alerts
    

عدد Alerts:

- ممكن صفر
    
- ممكن 100 في اليوم
    

كل Alert:

- ممكن يكون هجوم حقيقي
    

### توزيع الأدوار:

- **L1**: يفرز Alerts ويصعّد
    
- **L2**: تحليل أعمق + علاج
    
- **Engineer**: تحسين Rules
    
- **Manager**: متابعة الجودة والسرعة
    

---

## 🔹 خصائص الـ Alert (Alert Properties)

### 1️⃣ Alert Time

- وقت إنشاء Alert
    
- غالباً بعد الحدث بدقائق
    

---

### 2️⃣ Alert Name

- وصف مختصر للحدث
    
- مبني على Rule
    

---

### 3️⃣ Alert Severity

- Low
    
- Medium
    
- High
    
- Critical
    

---

### 4️⃣ Alert Status

- New
    
- In Progress
    
- Closed
    
- حالات مخصصة
    

---

### 5️⃣ Alert Verdict

- True Positive
    
- False Positive
    

---

### 6️⃣ Alert Assignee

- المحلل المسؤول
    
- هو اللي يتحاسب على القرار
    

---

### 7️⃣ Alert Description

- منطق الـ Rule
    
- ليه الحدث خطر
    
- إرشادات triage أحياناً
    

---

### 8️⃣ Alert Fields

- تفاصيل تقنية:
    
    - Hostname
        
    - Command Line
        
    - User
        

---

## 🔹 اختيار Alert مناسب (Prioritisation)

### الخطوات:

1. فلترة Alerts
    
2. اختيار Alerts الجديدة فقط
    
3. ترتيب حسب Severity
    
4. الأقدم قبل الأحدث
    

السبب:

- الهجوم الأقدم غالباً سبّب ضرر أكتر
    

---

## 🔹 Alert Triage

# 🔹 Initial Actions (الخطوات الأولية)

> _“The initial steps are designed to ensure that you take ownership of the assigned alert and avoid interfering with alerts being handled by other analysts…”_

الخطوات الأولية دي هدفها الأساسي **تنظيم الشغل داخل الـ SOC** ومنع اللخبطة بين المحللين.

يعني إيه الكلام ده عملياً؟

في SOC حقيقي:

- فيه أكتر من محلل شغال في نفس الوقت
    
- نفس الـ Dashboard
    
- نفس الـ Alerts
    

لو مفيش تنظيم:

- ممكن محللين يشتغلوا على نفس Alert
    
- أو حد يقفل Alert وحد تاني لسه بيحلله
    
- أو Alert يتساب من غير متابعة
    

علشان كده الخطوات الأولية بتعمل الآتي:

---

## 1️⃣ Take Ownership (استلام الـ Alert)

أول حاجة تعملها:

- **Assign Alert to Yourself**
    

ده معناه:

- أنت المسؤول الوحيد عن الـ Alert ده
    
- محدش غيرك هيشتغل عليه
    
- أي قرار يحصل بعد كده يتحسب عليك
    

ده مهم جداً في:

- المحاسبة
    
- التتبع
    
- الجودة
    

---

## 2️⃣ Move to In Progress

بعد ما تستلم الـ Alert:

- تغيّر حالته من `New` إلى `In Progress`
    

الهدف من الخطوة دي:

- تبين لباقي الفريق إن الـ Alert ده **قيد التحقيق**
    
- تمنع أي محلل تاني يفتحه بالخطأ
    

---

## 3️⃣ Familiarising Yourself with Alert Details

قبل أي تحليل تقني:  
لازم **تفهم الـ Alert نفسه**

وده بيشمل:

### 🔸 Alert Name

اسم الـ Alert بيديك فكرة سريعة:

- Phishing
    
- Suspicious Login
    
- Malware Execution
    
- Credential Dumping
    

---

### 🔸 Alert Description

الوصف بيشرح:

- ليه الـ Alert ده طلع؟
    
- الـ Rule اشتغلت على إيه؟
    
- إيه اللي اعتبره النظام نشاط مشبوه؟
    

---

### 🔸 Key Indicators

المؤشرات المهمة زي:

- Username
    
- Hostname
    
- IP Address
    
- File Name
    
- Command Line
    

دي **مفاتيح التحقيق** اللي هتشتغل عليها بعد كده.

---

# 🔹 Investigation (مرحلة التحقيق)

> _“This is the most complex step…”_

دي **أصعب وأهم مرحلة** في الشغل كله.

ليه؟

- لأنك هنا بتقرر:
    
    - هل ده هجوم حقيقي؟
        
    - ولا نشاط طبيعي؟
        

وأي غلطة هنا:

- يا إما Attack يفلت
    
- يا إما تضيع وقت الفريق على False Alert
    

---

## 🔹 استخدام المعرفة التقنية

في المرحلة دي:

- بتستخدم خبرتك
    
- فهمك للهجمات
    
- فهمك لسلوك المستخدم الطبيعي
    

وبتراجع:

- SIEM Logs
    
- EDR Logs
    
- Network Logs
    
- Cloud Logs (لو موجود)
    

---

## 🔹 Workbooks / Playbooks / Runbooks

> _“some teams develop Workbooks…”_

بعض الفرق بتعمل:

- **Workbooks**
    
- أو **Playbooks**
    
- أو **Runbooks**
    

كلهم نفس الفكرة تقريباً:

📘 مستندات بتقولك:

- لما يطلع Alert من النوع ده
    
- تمشي على خطوات محددة
    
- علشان متنساش حاجة
    
- وعلشان كل المحللين يشتغلوا بنفس الجودة
    

مثال:

- Alert Phishing → افتح الإيميل → راجع الرابط → افحص الـ Header → شوف لو حد فتحه
    

---

## 🔹 لو مفيش Workbooks

لو الفريق معندوش Workbooks (وده شائع في فرق صغيرة)، النص بيديك **إرشادات أساسية**:

---

### 1️⃣ Understand who is under threat

لازم تحدد:

- مين المتأثر؟
    

ممكن يكون:

- User معين
    
- جهاز (Hostname)
    
- Cloud Account
    
- Network Segment
    
- Website
    

ليه ده مهم؟

- لأن حساب Admin مثلاً أخطر من User عادي
    
- Server أخطر من Laptop
    

---

### 2️⃣ Note the action described in the alert

يعني:

- إيه اللي حصل؟
    

أمثلة:

- Suspicious Login
    
- Malware Execution
    
- Phishing Email
    
- Credential Access
    
- Data Exfiltration
    

فهم نوع الفعل بيوجه التحقيق كله.

---

### 3️⃣ Review surrounding events

دي نقطة مهمة جداً 👇

لازم:

- تشوف الأحداث اللي حصلت **قبل وبعد الـ Alert**
    
- أحياناً الـ Alert نفسه مش واضح
    
- لكن الأحداث اللي حواليه بتفضح الهجوم
    

مثلاً:

- Login غريب
    
- بعده مباشرة Download Tool
    
- بعده Command Line مشبوه
    

---

### 4️⃣ Use Threat Intelligence

تستخدم:

- Threat Intelligence Platforms
    
- Google
    
- VirusTotal
    
- MITRE ATT&CK
    

علشان:

- تتأكد هل الـ IP معروف؟
    
- هل الـ File ضار؟
    
- هل السلوك ده مرتبط بهجوم معروف؟
    

---

# 🔹 Final Actions (الخطوات النهائية)

> _“Your decisions here determine whether you found or missed the potential cyberattack.”_

المرحلة دي **أخطر مرحلة** لأن:

- قرارك النهائي هو اللي بيتسجل
    
- وبيتراجع بعد كده
    
- وبيأثر على أمان الشركة
    

---

## 1️⃣ Decide the Verdict

تختار واحد من الاتنين:

### ✅ True Positive

يعني:

- هجوم حقيقي
    
- نشاط ضار
    
- Threat فعلاً
    

---

### ❌ False Positive

يعني:

- نشاط طبيعي
    
- Rule اشتغلت غلط
    
- User كان بيعمل حاجة شرعية
    

---

## 2️⃣ Prepare Detailed Comment

لازم تكتب Comment واضح يشمل:

- إيه اللي حصل؟
    
- حللت إزاي؟
    
- شفت إيه في اللوجز؟
    
- ليه قررت TP أو FP؟
    

التعليق ده:

- مرجع لأي حد بعدك
    
- دليل إن شغلك مهني
    
- مهم جداً في المراجعات
    

---

## 3️⃣ Close the Alert

بعد القرار والتعليق:

- ترجع للـ Dashboard
    
- تغيّر الحالة إلى `Closed`
    

وبكده:

- الـ Alert اتقفل رسمياً
    
- خرج من دورة العمل

---

## 🔹 ملاحظات Dashboard

لو:

- ماطلعش Flag
    

يبقى:

- القيم غلط
    
- اعمل Restart للـ Dashboard
    

---
