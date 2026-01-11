
**Binwalk** هي أداة بتحلل الملفات الثنائية (**Binary Files**) وتبحث بداخلها عن:

- ملفات مضغوطة
    
- أنظمة ملفات
    
- Firmware
    
- صور، أرشيفات، أكواد
    
- بيانات متضمّنة داخل ملفات تانية
    

📌 مش بتبص على اسم الملف، بتبص على **الـ binary content**.

---

## 🧠 تعمل إيه بالظبط؟

Binwalk:

- تقرأ الملف Byte by Byte
    
- تبحث عن **File Signatures**
    
- تحدد:
    
    - نوع البيانات
        
    - مكانها (Offset)
        
    - حجمها التقريبي
        
- وممكن تستخرجها تلقائيًا
    

---

## 🕵️‍♂️ ليه بنستخدم Binwalk؟

تستخدم لما:

- يكون في ملف شكله عادي (JPG / EXE / BIN)
    
- لكن جواه:
    
    - Zip مخفي
        
    - Image
        
    - Firmware
        
    - File system
        
- أو في تحديات CTF
    

مثال:

> صورة JPG جواها ملف ZIP فيه Flag 😄

---

## 📂 أمثلة شائعة لاستخدام Binwalk

- تحليل Firmware أجهزة (Routers, IoT)
    
- اكتشاف Malware مخفي
    
- Forensics challenges
    
- Reverse engineering
    
- Steganography
    

---

## 🧪 مثال عملي بسيط

### فحص ملف:

`binwalk file.bin`

🔍 Output مثال:

`OFFSET      HEX         DESCRIPTION ------------------------------------------------ 0           0x0         PNG image 102400      0x19000     ZIP archive`

يعني:

- الملف يبدأ بصورة PNG
    
- جواه ZIP مخفي عند offset معين
    

---

### استخراج المحتوى المخفي:

`binwalk -e file.bin`

هيعمل فولدر:

`_extracted/  ├── 0.png  ├── 19000.zip`

---

## 🔥 استخراج أعمق (Recursive):

`binwalk -eM file.bin`

📌 `-M` = Matryoshka (يفتح حاجة جوا حاجة جوا حاجة)

---

## 📌 تحليل Firmware

`binwalk firmware.bin`

تقدر تشوف:

- SquashFS
    
- ELF binaries
    
- Kernel
    

---

## ⚠️ الفرق بين Binwalk و Foremost

|الأداة|الاستخدام|
|---|---|
|Binwalk|اكتشاف واستخراج بيانات **مخفية داخل ملف**|
|Foremost|استرجاع ملفات من Disk Image أو Raw Data|

📌 Binwalk ذكية في **التحليل**  
📌 Foremost قوية في **الاسترجاع**

---

## 🧠 قيود Binwalk

- مش دايمًا يطلع كل حاجة
    
- أحيانًا يحتاج:
    
    - manual extraction
        
    - dd
        
- مش بيعتمد على file system كامل
    

---