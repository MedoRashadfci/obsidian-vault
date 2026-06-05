
# **TryHackMe Tempest**

![Perseverance Logo](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/logo.png)

[](https://readysetexploit.gitlab.io/home/forensics/tempest/#Intro) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#Setup) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#MaliciousDoc) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#Follina) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#C2) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#Reconnaissance) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#Persistence) [](https://readysetexploit.gitlab.io/home/forensics/tempest/#Conclusion)

# Intro

Tempest is a digital forensics challenge from [TryHackMe](https://tryhackme.com/) in which we are tasked to find a whole attack chain. This challenge does an amazing job at emulating what a Incident Responder would do in order to find out more about a compromised system. We are provided with a few tools that will help us that should be a part of every aspiring Blue Teamer's toolset.

If you prefer a video walk-through, you can find it [here](https://youtu.be/KVdYS10ub6A)

# Setup

The room provides you with a terminal connection via its Split View functionality or you can also RDP into the machine with the credentials provided:

![creds](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/creds.png)

We can find the files we will be using in C:\Users\user\Desktop\Incident Files directory:

![files](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/files.png)

We can capture the SHA-256 hash of the files with the command Get-FileHash -Algorith SHA256 *

![hashes](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/hashes.png)

We are also going to use [TimeLine Explorer](https://aboutdfir.com/toolsandartifacts/windows/timeline-explorer/), a data-filtering tool that has a Excel like format, which is perfect for parsing data. We first need to convert the Event Viewer sysmont.evtx log file into a CSV file for TimeLine Explorer. Lucky for us, we have EvtxEcmd.exe installed. EvtxEcmd is one of the many tools created by [Eric Zimmerman](https://ericzimmerman.github.io/#!index.md). The tool is located in the C:\Tools\EvtxECmd\ directory and we can use the command .\EvtxECmd.exe -f 'C:\Users\user\Desktop\Incident Files\sysmon.evtx' --csv 'C:\Users\user\Desktop\Incident Files' --csvf sysmon.csv

![csv](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/csv.png)  
  
![csvdone](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/csvdone.png)

We can find TimeLine Explorer in the Taskbar. Then click on File, then Open, and select the CSV file:

![timeline](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/timeline.png)  
  
![timelineopen](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/timelineopen.png)

We are also going to use [SysmonView](https://github.com/nshalabi/SysmonTools), a tool that is used to visualize Sysmon logs. First let us open up the sysmon.evtx file using EventViewer

![eventviewer](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/eventviewer.png)

Then right-click on the log file and select Save All Events As. Select a location but make sure the file saves as a XML file. Takes about a minute or so:

![ev1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/ev1.png)  
  
![ev2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/ev2.png)  
  
![savedxml](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/savedxml.png)

We can find SysmonView in the Taskbar. Then click on File and Import Sysmon Event logs in order to upload our XML file:

![sysmonview](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/sysmonview.png)  
  
![sm1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/sm1.png)  
  
![sm2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/sm2.png)

And we are all set!

# Malicious Document

We know that a a user fell victim to a malicious document. We were informed by the Security of Operations Center (SOC) specialist that the malicious document has a .doc extension. We were also informed that the document was dowanload via chrome. So we can use Sysmonview to search for chrome.exe and open the Image Path and all of the Sessions (GUIDs)

![docfile1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/docfile1.png)  
  
![docfile2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/docfile2.png)

In TimeLine Explorer, we can search for chrome.exe using the Executable Info tab in order to get the user who downloaded the document

![execinfo1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/execinfo1.png)  
  
![username](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/username.png)

We can use TimeLine Explorer's Executable Info to search to get more information about the document, and we see that it was executed by windword.exe. We also can see the PID

![magicules](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/magicules.png)  
  
![pid](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/pid.png)

Looking into winword.exe using SysmonView, we find a suspicious domain:

![wordip](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/wordip.png)

But what was executed? Well using Payload Data 5, we can search for the Parent Process ID (PPID) and we find a very suspicious command:

![msdt](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/msdt.png)

Which a quick search reveals to be the Follina exploit:

![cve](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/cve.png)

Let us keep looking further.

# Follina

Examining the payload using [CyberChef](https://gchq.github.io/CyberChef/), we see where the payload was downloaded to. With the $app variable indicating that it starts in the C:\Users\benimaru\appdata\ directory:

![follinapayload](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/follinapayload.png)

We are informed that the payload was executed when the user logged on, which a quick search will reveal that is EVENT ID 4624

![eventid1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/eventid1.png)

If we look at the timestamp of the Follina attack, we see that it occured on 2022-06-20 at 17:13:35

![timestamp1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/timestamp1.png)

We were also provided with the Windows Event logs which we can also use TimeLine Explorer for:

![windowscsv](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/windowscsv.png)

Knowing the Event ID and the compromised user narrows down the search. Keep in mind that the username goes in Payload Data 1

![logintimestamp](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/logintimestamp.png)

Back in the Sysmon CSV, we can narrow down the search for Event ID 1 for process creation and for the benimaru username. Then starting with the timestamp of 17:14:49 we find a suspicious Powershell entry at 17:15:10

![powershell](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/powershell.png)

We see that the command download a binary called first.exe. We can search it using the Executable Info in order to find the SHA-256 hash:

![firsthash](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/firsthash.png)

Back in Sysmon, we can examine the binary and we find another suspicious domain

![susdomain](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/susdomain.png)

Let's see where this leads us.

# C2

Looking at the Powershell command once more, we notice a second domain

![susdomain2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/susdomain2.png)

We are also provided with the capture.pcap file that we can examine using [Wireshark](https://www.wireshark.org/) or [Brim](https://github.com/brimdata/brim). I am going to use Wireshark since it is a personal preference. We can search by hostname with the filter of http.host matches "phishteam.xyz" and we find the Full URL

![fullurl](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/fullurl.png)

Looking at our other suspicious domain with the filter http.host matches "resolvecyber.xyz". We start seeing the Command and Control (C2) execution:

![c2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/c2.png)

Following the TCP Stream, we can detect a few things:

1. The encoding of the command
2. The parameter
3. The URL Endpoint
4. The HTTP Method
5. And the User-Agent has the Programming Language used to compile the binary.

![tcpstream](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/tcpstream.png)

Now we can very slowly and painfully grab go through each stream and decode each base64 encoded payload. Or we can use [Tshark](https://www.wireshark.org/docs/man-pages/tshark.html). Tshark will allow us to parse through this PCAP file and get what need. I am going to cover how I did it, which is probably not the most effective or efficient way, but I got the results I needed. It is important to note that Tshark is not installed in this machine, but more on that later.

First thing is to assign the User-Agent as a column since that is what all of these payload have in common. We can do so by right-clicking on user-agent under the HyperText Transfer Protocol and selecting Apply as Column. Then we are going to click on our new column and select Column Preferences. And we will see our new Column Field Name

![gotcolumn1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/gotcolumn1.png)  
  
![gotcolumn2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/gotcolumn2.png)

Next, I am going to use [Impacket's](https://www.secureauth.com/labs/open-source-tools/impacket/) SMBServer so that I can transfer the file:

![smbserver1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/smbserver1.png)  
  
![smbserver2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/smbserver2.png)  
  
![smbserver3](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/smbserver3.png)

And now I am going to use Tshark to get what I need with the command tshark -r capture.pcapng -Y 'http.user_agent' | grep -w '9ab62b5' | grep -v 'HTTP 166' | awk '{print $9}' | sed 's/q=/ /g' | awk '{print $2}'

1. -Y allows to isolate to the particular field
2. We can greping for 9ab62b5
3. Then ignoring all HTTP 166 because that is just the response
4. Using awk to print the 9th column
5. Using sed to replace the q= with a single space
6. Using awk one more time get the 2nd column

![gotbase64](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/gotbase64.png)

We can save it all to a file and starting examining:

![savedtofile](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/savedtofile.png)

Let's examine the commands executed.

# Reconnaissance

Going through the commands, we see that the malicious threat actor found a Powershell script with an exposed password

![gotpass](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/gotpass.png)

We see that port 5985 is open which is used for WinRM. WinRM is way to connect to the machine remotely via a shell:

![winrm](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/winrm.png)

Then client downloaded a file called ch.exe

![downloadedch](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/downloadedch.png)

Which we can find the command executed for it using TimeLine Explorer

![chexe](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/chexe.png)

What kind of binary is this? We can look at the SHA-256 hash and do a quick search to get our answer:

![chisel1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/chisel1.png)  
  
![chisel2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/chisel2.png)  
  
![chisel3](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/chisel3.png)

What is Chisel? [Chisel](https://github.com/jpillora/chisel) is a binary used to establish tunneling. But was a tunnel established? Well there are few things to notice here:

1. The command using ch.exe occured at 17:18:48
2. We are aware that port 5985 is open
3. If we focus on Event ID 1 once again
4. The next timestamp is at 17:19:06
5. And we see that the wsmprovhost.exe process is launched. Which is used for WinRM

![winrm2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/winrm2.png)  
  
![winrm3](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/winrm3.png)  
  
![winrm4](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/winrm4.png)

Then we notice a small jump to where the user becomes SYSTEM

![tosystem](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/tosystem.png)

But how? Well back in TimeLine Explorer, we left off at timestamp 17:19:06. We see a Powershell command downloading another binary and it takes place at 17:20:06. We are still focused on Event ID 1

![spf1](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/spf1.png)  
  
![spf2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/spf2.png)

We can find more information about this binary using its SHA-256 hash:

![spf3](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/spf3.png)  
  
![spf4](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/spf4.png)

This particular exploit utilizes a specfic privilege according this [github repository](https://github.com/itm4n/PrintSpoofer)

![impersonate](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/impersonate.png)

We see that this binary executed another binary

![final](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/final.png)

Usisng SysmonView, we can see the same domain connection as before but to a different port

![final2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/final2.png)

Let us find out if there were any persistence methods established.

# Persistence

After becoming SYSTEM, the user was trying to add two accounts

![errors](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/errors.png)

But the user kept getting an error. The user was just missing a simple parameter:

![errorfixed](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/errorfixed.png)

Which we can confirm by searching for the Event ID in the Windows Logs

![acctcreateid](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/acctcreateid.png)  
  
![acctcreated](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/acctcreated.png)

Then we see that the malicious threat actor added one of the two users the Administrators group

![group](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/group.png)

That occured at timestamp of 17:27:41 which can pin point us to the Event ID using the Windows Logs:

![group2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/group2.png)  
  
![group3](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/group3.png)  
  
![group4](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/group4.png)

The attacker also established one last persistent method using sc.exe

![scexe](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/scexe.png)

The binary sc.exe is used to control services. The auto=start is assuring that the payload of final.exe runs often:

![scexe2](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/scexe2.png)

We can use TimeLine Explorer to find the full path of execution:

![finalexec](https://readysetexploit.gitlab.io/home/assets/images/forensics/tempest/finalexec.png)

And we finally painted a whole picture of the attack chain 😁

# Conclusion

This is a great room I found that is great for anyone looking to get more experience in Digital Forensics. By combining various methodologies with the various tools at our disposal, we were able to identify what the attacker was able to do. We were able to find the point of entry, the different lateral movements, the weaknesses in the system, and the privilege escalation. I highly recommend this room.

  

YouTube:

[ReadySetExploit](https://www.youtube.com/channel/UCUOD6TW1JBTfmyMO6uiq5sA)