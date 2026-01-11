### **مقدمة في علم الاستطلاع (Introduction to Reconnaissance)**

يفترض هذا القسم أن القارئ يمتلك معرفة مسبقة بأساسيات شبكات الحاسوب (Computer Networks). وقبل الخوض في التفاصيل التقنية، يستشهد النص بالحكمة العسكرية القديمة من كتاب "فن الحرب" لـ Sun Tzu التي تقول: _"إذا كنت تعرف عدوك وتعرف نفسك، فنصرك لا شك فيه"_.

في سياق الأمن السيبراني:

- **المهاجم (Attacker):** يحتاج لجمع أكبر قدر من المعلومات عن الأنظمة المستهدفة.
    
- **المدافع (Defender):** يجب أن يدرك ما يمكن للخصم اكتشافه عن شبكته ليتمكن من حمايتها.
    

### **تعريف الـ Reconnaissance**

يُعرف الاستطلاع (Recon) بأنه عملية مسح أولي لجمع المعلومات عن الهدف. وهي تمثل الخطوة الأولى في نموذج **The Unified Kill Chain** بهدف الحصول على موطئ قدم أولي (Initial Foothold) في النظام. وينقسم الاستطلاع إلى فئتين:

#### **أولاً: الـ Passive Reconnaissance (الاستطلاع السلبي)**

في هذا النوع، يعتمد الممارس على المعلومات المتاحة علنًا (Publicly available knowledge). يتم الوصول إلى هذه البيانات من موارد عامة دون التفاعل المباشر مع الهدف. يشبه ذلك مراقبة منطقة معينة من بعيد دون الدخول إليها.

**أمثلة على أنشطة الـ Passive Recon:**

- البحث عن سجلات الـ **DNS records** لنطاق معين من خلال خادم DNS عام.
    
- مراجعة إعلانات الوظائف (Job ads) الخاصة بالشركة المستهدفة.
    
- قراءة المقالات الإخبارية عن الشركة.
    

#### **ثانياً: الـ Active Reconnaissance (الاستطلاع النشط)**

على عكس النوع الأول، لا يمكن القيام بهذا النوع بخصوصية تامة، لأنه يتطلب تفاعلاً مباشراً (Direct engagement) مع الهدف. يشبه ذلك فحص الأقفال على الأبواب والنوافذ للتأكد من نقاط الدخول المحتملة.

**أمثلة على أنشطة الـ Active Recon:**

- الاتصال المباشر بخوادم الشركة مثل **HTTP**, **FTP**, و **SMTP**.
    
- محاولة الحصول على معلومات عبر الهاتف (Social Engineering).
    
- دخول مقر الشركة بادعاء صفة فني إصلاح.
    

**ملاحظة قانونية:** نظرًا للطبيعة الاقتحامية لـ **Active Reconnaissance**، فقد يواجه الشخص ملاحقة قانونية ما لم يحصل على تفويض قانوني مسبق.

---

### **الأسئلة (Questions)**

**Answer the questions below:**

- **You visit the Facebook page of the target company, hoping to get some of their employee names. What kind of reconnaissance activity is this? (A for active, P for passive)**
    
    - **الإجابة:** **P** (لأنك تتعامل مع منصة خارجية "فيسبوك" وليس مع أنظمة الشركة مباشرة).
        
- **You ping the IP address of the company webserver to check if ICMP traffic is blocked. What kind of reconnaissance activity is this? (A for active, P for passive)**
    
    - **الإجابة:** **A** (لأن عملية الـ Ping ترسل حزم بيانات مباشرة إلى سيرفر الهدف).
        
- **You happen to meet the IT administrator of the target company at a party. You try to use social engineering to get more information about their systems and network infrastructure. What kind of reconnaissance activity is this? (A for active, P for passive)**
    
    - **الإجابة:** **A** (لأن الهندسة الاجتماعية تعتبر تفاعلاً مباشراً مع العنصر البشري التابع للهدف).






---

### **بروتوكول WHOIS (The WHOIS Protocol)**

يُعد **WHOIS** بروتوكول استعلام واستجابة (Request and response protocol) يتبع مواصفات **RFC 3912**. يعمل خادم الـ WHOIS عبر الاستماع على منفذ **TCP port 43** لاستقبال الطلبات الواردة.

تتولى جهة تسجيل النطاقات (Domain registrar) مسؤولية صيانة سجلات WHOIS لأسماء النطاقات التي تقوم بتأجيرها. وعند الاستعلام، يرد الخادم بمعلومات متنوعة، من أهمها:

- **Registrar:** الشركة التي تم تسجيل النطاق من خلالها.
    
- **Contact info of registrant:** بيانات التواصل مع مسجل النطاق (الاسم، المنظمة، العنوان، الهاتف)، ما لم يتم إخفاؤها عبر خدمة الخصوصية (Privacy service).
    
- **Creation, update, and expiration dates:** تواريخ التسجيل الأول، آخر تحديث، وموعد انتهاء الصلاحية.
    
- **Name Server:** الخوادم المسؤولة عن توجيه وحل اسم النطاق (DNS resolution).
    

### **طريقة الاستخدام (Usage)**

للحصول على هذه البيانات، نستخدم عميل WHOIS محلي أو خدمة عبر الإنترنت. في الأنظمة المعتمدة على Linux (مثل AttackBox أو Kali)، يتم استخدام الأداة عبر الطرفية (Terminal) بالصيغة التالية:

`whois DOMAIN_NAME`

**مثال تطبيقي:** عند تنفيذ الأمر `whois tryhackme.com` في بيئة العمل:

Bash

```
user@TryHackMe$ whois tryhackme.com
Domain name: tryhackme.com
Registrar: NAMECHEAP INC
Creation Date: 2018-07-05T19:46:15.00Z
Updated Date: 2021-05-01T19:43:23.31Z
Registrar Registration Expiration Date: 2027-07-05T19:46:15.00Z
Name Server: kip.ns.cloudflare.com
Name Server: uma.ns.cloudflare.com
```

### **الأهمية في الاختبار الاختراقي (Pentesting Significance)**

تساعد المعلومات التي يتم جمعها في اكتشاف "أسطح هجوم" (Attack surfaces) جديدة، مثل استخدام بيانات التواصل في هجمات الهندسة الاجتماعية (Social engineering) أو استهداف خوادم البريد الإلكتروني أو الـ DNS التابعة للعميل، بشرط أن تكون ضمن نطاق الفحص المتفق عليه.

_ملاحظة:_ بسبب إساءة استخدام هذه البيانات من قبل أدوات جمع البريد العشوائي (Spammers)، تقوم العديد من الخدمات بحجب أو تنقيح (Redact) عناوين البريد الإلكتروني أو استبدالها بخدمات الخصوصية.

---

### **الأسئلة (Questions)**

**Answer the questions below**

- **When was TryHackMe.com registered?**
    
    - **الإجابة:** **2018-07-05**
        
    - _(مستخرجة من حقل Creation Date في مخرجات Terminal المرفقة)._
        
- **What is the registrar of TryHackMe.com?**
    
    - **الإجابة: NAMECHEAP.com
        
    - _(تظهر بوضوح في حقل Registrar)._
        
- **Which company is TryHackMe.com using for name servers?**
    
    - **الإجابة:** **CLOUDFLARE.com**





### **أدوات استعلام الـ DNS: أداة nslookup و dig**

بعدما تعلمنا كيفية استخدام بروتوكول **WHOIS** للحصول على خوادم الأسماء (DNS servers)، ننتقل الآن لاستخدام هذه المعلومات في استخراج بيانات تقنية أدق حول النطاق المستهدف.

#### **1. أداة nslookup (Name Server Look Up)**

تُستخدم هذه الأداة للعثور على عنوان الـ IP الخاص بنطاق معين. الصيغة العامة للأمر هي:

`nslookup OPTIONS DOMAIN_NAME SERVER`

**المعاملات الأساسية (Parameters):**

- **OPTIONS:** تحتوي على نوع الاستعلام (Query type) كما هو موضح في الجدول أدناه.
    
- **DOMAIN_NAME:** اسم النطاق المراد البحث عنه.
    
- **SERVER:** خادم الـ DNS الذي ترغب في سؤاله (مثل خوادم Google `8.8.8.8` أو Cloudflare `1.1.1.1`).
    

|**Query type**|**Result**|
|---|---|
|**A**|IPv4 Addresses|
|**AAAA**|IPv6 Addresses|
|**CNAME**|Canonical Name (الأسماء المستعارة)|
|**MX**|Mail Servers (خوادم البريد)|
|**SOA**|Start of Authority (معلومات المنطقة)|
|**TXT**|TXT Records (سجلات نصية)|

**فائدة الاستعلام:** عندما نستعلم عن سجلات **A** لنطاق مثل `tryhackme.com` ونحصل على عناوين IPv4 متعددة، فإن كل عنوان يمثل "سطح هجوم" محتمل يمكن فحصه بحثًا عن ثغرات أمنية. أما سجلات **MX**، فهي تكشف لنا عن خوادم البريد؛ فإذا كانت الشركة تستخدم خدمات مثل Google، فمن المستبعد وجود ثغرات في إصدار الخادم، ولكن في حالات أخرى قد نجد خوادم غير محمية أو قديمة (Not patched).

---

#### **2. أداة dig (Domain Information Groper)**

بالنسبة للاستعلامات المتقدمة، نستخدم أداة **dig** التي توفر وظائف إضافية ومعلومات أكثر تفصيلاً مثل الـ **TTL** (Time To Live).

صيغة الأمر:

dig @SERVER DOMAIN_NAME TYPE

عند مقارنة مخرجات **dig** بمخرجات **nslookup**، نجد أن **dig** تعيد تفاصيل تقنية أوسع حول كيفية معالجة الطلب ووقت بقاء السجل في الذاكرة المؤقتة.

---

### **الأهمية في الاستطلاع (Reconnaissance Significance)**

تعد سجلات **TXT** كنزاً للمعلومات؛ فهي تُستخدم أحياناً للتحقق من ملكية النطاق، أو إعدادات أمن البريد (مثل SPF و DKIM)، وفي بعض الأحيان قد تحتوي على ملاحظات أو "أعلام" (Flags) مخفية أثناء اختبارات الاختراق.

---

### **الأسئلة (Questions)**

**Answer the questions below**

- **Check the TXT records of thmlabs.com. What is the flag there?**
    
    - **طريقة الحل:** يجب تنفيذ الأمر التالي في الطرفية: `nslookup -type=txt thmlabs.com` أو `dig thmlabs.com TXT`.
        
    - **الإجابة:** **THM{7be216097538a74b30e461019665f8a6}**



---

### **أداة DNSDumpster (استكشاف النطاقات الفرعية)**

لا تستطيع أدوات الاستعلام التقليدية مثل `nslookup` و `dig` العثور على النطاقات الفرعية (Subdomains) تلقائيًا. ومع ذلك، قد يحتوي النطاق المستهدف على نطاقات فرعية مثل `wiki.tryhackme.com` أو `webmail.tryhackme.com` والتي قد تكشف عن كم هائل من المعلومات. تكمن الخطورة في أن بعض هذه النطاقات الفرعية قد لا يتم تحديثها بانتظام، مما يؤدي إلى وجود خدمات مصابة بثغرات (Vulnerable services).

#### **كيفية اكتشاف النطاقات الفرعية؟**

هناك عدة طرق للبحث عن هذه النطاقات:

1. **محركات البحث (Search Engines):** تجميع قائمة من النتائج العامة، ولكنها عملية تتطلب فحص عشرات الصفحات.
    
2. **التخمين (Brute-forcing):** تجربة أسماء عشوائية ومعرفة ما إذا كان لها سجل DNS، وهي عملية تستهلك الكثير من الوقت.
    
3. **الخدمات المتخصصة (Online Services):** استخدام أدوات مثل **DNSDumpster**.
    

#### **مميزات DNSDumpster**

تعد **DNSDumpster** أداة استطلاع قوية تمنحك نتائج تفصيلية في استعلام واحد:

- **اكتشاف النطاقات الفرعية:** يمكنها العثور على نطاقات مثل `blog.tryhackme.com` التي قد لا تظهر في الاستعلامات العادية.
    
- **الجداول والرسوم البيانية:** تعرض المعلومات في جداول سهلة القراءة، بالإضافة إلى تمثيل رسومي (Graph) يوضح تفرع الـ DNS والـ MX إلى سيرفراتهم وعناوين الـ IP الخاصة بهم.
    
- **تحديد الموقع الجغرافي (Geolocation):** تحاول الأداة تحديد الموقع الفعلي للسيرفرات ومعرفة مالك الشبكة.
    
- **تصدير البيانات:** تتوفر ميزة تجريبية (Beta) تسمح بتصدير الرسم البياني وتحريك العناصر بداخله لتسهيل التحليل.
    

---

### **الأهمية في الاستطلاع (Reconnaissance Significance)**

بدلاً من إجراء عدة استعلامات يدوية للبحث عن سجلات A و MX و TXT، توفر هذه الأداة "لوحة تحكم" كاملة لنظام الـ DNS الخاص بالهدف، مما يساعد المختبر على رسم خارطة واضحة لسطح الهجوم.

---

### **الأسئلة (Questions)**

**Answer the questions below**

- **Lookup tryhackme.com on DNSDumpster. What is one interesting subdomain that you would discover in addition to www and blog?**
    
    - **طريقة الحل:** عند الدخول لموقع DNSDumpster والبحث عن `tryhackme.com` وفحص قائمة النطاقات الفرعية (Subdomains).
        
    - **الإجابة:** **remote**


### **محرك البحث Shodan.io**

عندما يتم تكليفك بإجراء اختبار اختراق (Penetration test) ضد أهداف محددة، تبرز أهمية خدمة **Shodan.io** كأداة قوية في مرحلة الاستطلاع السلبي (**Passive reconnaissance**). تتيح لك هذه الأداة جمع معلومات متنوعة عن شبكة العميل دون الحاجة للاتصال المباشر بها. وعلى الجانب الدفاعي، يمكن للمؤسسات استخدامه لمعرفة الأجهزة المتصلة والمنكشفة (Exposed devices) التابعة لها.

#### **ما هو Shodan.io؟**

بخلاف محركات البحث التقليدية التي تفهرس صفحات الويب، يحاول **Shodan.io** الاتصال بكل جهاز يمكن الوصول إليه عبر الإنترنت لبناء محرك بحث لـ **"الأشياء" (Things)** المتصلة. بمجرد أن يتلقى استجابة من أي جهاز، يقوم بجمع كافة المعلومات المتعلقة بالخدمة وحفظها في قاعدة بياناته لتصبح قابلة للبحث.

#### **ما الذي يمكننا تعلمه من Shodan؟**

عند البحث عن نطاق مثل `tryhackme.com` أو عنوان IP محدد، يمكننا استخراج تفاصيل تقنية هامة تشمل:

- **IP address:** عنوان البروتوكول الخاص بالجهاز.
    
- **Hosting company:** الشركة المستضيفة للخدمة.
    
- **Geographic location:** الموقع الجغرافي للجهاز.
    
- **Server type and version:** نوع خادم الويب وإصداره (مثل Apache أو nginx).
    

يعتبر **Shodan** أداة حاسمة للمختبرين لأنها قد تكشف عن أجهزة منزلية، كاميرات مراقبة، أو أنظمة تحكم صناعية (ICS) غير محمية ومتاحة للعامة. يوفر الموقع خيارات بحث متقدمة تساعد في تصفية النتائج بناءً على المنفذ (Port)، الدولة، أو نظام التشغيل.

---

### **الأسئلة (Questions)**

**Answer the questions below**

- **According to Shodan.io, what is the first country in the world in terms of the number of publicly accessible Apache servers?**
    
    - **طريقة الحل:** بالبحث في Shodan باستخدام `product:Apache` ومراجعة قسم "Top Countries".
        
    - **الإجابة:** **United States**
        
- **Based on Shodan.io, what is the 3rd most common port used for Apache?**
    
    - **طريقة الحل:** من خلال نتائج بحث Apache، نلقي نظرة على قسم "Top Ports".
        
    - **الإجابة:** **8080**
        
- **Based on Shodan.io, what is the 3rd most common port used for nginx?**
    
    - **طريقة الحل:** بالبحث عن `product:nginx` ومراجعة ترتيب المنافذ الأكثر استخداماً في القائمة الجانبية.
        
    - **الإجابة:** **8888**


ركزنا في هذا القسم على تقنيات الاستطلاع السلبي (**Passive Reconnaissance**)، وهي المرحلة التي يتم فيها جمع البيانات دون أي اتصال مباشر مع أجهزة الهدف. تناولنا الأدوات التي تعمل عبر سطر الأوامر (Command-line tools) مثل **whois** و **nslookup** و **dig**، كما استعرضنا الخدمات العامة المتاحة عبر الويب مثل **DNSDumpster** و **Shodan.io**.

تكمن قوة هذه الأدوات في قدرتها على كشف "منجم" من المعلومات بمجرد إتقان خيارات البحث وفهم النتائج، مما يساعد في رسم صورة دقيقة للهدف قبل البدء بأي خطوة هجومية فعلية.

---

### **جدول مرجعي للأوامر (Command Reference Table)**

هذا الجدول يلخص أهم الأوامر التي تم تغطيتها في الدروس السابقة وأمثلة على استخدامها:

|**Purpose (الغرض من الأمر)**|**Command-line Example (مثال)**|
|---|---|
|Lookup **WHOIS** record|`whois tryhackme.com`|
|Lookup DNS **A** records|`nslookup -type=A tryhackme.com`1234|
|Lookup DNS **MX** records at DNS server5678|`nslookup -type=MX tryhackme.com 1.1.1.1`9101112|
|Lookup DNS **TXT** records13141516|`nslookup -type=TXT tryhackme.com`17181920|
|Lookup DNS **A** records21222324|`dig tryhackme.com A`25262728|
|Lookup DNS **MX29** records at DNS server303132|`dig @1.1.1.1 tryhackme.com MX`333435|
|Lookup DNS **TXT** records363738|`dig tryhackme.com TXT`3940|