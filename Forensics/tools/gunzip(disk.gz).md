بفك بيها الملفات اللي امتدادها .gz وبتتحول تبقا .dd لو ملف عبارة عن disk
ولو عايزين نعرض الdisk  نعمل كده :

mkdir mnt
sudo mount -o loop disko-1.dd mnt
ls mnt

### ### `mkdir mnt`

يعني:

> اعمل مجلد اسمه **mnt**

هنستخدمه كـ **مكان نركّب (mount) فيه القرص** علشان نقدر نتصفحه كأنه folder عادي.


لو عايز تحذفه تكتب sudo umount mnt او sudo umount -l mnt
sudo rm -r mnt


---

### ### `sudo mount -o loop disko-1.dd mnt`

هذه أهم خطوة 👇  
هي تعمل **mount** لملف الديسك disko-1.dd كأنه هارد ديسك حقيقي.

- الـ `-o loop` معناها:  
    **عامل الملف كأنه جهاز Block Device (هارد).**
    
- mount ببساطة:
    
    > اربط القرص في مجلد mnt حتى نقدر ندخل ونقرأ محتوياته.
    

بعد هذه الخطوة، محتوى القرص يصبح ظاهر كأنه:

`mnt/
    home/
        etc/
            var/
                ...etc`

---

### ### `ls mnt`

ده ببساطة لعرض الملفات داخل القرص:

`ls mnt`

يعرض:

- directories
    
- files
    
- hidden files أحيانًا
    

---

# 🔥 لماذا نعمل هذه الخطوة؟

لأن ملف **.dd** هو نسخة raw من قرص كامل.  
ومحتوياته لا تظهر إلا إذا:

✔ تم mount  
✔ تم التعامل معه كقسم filesystem

بعد mount:

- يمكنك الدخول كأنك دخلت على جهاز آخر:
    

`cd mnt/home/
cd mnt/etc/
cat ملفات grep على flag`