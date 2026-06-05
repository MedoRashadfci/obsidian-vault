##### 1-Which Linux distribution is being used on this machine?
After use FTK imager and search `/var/log/syslog`

![[Pasted image 20260430013537.png]]

then Answer: **Kali**

##### 2-What is the MD5 hash of the Apache **access.log** file?
![[Pasted image 20260430014122.png]]

after extract file hash 

![[Pasted image 20260430014042.png]]

**d41d8cd98f00b204e9800998ecf8427e**


##### 3-It is suspected that a credential dumping tool was downloaded. What is the name of the downloaded file?

Just open Download folder >![[Pasted image 20260430014329.png]]
**mimikatz_trunk.zip**
##### 4-A super-secret file was created. What is the absolute path to this file?

in kali can show old commends with file in each user name `/.bash_history` 

![[Pasted image 20260430012809.png]]

after extract  : `/root/Desktop/SuperSecretFile.txt`

![[Pasted image 20260430013054.png]]


##### 5-What program used the file **didyouthinkwedmakeiteasy.jpg** during its execution?
 with file ./bash_history >![[Pasted image 20260430015028.png]] 
 **binwalk**

##### 6-What is the third goal from the checklist Karen created?

on Desktop directory > found Checklist file ![[Pasted image 20260430015453.png]]
After Extract 
![[Pasted image 20260430015415.png]]

**Profit**

##### 7-How many times was Apache run?
log files in `/var/log/apache2/`

![[Pasted image 20260430020613.png]]

**Answer : 0**

##### 8-This machine was used to launch an attack on another. Which file contains the evidence for this?

9-
![[Pasted image 20260430021734.png]]
Young

10-
![[Pasted image 20260430022125.png]]
postgres

11-
![[Pasted image 20260430022329.png]]

/root/Documents/myfirsthack/