
## 🛠️ ما هي BitLocker2John؟

**bitlocker2john** هي أداة ضمن **John the Ripper**  
وظيفتها:

> استخراج **BitLocker hash** من قرص أو ملف مشفّر  
> عشان يتعمله **Password Cracking** باستخدام John

يعني:

- الأداة **مش بتفك التشفير**
    
- هي بس بتحوّل BitLocker data إلى **format يفهمه John**
    

---

## 🎯 متى تستخدم BitLocker2John؟

تستخدمها فقط في الحالات دي:

### ✅ Digital Forensics

- تحقيق في جهاز مصادَر
    
- نسيان كلمة مرور BitLocker داخل شركة
    

### ✅ Incident Response

- جهاز موظف اتقفل وضروري فتحه للتحقيق
    

### ✅ CTF / Lab

- TryHackMe
    
- Hack The Box
    

❌ **ممنوع**:

- اختراق أجهزة مش بتاعتك
    
- كسر تشفير بدون إذن
    

---

## 🧠 BitLocker2John بتحتاج إيه؟

واحد من دول:

- `.dd` disk image
    
- `.img`
    
- Physical drive
    
- BitLocker-encrypted partition
    

---

## 🐧 استخدام BitLocker2John على Kali Linux

### 1️⃣ التأكد إن John موجود

`john --version`

لو مش موجود:

`sudo apt update sudo apt install john`

---

### 2️⃣ استخراج BitLocker hash

لو عندك disk image مثلًا:

`bitlocker2john disk.img > bitlocker.hash`

لو partition:

`bitlocker2john /dev/sdb1 > bitlocker.hash`

وحل تاني اني اعرضهم على طول 
`bitlocker2john -i disk.dd `  
📌 الناتج:

- ملف فيه **BitLocker hash**
    
- ده اللي John هيشتغل عليه
    

---

### 3️⃣ كسر كلمة المرور باستخدام John

`john bitlocker.hash`

أو باستخدام wordlist:

`john --wordlist=/usr/share/wordlists/rockyou.txt bitlocker.hash`

---

### 4️⃣ عرض كلمة المرور بعد ما يخلص

`john --show bitlocker.hash`

---

## 🔍 BitLocker2John بيكسر إيه بالظبط؟

- BitLocker **Password-based encryption**
    
- مش بيكسر:
    
    - TPM-only protection
        
    - Hardware-backed encryption



mkdir /mnt/bitlocker
mkdir /mnt/bitlocker_raw



dislocker -V disk.dd -u<username> -- /mnt/bitlocker 