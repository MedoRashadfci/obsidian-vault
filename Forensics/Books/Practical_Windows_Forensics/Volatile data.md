
---

Under the principle of **“order of volatility”**, you must first collect information that is classified as **Volatile Data** (such as the list of network connections, running processes, and logon sessions), which will be irretrievably lost if the computer is powered off.

Then, you can start collecting **non-volatile data**, which can also be obtained using the traditional approach of disk image analysis. The main difference is that a **Live Forensics** dataset is easier to obtain while the machine is still running.

The process of obtaining a memory dump and a disk image, as well as their analysis, is described in detail in other chapters. This chapter focuses specifically on the collection of **Volatile Data**.

Typically, this category includes the following:

- System uptime and current time
    
- Network parameters (NetBIOS name cache, active connections, routing table, etc.)
    
- NIC configuration settings
    
- Logged-on users and active sessions
    
- Loaded drivers
    
- Running services
    
- Running processes and their related parameters (loaded DLLs, open handles, ownership)
    
- Autostart modules
    
- Shared drives and files opened remotely
    

Recording the **date and time** of data collection allows you to define a time interval in which the investigator can analyze the system:

```bash
(date /t) & (time /t) > %COMPUTERNAME%\systime.txt
systeminfo | find "Boot Time" >> %COMPUTERNAME%\systime.txt
```

The last command shows how long the machine has been running since the last reboot.

Using the **%COMPUTERNAME%** environment variable allows the creation of separate directories for each machine, which is useful when collecting data from multiple computers within a network.

---
