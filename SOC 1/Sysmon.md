# Event ID 1 - Process Creation

بيسجل أي Process جديد بيتشغل على الجهاز.

### مفيد في اكتشاف:

- PowerShell
- CMD
- Malware Executables
- Living Off The Land Binaries (LOLBins)

### مثال

```
<ProcessCreate onmatch="exclude">    <CommandLine condition="is">        C:\Windows\system32\svchost.exe -k appmodel -p -s camsvc    </CommandLine></ProcessCreate>
```

هنا Sysmon هيستبعد العملية دي من اللوجات لأنها عملية ويندوز طبيعية.

### أثناء التحقيق

ابحث عن:

```
powershell.execmd.exerundll32.exeregsvr32.exemshta.exewscript.execscript.exe
```

---

# Event ID 3 - Network Connection

بيسجل أي اتصال شبكة بيعمله Process.

### مفيد في اكتشاف

- Reverse Shells
- Malware C2 Communication
- Port Scanning
- Data Exfiltration

### مثال

```
<NetworkConnect onmatch="include">    <Image condition="image">nmap.exe</Image>    <DestinationPort condition="is">4444</DestinationPort></NetworkConnect>
```

### المعنى

لو:

```
nmap.exe
```

عمل اتصال

أو

```
Port 4444
```

اتفتح

هيتسجل Event.

---

### أثناء التحقيق

ابحث عن:

```
Destination Port 4444Destination Port 1337Destination Port 8080
```

وكمان اتصالات خارجية غريبة.

---

# Event ID 7 - Image Loaded

بيسجل DLLs اللي أي Process بيعملها Load.

### مفيد في اكتشاف

- DLL Injection
- DLL Hijacking
- Malicious DLLs

### مثال

```
<ImageLoad onmatch="include">    <ImageLoaded condition="contains">\Temp\</ImageLoaded></ImageLoad>
```

### المعنى

أي DLL يتم تحميله من:

```
C:\Users\...\Temp\
```

هيتسجل.

وده غير طبيعي غالباً.

---

### أثناء التحقيق

ابحث عن:

```
*.dll
```

داخل:

```
TempAppDataDownloads
```

---

# Event ID 8 - CreateRemoteThread

من أهم Events في Sysmon.

بيسجل محاولة Process حقن كود داخل Process أخرى.

### مفيد في اكتشاف

- Process Injection
- Cobalt Strike
- Meterpreter
- Malware Injection

---

### مثال

```
<CreateRemoteThread onmatch="include">    <StartAddress condition="end with">0B80</StartAddress></CreateRemoteThread>
```

### أثناء التحقيق

ابحث عن:

```
SourceImageTargetImage
```

مثال:

```
powershell.exe -> explorer.exe
```

أو

```
malware.exe -> lsass.exe
```

---

# Event ID 11 - File Created

بيسجل الملفات التي تم إنشاؤها أو تعديلها.

### مفيد في اكتشاف

- Malware Droppers
- Ransomware
- Persistence Files

### مثال

```
<FileCreate onmatch="include">    <TargetFilename condition="contains">        HELP_TO_SAVE_FILES    </TargetFilename></FileCreate>
```

### المعنى

إذا تم إنشاء ملف باسم:

```
HELP_TO_SAVE_FILES.txt
```

فهذا مؤشر شائع لبرامج الفدية.

---

### أثناء التحقيق

ابحث عن:

```
.exe.dll.ps1.bat.vbs.hta
```

---

# Event ID 12 / 13 / 14 - Registry Events

تسجل أي تغيير في Registry.

---

## Event 12

Registry Object Created

---

## Event 13

Registry Value Set

---

## Event 14

Registry Key Renamed

---

### مفيد في اكتشاف

- Persistence
- Registry Run Keys
- Startup Modifications

### مثال

```
<RegistryEvent onmatch="include">    <TargetObject condition="contains">        Windows\System\Scripts    </TargetObject></RegistryEvent>
```

### أثناء التحقيق

ابحث عن:

```
HKCU\Software\Microsoft\Windows\CurrentVersion\RunHKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

---

# Event ID 15 - FileCreateStreamHash

بيكشف استخدام:

```
Alternate Data Streams (ADS)
```

### تقنية مشهورة لإخفاء الملفات الخبيثة.

---

### مثال

```
<FileCreateStreamHash onmatch="include">    <TargetFilename condition="end with">.hta</TargetFilename></FileCreateStreamHash>
```

### مثال فعلي

```
echo malware > test.txt:hidden.hta
```

هينشئ ملف مخفي داخل ADS.

---

### أثناء التحقيق

ابحث عن:

```
:Zone.Identifier:hidden.hta
```

---

# Event ID 22 - DNS Query

بيسجل جميع استعلامات DNS.

### مفيد في اكتشاف

- Malware Domains
- C2 Servers
- DNS Tunneling
- Phishing Domains

### مثال

```
<DnsQuery onmatch="exclude">    <QueryName condition="end with">        .microsoft.com    </QueryName></DnsQuery>
```

### المعنى

يستبعد استعلامات Microsoft لأنها Noise.

---

### أثناء التحقيق

ابحث عن:

```
pastebin.comngrok.ioduckdns.orgno-ip.org
```

وأي دومينات عشوائية مثل:

```
asdhjkasd123.xyz
```

---

# أهم Event IDs تحفظها للفورينزيكس

| Event ID | الوظيفة                |
| -------- | ---------------------- |
| 1        | Process Creation       |
| 3        | Network Connection     |
| 7        | DLL Load               |
| 8        | Process Injection      |
| 11       | File Creation          |
| 12-14    | Registry Changes       |
| 15       | Alternate Data Streams |
| 22       | DNS Queries            |

---
----
# ما هو Sysmon؟

Sysmon (System Monitor) أداة مجانية من Microsoft تعمل كخدمة داخل ويندوز وتقوم بتسجيل أحداث تفصيلية جداً عن:

- العمليات (Processes)
    
- الاتصالات الشبكية
    
- تحميل الـ DLLs
    
- تغييرات الـ Registry
    
- إنشاء الملفات
    
- DNS Queries
    

وتقوم بإرسالها إلى Windows Event Logs.

---

# 1. تحميل Sysmon

يمكن تحميل Sysmon منفرداً أو تحميل حزمة Sysinternals بالكامل.

### باستخدام PowerShell

```powershell
Download-SysInternalsTools C:\Sysinternals
```

سيتم تنزيل جميع أدوات Sysinternals داخل:

```text
C:\Sysinternals
```

---

# 2. تحميل ملف Configuration

Sysmon بدون Config يسجل Logs قليلة نسبياً.

لذلك نستخدم ملفات Configuration جاهزة مثل:

- [SwiftOnSecurity Sysmon Config](https://github.com/SwiftOnSecurity/sysmon-config?utm_source=chatgpt.com)
    
- [ION-Storm Sysmon Config](https://github.com/ion-storm/sysmon-config?utm_source=chatgpt.com)
    

هذه الملفات تضيف قواعد متقدمة لاكتشاف الأنشطة المشبوهة.

---

# 3. تشغيل Sysmon

افتح PowerShell أو CMD كمسؤول Administrator.

ثم نفذ:

```cmd
Sysmon.exe -accepteula -i ..\Configurations\swift.xml
```

### شرح الخيارات

|الخيار|الوظيفة|
|---|---|
|-accepteula|قبول اتفاقية الاستخدام تلقائياً|
|-i|Install Sysmon|
|swift.xml|ملف الإعدادات المستخدم|

---

# ماذا يحدث أثناء التثبيت؟

ستظهر رسائل مشابهة:

```text
Loading configuration file
Configuration file validated
Sysmon installed
SysmonDrv installed
Starting SysmonDrv
Starting Sysmon
```

معناها:

✅ تم التحقق من ملف الإعدادات

✅ تم تثبيت الخدمة

✅ تم تشغيل Driver

✅ بدأ Sysmon في تسجيل الأحداث

---

# 4. أين تجد اللوجات؟

افتح:

```text
Event Viewer
```

ثم:

```text
Applications and Services Logs
 └── Microsoft
      └── Windows
           └── Sysmon
                └── Operational
```

أو من Run:

```cmd
eventvwr.msc
```

---

# 5. التحقق من عمل Sysmon

بعد التثبيت ستجد Events مثل:

```text
Event ID 1
Process Creation

Event ID 3
Network Connection

Event ID 22
DNS Query
```

إذا ظهرت هذه الأحداث فالتثبيت ناجح.

---

# أوامر مهمة جداً

### عرض الإعدادات الحالية

```cmd
Sysmon.exe -c
```

---

### تحديث Configuration

```cmd
Sysmon.exe -c newconfig.xml
```

---

### إزالة Sysmon

```cmd
Sysmon.exe -u
```

---

### عرض المساعدة

```cmd
Sysmon.exe -?
```

---

# كـ Threat Hunter أو DFIR Analyst

أول 5 Events تركز عليها دائماً هي:

|Event ID|السبب|
|---|---|
|1|Process Creation|
|3|Network Connections|
|8|Process Injection|
|11|File Creation|
|22|DNS Queries|

هذه الأحداث وحدها تكشف نسبة كبيرة من:

- Malware Execution
    
- PowerShell Attacks
    
- Reverse Shells
    
- Cobalt Strike Beacons
    
- Persistence Techniques
    
- Command & Control Traffic
    

وهي الأحداث التي ستراها باستمرار في منصات مثل Splunk وWazuh وMicrosoft Sentinel أثناء التحقيقات الأمنية.