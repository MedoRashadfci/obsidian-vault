## مقدمة عن سجلات Linux

على عكس الاعتقاد الشائع، فإن الأنظمة القائمة على **Linux-based systems** ليست حصينة ضد البرمجيات الخبيثة (**Malware**)، بل إن الاختراقات التي تستهدفها في تزايد مستمر. لذا، بصفتك محلل عمليات أمنية (**SOC analyst**)، ستحتاج غالباً للتحقيق في التنبيهات (**Alerts**) الخاصة بهذه الأنظمة، وهذا يتطلب فهماً عميقاً لكيفية عمل السجلات (**Logging**).

يركز هذا المحتوى على التوزيعات الشهيرة مثل **Debian**, **Ubuntu**, **CentOS**, و **RHEL**، وتحديداً على خوادم Linux التي لا تحتوي على واجهة رسومية (**Servers without a GUI**).

---

## العمل مع السجلات (Working With Logs)

تختلف أنظمة Linux عن Windows في أنها تخزن معظم الأحداث في ملفات نصية بسيطة (**Plain text files**). هذا يعني أنه يمكنك قراءة السجلات باستخدام أي محرر نصوص (**Text editor**) دون الحاجة لأدوات متخصصة مثل **Event Viewer**.

ومع ذلك، فإن سجلات Linux الافتراضية تُعد أقل تنظيماً (**Less structured**)؛ حيث لا توجد أكواد للأحداث (**Event codes**) أو قواعد صارمة لتنسيق السجلات (**Log formatting rules**). تقع معظم هذه الملفات في المجلد **/var/log**.

## ملف Syslog

يُعد ملف **/var/log/syslog** تدفقاً مجمعاً (**Aggregated stream**) لمختلف أحداث النظام. عند استعراض محتواه، ستجد هيكلية تحتوي على:

1. الطابع الزمني (**Timestamp**).
    
2. اسم الجهاز (**Hostname**).
    
3. الخدمة أو العملية التي ولّدت السجل مع رقم التعريف الخاص بها (**Process ID**).
    
4. رسالة الحدث التفصيلية.
    

---

## تصفية السجلات (Filtering Logs)

عند قراءة ملف الـ **syslog**، ستواجه آلاف الأحداث، لكن القليل منها فقط مفيد لعمل الـ **SOC**. لذا، يجب عليك تصفية السجلات وتقليص نطاق البحث.

- يمكنك استخدام أمر **"grep"** للبحث عن كلمات مفتاحية معينة، مثل **"CRON"** لرؤية السجلات المتعلقة بالمهام المجدولة فقط.
    
- يمكنك استخدام الخيار **"grep -v"** لاستثناء كلمة معينة من النتائج.
    

---

## اكتشاف السجلات (Discovering Logs)

إذا كنت تبحث عن أحداث معينة (مثل عمليات تسجيل دخول المستخدمين) ولا تعرف مكانها الدقيق، يمكنك البحث في جميع ملفات المجلد **/var/log**.

بما أنها ملفات نصية، يمكنك استخدام **grep** للبحث عن كلمات مثل **"login"**, **"auth"**, أو **"session"** في كافة الملفات داخل المجلد لتحديد الملف الصحيح الذي يحتوي على المعلومات المطلوبة.

|**الملف**|**الوصف**|
|---|---|
|**/var/log/auth.log**|يحتوي عادةً على سجلات المصادقة (Authentication).|
|**/var/log/kern.log**|يحتوي على سجلات النواة (Kernel).|
|**/var/log/dpkg.log**|يحتوي على سجلات تثبيت الحزم والبرامج.|

---

## تنبيهات هامة (Logging Caveats)

يسمح Linux بتغيير تنسيق السجل (**Log format**)، ومستوى التفاصيل (**Verbosity**)، ومكان التخزين بسهولة. وبسبب تعدد التوزيعات، قد تلاحظ أن السجلات تختلف من نظام لآخر أو قد لا تجد بعضها في أماكنها الافتراضية بناءً على إعدادات كل نظام.

---

---

---

## الـ Authentication Logs

أول وأهم **log file** ستحتاج لعمل **monitor** له هو الـ `/var/log/auth.log` (أو `/var/log/secure` في الأنظمة الـ **RHEL-based**). ورغم أن اسمه يوحي بأنه مخصص لعمليات الـ **authentication** فقط، إلا أنه في الحقيقة مخزن لكل الـ **user management events**، والـ **sudo commands** التي يتم تنفيذها، وأكثر من ذلك بكثير! لنبدأ بالـ **log file format**:
![[Pasted image 20260404213409.png]]
### الـ Login and Logout Events

هناك طرق كثيرة يعمل بها المستخدمون **authenticate** للدخول إلى ماكينة **Linux**: سواء كان ذلك **locally**، أو عبر الـ **SSH**، أو باستخدام أوامر **"sudo"** أو **"su"**، وحتى الـ **automatic logins** الخاصة بالـ **cron jobs**. كل عملية **logon** و **logoff** ناجحة يتم تسجيلها، ويمكنك رؤيتها عبر عمل **filter** للأحداث التي تحتوي على **keywords** مثل **"session opened"** أو **"session closed"**:

**Local and Remote Logins**

Bash

```
root@thm-vm:~$ cat /var/log/auth.log | grep -E 'session opened|session closed'
# Local, on-keyboard login and logout of Bob (login:session)
2025-08-02T16:04:43 thm-vm login[1138]: pam_unix(login:session): session opened for user bob(uid=1001) by bob(uid=0)
2025-08-02T19:23:08 thm-vm login[1138]: pam_unix(login:session): session closed for user bob
# Remote login examples of Alice (via SSH and then SMB)
2025-08-04T09:09:06 thm-vm sshd[839]: pam_unix(sshd:session): session opened for user alice(uid=1002) by alice(uid=0)
2025-08-04T12:46:13 thm-vm smbd[1795]: pam_unix(samba:session): session opened for user alice(uid=1002) by alice(uid=0)
```

**Cron and Sudo Logins**

Bash

```
root@thm-vm:~$ cat /var/log/auth.log | grep -E 'session opened|session closed'
# Traces of some cron job launch running as root (cron:session)
2025-08-06T19:35:01 thm-vm CRON[41925]: pam_unix(cron:session): session opened for user root(uid=0) by root(uid=0)
2025-08-06T19:35:01 thm-vm CRON[3108]: pam_unix(cron:session): session closed for user root
# Carol running "sudo su" to access root (sudo:session)
2025-08-07T09:12:32 thm-vm sudo: pam_unix(sudo:session): session opened for user root(uid=0) by carol(uid=1003)
```

بالإضافة لـ **system logs**، الـ **SSH daemon** بيخزن الـ **log** الخاص به لعمليات الـ **SSH logins** الناجحة والفاشلة. هذه السجلات تذهب لنفس الملف `auth.log` ولكن بـ **format** مختلف قليلاً:

**SSH-Specific Events**

Bash

```
root@thm-vm:~$ cat /var/log/auth.log | grep "sshd" | grep -E 'Accepted|Failed'
# Common SSH log format: <is-successful> <auth-method> for <user> from <ip>
2025-08-07T11:21:25 thm-vm sshd[3139]: Failed password for root from 222.124.17.227 port 50293 ssh2
2025-08-07T14:17:40 thm-vm sshd[3139]: Failed password for admin from 138.204.127.54 port 52670 ssh2
2025-08-09T20:30:51 thm-vm sshd[1690]: Accepted publickey for bob from 10.19.92.18 port 55050 ssh2: <key>
```

---

### الـ Miscellaneous Events

يمكنك أيضاً استخدام نفس الـ **log file** لاكتشاف الـ **user management events**. الأمر سهل إذا كنت تعرف الـ **basic Linux commands**: إذا كان `useradd` هو الأمر لإضافة مستخدمين جدد، ابحث فقط عن **keyword** باسم "useradd" لترى أحداث إنشاء المستخدمين!

**User Management Events**

Bash

```
root@thm-vm:~$ cat /var/log/auth.log | grep -E '(passwd|useradd|usermod|userdel)\['
2023-02-01T11:09:55 thm-vm passwd[644]: password for 'ubuntu' changed by 'root'
2025-08-07T22:11:11 thm-vm userdel[1887]: delete user 'oldbackdoor'
2025-08-07T22:11:29 thm-vm useradd[1878]: new user: name=backdoor, UID=1002, GID=1002, shell=/bin/sh
2025-08-07T22:11:54 thm-vm usermod[1906]: add 'backdoor' to group 'sudo'
2025-08-07T22:11:54 thm-vm usermod[1906]: add 'backdoor' to shadow group 'sudo'
```

أخيراً، واعتماداً على الـ **system configuration**، قد تصادف أحداثاً غريبة أو **unexpected**. مثلاً، قد تجد الـ **commands** التي نُفذت باستخدام **sudo**، وهذا يساعدك في تتبع الـ **malicious actions**. في المثال أدناه، استخدم المستخدم "ubuntu" الـ **sudo** لإيقاف الـ **EDR**، وقراءة حالة الـ **firewall**، وفي النهاية الوصول للـ **root**:

**Commands Run With Sudo**

Bash

```
root@thm-vm:~$ cat /var/log/auth.log | grep -E 'COMMAND='
2025-08-07T11:21:49 thm-vm sudo: ubuntu : TTY=pts/0 ; [...] COMMAND=/usr/bin/systemctl stop edr
2025-08-07T11:23:18 thm-vm sudo: ubuntu : TTY=pts/0 ; [...] COMMAND=/usr/bin/ufw status numbered
2025-08-07T11:23:33 thm-vm sudo: ubuntu : TTY=pts/0 ; [...] COMMAND=/usr/bin/su
```

---------------

---

## الـ Generic System Logs

نظام **Linux** بيسجل أحداث تانية كتير متوزعة في ملفات داخل مسار `/var/log`. الـ **Logs** دي بتشمل تحركات الـ **kernel**، تغييرات الشبكة، والـ **package installation**. شكل الـ **content** بيختلف حسب نوع الـ **OS**، ودي أشهر الملفات:

- **`/var/log/kern.log`**: رسائل وأخطاء الـ **Kernel**، ودي بنحتاجها في التحقيقات المتقدمة (**advanced investigations**).
    
- **`/var/log/syslog`** (أو **`/var/log/messages`**): ده "البث المباشر" المجمع لأحداث كتير ومختلفة في النظام.
    
- **`/var/log/dpkg.log`** (أو **`/var/log/apt`**): سجلات إدارة البرامج في الأنظمة الـ **Debian-based**.
    
- **`/var/log/dnf.log`** (أو **`/var/log/yum.log`**): سجلات إدارة البرامج في الأنظمة الـ **RHEL-based**.
    

الـ **logs** دي كنز في حالات الـ **DFIR** (التحقيق الجنائي الرقمي)، لكن نادراً ما بيعتمد عليها فريق الـ **SOC** بشكل يومي لأنها بتبقى **noisy** (فيها دوشة وتفاصيل كتير) وصعبة في الـ **parsing** (التحليل والاستخراج).

---

## الـ App-Specific Logs

في الـ **SOC**، أحياناً بتحتاج تعمل **monitor** لبرنامج معين. مثلاً، عشان تعرف إيه اللي بيحصل في الموقع بتاعك، لازم تحلل الـ **web server logs**. إليك مثال من سجلات الـ **Nginx**:

**Nginx Web Access Logs**

Bash

```
root@thm-vm:~$ cat /var/log/nginx/access.log
# Every log line corresponds to a web request to the web server
10.0.1.12 - - [11/08/2025:14:32:10 +0000] "GET / HTTP/1.1" 200 3022
10.0.1.12 - - [11/08/2025:14:32:14 +0000] "GET /login HTTP/1.1" 200 1056
10.0.1.12 - - [11/08/2025:14:33:09 +0000] "POST /login HTTP/1.1" 302 112
10.0.4.99 - - [11/08/2025:17:11:20 +0000] "GET /images/logo.png HTTP/1.1" 200 5432
10.0.5.21 - - [11/08/2025:17:56:23 +0000] "GET /admin HTTP/1.1" 403 104
```

كل سطر هنا بيمثل **request** (طلب) وصل للسيرفر، بيوضح الـ **IP**، الوقت، الصفحة المطلوبة، وحالة الرد (زي **200** للنجاح أو **403** للمنع).

---

## الـ Bash History

مصدر تاني مهم جداً هو الـ **Bash history**، وهي ميزة بتسجل كل أمر بتكتبه وتدوس **Enter**. الأوامر بتتحفظ في الـ **memory** طول ما الـ **session** شغالة، وأول ما تعمل **logout** بتتكتب في ملف اسمه `~/.bash_history`.

**Bash History File and Command**

Bash

```
ubuntu@thm-vm:~$ cat /home/ubuntu/.bash_history
echo "hello" > world.txt
nano /etc/ssh/sshd_config
sudo su

ubuntu@thm-vm:~$ history
1 echo "hello" > world.txt
2 nano /etc/ssh/sshd_config
3 sudo su
4 ls -la /home/ubuntu
5 cat /home/ubuntu/.bash_history
6 history
```

### الـ Bash History Limitations (القيود والعيوب)

رغم أهميته، الـ **SOC teams** مش بيعتمدوا عليه كلياً؛ لأنه مبيسجلش الـ **non-interactive commands** (زي أوامر الـ **cron jobs**). والأهم من كده إن الـ **attackers** يقدروا يخدعوه بسهولة:

1. **Leading Space:** لو المهاجم حط "مسافة" قبل الأمر، غالباً مش بيتسجل في الـ **logs**!
    
    Bash
    
    ```
    ubuntu@thm-vm:~$  echo "You will never see me in logs!"
    ```
    
2. **Scripts:** ممكن يحط أوامره كلها في **script** ويشغله، فـ الـ **history** يسجل تشغيل السكريبت بس مش الأوامر اللي جواه.
    
    Bash
    
    ```
    ubuntu@thm-vm:~$ nano legit.sh && ./legit.sh
    ```
    
3. **Changing Shells:** ممكن يستخدم **shell** تاني زي `/bin/sh` وده مبيسجلش الـ **history** بنفس طريقة الـ **Bash**.
    
    Bash
    
    ```
    ubuntu@thm-vm:~$ sh
    $ echo "I am no longer tracked by Bash!"
    ```
    

---
-----
## الـ Runtime Monitoring

حتى الآن، استكشفنا مصادر كتير للـ **Logs** في **Linux**، لكن مفيش ولا واحد منهم يقدر يجاوبك بدقة على أسئلة زي: "إيه البرامج اللي Bob شغلها النهاردة؟" أو "مين اللي مسح الـ **home folder** بتاعي وإمتى؟".

ده لأن الـ **Linux** بشكل افتراضي مش بيسجل الـ **process creation** (إنشاء العمليات) أو الـ **file changes** (تعديلات الملفات)، ودي اللي بنسميها **runtime events**. وحتى نظام **Windows** عنده نفس القصور، وعشان كدة بنحتاج أدوات إضافية زي **Sysmon** في ويندوز، وبنستخدم نهج مشابه في لينكس.

---

## الـ System Calls (نداءات النظام)

عشان نفهم الأدوات دي بتشتغل إزاي، لازم نفهم مفهوم جوهري في أي **OS** وهو الـ **System Calls**.

باختصار، أي برنامج عايز يعمل حاجة "حساسة" (زي فتح ملف، تشغيل بروسيس، استخدام الكاميرا)، لازم يطلب "إذن" من قلب النظام (**Kernel**). الطلب ده هو الـ **System Call**.

- في **Linux**، فيه أكتر من **300** نوع من الـ **system calls**.
    
- أشهرهم مثلاً هو `execve`؛ وده اللي بيستخدمه النظام عشان "ينفذ" أو يشغل أي برنامج.
    

### ليه لازم نعرف الـ System Calls؟

ببساطة، لأن كل أنظمة الـ **EDR** وأدوات الـ **logging** الحديثة بتعتمد عليهم. هي بتراقب الـ **system calls** الأساسية دي وبتحولها لكلام مفهوم لينا كبشر.

الميزة القوية هنا إن **المهاجمين (Attackers)** تقريباً مستحيل يهربوا من الـ **system calls**؛ لأن أي حركة هيعملوها على الجهاز لازم تمر عبر الـ **Kernel**. كل اللي عليك إنك تختار الـ **system calls** اللي عايز تراقبها وتسجلها.


---
----
---

## الـ Audit Daemon (Auditd)

الـ **Auditd** هو نظام مدمج في **Linux** وظيفته الأساسية هي مراقبة الـ **runtime**. الفكرة مش إننا نسجل "كل حاجة" (لأن ده هيعمل آلاف الجيجا بايت من الـ **noisy logs**)، لكن الفكرة إننا نكتب **Rules** (قواعد) ذكية تراقب أهم الـ **system calls**.

القواعد دي بتكون موجودة في مسار `/etc/audit/rules.d/`.

### كيف تقرأ نتائج الـ Auditd؟

بدل ما ندوخ في ملف الـ **log** الخام، بنستخدم أمر سحري اسمه `ausearch`. ده بيرتب المخرجات ويخليها سهلة القراءة.

**مثال: البحث عن تشغيل أداة "Wget"**

Bash

```
root@thm-vm:~$ ausearch -i -k proc_wget
---- type=PROCTITLE msg=audit(08/12/25 12:48:19.093:2219) : proctitle=wget https://files.tryhackme.thm/report.zip type=CWD msg=audit(08/12/25 12:48:19.093:2219) : cwd=/root type=EXECVE msg=audit(08/12/25 12:48:19.093:2219) : argc=2 a0=wget a1=https://files.tryhackme.thm/report.zip type=SYSCALL msg=audit(08/12/25 12:48:19.093:2219) : arch=x86_64 syscall=execve [...] ppid=3752 pid=3888 auid=ubuntu uid=root tty=pts1 exe=/usr/bin/wget key=proc_wget
```

هنا الـ `ausearch` هيقسم لك الحدث لـ 4 سطور مهمة:

1. **PROCTITLE:** بيوريك الأمر اللي اتكتب بالظبط (مثلاً: `wget https://...`).
    
2. **CWD:** بيقولك المستخدم كان واقف في أنهي مجلد وهو بينفذ الأمر.
    
3. **SYSCALL:** وده "القلب"، فيه تفاصيل تقنية زي:
    
    - **pid / ppid:** رقم العملية ورقم "الأب" اللي شغلها (عشان تبني شجرة العمليات **Process Tree**).
        
    - **auid (Audit User):** المستخدم اللي دخل للجهاز أصلاً (حتى لو حول لـ root هيفضل اسمه متسجل هنا).
        
    - **uid:** المستخدم اللي نفذ الأمر فعلياً حالياً.
        
    - **exe:** المسار الحقيقي للبرنامج اللي اشتغل.
        
    - **key:** الكلمة اللي إحنا اخترناها في الـ **rule** عشان نسهل البحث (زي `proc_wget`).
        

---

## مراقبة الملفات (File Events)

الـ **Auditd** مش بس للبرامج، ده كمان "مخبر" ممتاز للملفات الحساسة. لو حد حاول يعدل في ملف الـ **SSH** مثلاً، الـ **Auditd** هيجيبه:

**مثال: تعديل إعدادات الـ SSH**

Bash

```
root@thm-vm:~$ ausearch -i -k file_sshconf
---- type=PROCTITLE msg=audit(08/12/25 13:06:47.656:2240) : proctitle=nano /etc/ssh/sshd_config type=CWD msg=audit(08/12/25 13:06:47.656:2240) : cwd=/ type=PATH msg=audit(08/12/25 13:06:47.656:2240) : item=0 name=/etc/ssh/sshd_config [...] type=SYSCALL msg=audit(08/12/25 13:06:47.656:2240) : arch=x86_64 syscall=openat [...] ppid=3752 pid=3899 auid=ubuntu uid=root tty=pts1 exe=/usr/bin/nano key=file_sshconf
```

هتلاقي في الـ **Logs** إن الـ **syscall** المستخدم هو `openat` (لفتح الملف)، والبرنامج المستخدم هو `nano`.

---

## البدائل (Auditd Alternatives)

رغم قوة الـ **Auditd**، إلا أن مخرجاته أحياناً بتكون صعبة في القراءة أو الربط مع أنظمة الـ **SIEM**. عشان كدة فيه بدائل تانية بتشتغل بنفس المبدأ (مراقبة الـ **System Calls**):

- **Sysmon for Linux:** لو أنت متعود على **Windows Sysmon** وبتحب الـ **XML**.
    
- **Falco:** ممتاز جداً للأنظمة اللي شغالة بـ **Containers** (زي Docker و Kubernetes).
    
- **Osquery:** بيخليك تسأل عن معلومات الجهاز وكأنك بتكتب **SQL Query**.
    
- **EDRs:** الحلول الأمنية المتكاملة اللي بتراقب كل حاجة تلقائياً.