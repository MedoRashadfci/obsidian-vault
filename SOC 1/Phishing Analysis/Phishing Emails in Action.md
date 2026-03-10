
# 🎣 السيناريو: Cancel your PayPal order

المهاجم بيحاول يخليك تصدق إنك عملت عملية شراء على:

PayPal

وإن لازم “تلغي الطلب”.

---

# 🚨 أول علامة خطر — Spoofed Email

المذكور:

`From Name: service@paypal.com Actual Email: gibberish@sultanbogor.com`

📌 هنا عندك Spoofing واضح.

- الاسم الظاهر = PayPal
    
- الدومين الحقيقي = sultanbogor.com
    

ده وحده كافي لتصنيف الرسالة Malicious.

---

# 🚨 العلامة الثانية — Recipient Mismatch

الإيميل اتبعت لعنوان غير مرتبط بالحساب.

دي تقنية شائعة:

- إرسال عشوائي
    
- أو خطأ متعمد لإرباك الضحية
    

---

# 🚨 العلامة الثالثة — Urgency

Subject:

> Cancel your PayPal order

ده بيلعب على:

- الخوف
    
- السرعة
    
- رد الفعل العاطفي
    

المهاجم عايزك تضغط قبل ما تفكر.

---

# 🎨 العلامة الرابعة — HTML Impersonation

![https://security.berkeley.edu/sites/default/files/styles/panopoly_image_original/public/paypal_phish_example.png?itok=UI5VHEgr&timestamp=1459449832](https://security.berkeley.edu/sites/default/files/styles/panopoly_image_original/public/paypal_phish_example.png?itok=UI5VHEgr&timestamp=1459449832)

![https://www.pcrisk.de/images/stories/screenshots202407/paypal-order-confirmation-email-scam-main.jpg](https://www.pcrisk.de/images/stories/screenshots202407/paypal-order-confirmation-email-scam-main.jpg)

![https://cdn.prod.website-files.com/6130a9118b1be9aebe2c2837/66cecb2d08dcf0c5dd76cb53_PayPalScam1.webp](https://cdn.prod.website-files.com/6130a9118b1be9aebe2c2837/66cecb2d08dcf0c5dd76cb53_PayPalScam1.webp)

4

الرسالة:

- لوجو PayPal
    
- ألوان رسمية
    
- زرار Cancel the order
    

ده كله تصميم نفسي.

---

# 🔗 أهم نقطة: الرابط المختصر

الرابط كان Shortened URL.

⚠️ ليه المهاجم يستخدم URL Shortener؟

- يخفي الدومين الحقيقي
    
- يتجاوز بعض الفلاتر
    
- يقلل الشك
    

---

# 🔍 لما تم توسيع الرابط

تمدد إلى:

`google.com`

دي نقطة مهمة جدًا 👇


__________________________________________________________________________

---

# 📦 Track your package

(تتبّع شحنتك)

ده نوع شائع جدًا من رسائل التصيد، لأنه بيعتمد على شيء الناس بتنتظره فعلًا: شحنة أو طلب.

---

# 🎣 التقنيات المستخدمة في هذا الإيميل

## 1️⃣ Spoofed Email Address (انتحال عنوان البريد)

يعني:

- اسم المرسل يظهر كأنه من شركة شحن.
    
- لكن العنوان الحقيقي في الـ Header مختلف.
    

مثال:

`From: Shipping Center Actual Email: random@maliciousdomain.com`

⚠️ الهدف: كسب ثقة الضحية.

---

## 2️⃣ Pixel Tracking (تتبّع بالبكسل)

دي نقطة خطيرة ومهمة جدًا 👇

![https://www.getmailbird.com/assets/components/phpthumbof/cache/How-email-tracking-pixel-works.d204858b3a85cee3ecabb3f42cfbfa5d.webp](https://www.getmailbird.com/assets/components/phpthumbof/cache/How-email-tracking-pixel-works.d204858b3a85cee3ecabb3f42cfbfa5d.webp)

![https://miro.medium.com/1%2AW3H_FMKqGPrCwr-hOiVzRA.jpeg](https://miro.medium.com/1%2AW3H_FMKqGPrCwr-hOiVzRA.jpeg)

![https://www.seobility.net/en/wiki/images/f/fc/Tracking-pixel.png](https://www.seobility.net/en/wiki/images/f/fc/Tracking-pixel.png)

4

### ما هو Tracking Pixel؟

هو:

- صورة صغيرة جدًا (1x1 pixel غالبًا)
    
- غير مرئية للمستخدم
    
- مضمّنة داخل الإيميل
    

مثال في الكود:

`<img src="http://malicious-site.com/track?id=12345">`

### ماذا يحدث؟

عند فتح الإيميل:

- يتم تحميل الصورة من سيرفر المهاجم
    
- السيرفر يعرف:
    
    - إن الإيميل تم فتحه
        
    - عنوان الـ IP
        
    - نوع الجهاز
        
    - أحيانًا الموقع الجغرافي التقريبي
        

💡 ده بيساعد المهاجم يعرف إن:

> "الإيميل وصل لواحد حقيقي ومهتم"

---

## 3️⃣ Link Manipulation (التلاعب بالرابط)

يعني:

- الرابط يبدو شرعي.
    
- لكن في الحقيقة يشير إلى دومين مشبوه.
    

مثال:

`Track your package: shipping-update.com`

لكن الرابط الحقيقي:

`malware-delivery-center.xyz`

---

# 🔎 ملاحظات سريعة على الإيميل

## ✅ يبدو وكأنه من مركز توزيع

المهاجم اختار سيناريو واقعي:

- شركة شحن
    
- رقم تتبع
    
- إشعار توصيل
    

ده يسمى:

> Pretexting (اختلاق سيناريو مقنع)

---

## ✅ Subject يحتوي على Tracking Number

ده بيزيد الإقناع.

مثال:

`Your package is on hold – Tracking #ZX9218374`

ده بيخلي الضحية تفكر:

> يمكن فعلًا طلبت حاجة؟

---

## ❓ لماذا Yahoo حجب الصور تلقائيًا؟

لأن:

- الصور قد تحتوي على Tracking Pixels
    
- تحميل الصورة = تسريب معلومات
    

كثير من مزودي البريد يفعلون ذلك للحماية.

---

# 🧠 لماذا لم يعمل Hover على الرابط؟

لأن:

- Yahoo عطّل الروابط في الإيميل
    
- إجراء أمني لمنع النقر العرضي
    

---

# 🔍 تحليل الـ Raw Source

عند فتح الكود الخام ظهر:

`Tracking.png`

ده مؤشر على وجود:

- Image-based tracker
    
- أو redirect مخفي
    

---

# 🎯 لماذا يستخدم المهاجم Tracking Pixels؟

الأسباب:

1. معرفة من فتح الإيميل
    
2. معرفة الجهاز المستخدم
    
3. تحديد الضحايا المحتملين
    
4. تحسين حملات Spam
    
5. بيع بيانات النشاط
    

---

# 🔗 الرابط يشير إلى دومين مشبوه

وصفه في النص:

> shady-looking domain

يعني:

- اسم عشوائي
    
- TLD غريب (.xyz .top .info)
    
- لا علاقة له بشركة شحن حقيقية
    

⚠️ غالبًا يكون:

- Malware distribution
    
- Credential harvesting
    
- Redirect chain
    

لكن:

> لازم تحليل إضافي لتأكيد ذلك.

---

# 🛡 تحليل أمني احترافي (لو كنت Billy)

## Indicators of Compromise:

- Spoofed sender
    
- Tracking pixel
    
- Suspicious domain
    
- Shipping pretext
    
- Image-based tracking
    

---

# 📌 نوع الهجوم؟

ده:

✔ Phishing  
✔ Social Engineering  
✔ Tracking-enabled campaign

هدفه غالبًا:

- سرقة بيانات
    
- أو تثبيت Malware
    
- أو تأكيد صلاحية البريد


____________________________________________________________________________
# 📄 Select your email provider to view document

(اختر مزود بريدك الإلكتروني لعرض المستند)

---

# 🎣 التقنيات المستخدمة في هذا الهجوم

## 1️⃣ Urgency (الإلحاح)

الإيميل بيقول إن:

- رابط تحميل الفاكس **ينتهي في نفس اليوم**
    
- لازم تتحرك فورًا
    

⚠️ الهدف:  
تعطيل التفكير المنطقي ودفع الضحية للتصرف بسرعة.

---

## 2️⃣ HTML Impersonation

(انتحال علامة تجارية باستخدام HTML)

الصفحة الأولى تبدو وكأنها من:

Microsoft

وتحديدًا تشبه:

Microsoft OneDrive

لكن:

- الـ URL ليس له علاقة بـ Microsoft
    
- الدومين مشبوه
    

ده يسمى:

> Brand Impersonation

---

## 3️⃣ Link Manipulation

(التلاعب بالرابط)

الرابط الظاهر يوحي بأنه:

- مستند رسمي
    
- فاكس مهم
    
- مشاركة عبر OneDrive
    

لكن عند الفحص:

- الدومين مختلف
    
- لا يوجد أي علاقة رسمية بالشركة
    

---

## 4️⃣ Credential Harvesting

(حصاد بيانات تسجيل الدخول)

الضحية يتم توجيهها إلى صفحة أخرى تشبه:

Adobe

لكن:

- URL ليس Adobe
    
- عنوان التبويب مكتوب فيه:  
    “Share Point Online”
    

وده منتج تابع لـ:

Microsoft SharePoint

⚠️ هنا تناقض:

- صفحة تشبه Adobe
    
- عنوان يقول SharePoint
    
- لكن الرابط لا علاقة له بأي منهما
    

ده مؤشر قوي جدًا على التصيد.

---

# 🔐 كيف تتم سرقة البيانات؟

الضحية تُطلب منها:

> تسجيل الدخول باستخدام مزود البريد الخاص بها

وتظهر خيارات مثل:

- Outlook
    
- Gmail
    
- Yahoo
    

في السيناريو:  
الضحية اختارت Outlook.

---

## ماذا يحدث فعليًا؟

عند إدخال البيانات:

حتى لو كانت البيانات صحيحة:

- يظهر خطأ في تسجيل الدخول.
    

ليه؟

لأن:

- الضحية لا تتصل فعليًا بـ Outlook
    
- لا يوجد Authentication حقيقي
    

بل:

- يتم إرسال الـ Username و Password مباشرة إلى سيرفر المهاجم
    

🎯 الهدف تحقق:

> تم سرقة بيانات الدخول

---

# 🚨 نقطة مهمة جدًا

حتى لو أدخلت بيانات صحيحة:

- ستظهر رسالة خطأ دائمًا
    

لأن الهدف:

- جمع البيانات فقط
    
- وليس تسجيل الدخول
    

---

# ✍️ Poor Grammar & Typos

(أخطاء نحوية وإملائية)

رغم احترافية التصميم:

- توجد أخطاء لغوية واضحة
    
- صياغة غير طبيعية
    

ده مؤشر شائع جدًا في حملات التصيد.

---

# 🧠 تسلسل الهجوم (Attack Flow)

1. إيميل يوهم بوجود فاكس مهم
    
2. زر لتحميل المستند
    
3. إعادة توجيه إلى صفحة تشبه OneDrive
    
4. إعادة توجيه إلى صفحة تشبه Adobe
    
5. مطالبة بإدخال بيانات البريد
    
6. سرقة البيانات
    
7. عرض رسالة خطأ وهمية



