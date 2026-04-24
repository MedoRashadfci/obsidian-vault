https://cyberdefenders.org/blueteam-ctf-challenges/psexec-hunt/
## Scenario:
**An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.**
------
#### First > Explain (What is PsExec ?)
This is a **lateral movement attack using PsExec**.

After an attacker breaks into one machine, they don’t stop there.  
They try to **move inside the network** to reach more important systems.

PsExec helps them:

- Run commands on another Windows machine remotely
- Use valid credentials (stolen username/password)
- Act like a normal admin, which makes detection harder

---

## How the attack works (step by step)

1. **Initial access**
    - Attacker gets into one computer (phishing, vulnerability, etc.)
2. **Credential use**
    - They get admin credentials (or reuse existing ones)
3. **Connect over SMB (port 445)**
    - They access hidden admin shares like:
        - `\\TARGET\ADMIN$`
        - `\\TARGET\C$`
4. **Upload PsExec service**
    - A file called:
        
        PSEXESVC.exe
        
    - is copied to the target machine
5. **Create remote service**
    - The attacker creates a service on the target system
6. **Execute commands**
    - Commands are run remotely (like opening a shell)
7. **Move to more machines**
    - Repeat the same steps across the network

---

## Why attackers like this method

- Uses **legitimate Windows tools**
- Looks like **normal admin activity**
- Works fast inside internal networks
- Hard to detect if you don’t monitor properly

---

## How to detect this attack (Network + Host)

### 1. Network detection (Wireshark / PCAP)

Look at SMB traffic:

- Filter:
    
    smb || smb2
    

Watch for:

- Connections to multiple internal machines
- Access to:
    
    ADMIN$  
    C$
    
- File upload:
    
    PSEXESVC.exe
    
- NTLM authentication between machines
- Same user logging into many hosts quickly

---

### 2. Host-based detection (Logs)

On Windows machines, check:

#### Security Logs:

- Event ID **4624** → Successful logon (type 3 = network)
- Event ID **4672** → Admin privileges used

#### System Logs:

- Event ID **7045** → New service installed  
    (look for `PSEXESVC`)

---

### 3. Behavioral signs

- One machine suddenly connects to many others
- Same username used across different hosts
- Admin shares accessed frequently
- Short-lived service creation and execution

---

## Simple detection idea

If you see:

- SMB login  
    → then file upload  
    → then service creation

This chain strongly suggests **PsExec lateral movement**

---

## How to reduce risk

- Limit admin privileges
- Disable or monitor admin shares if possible
- Use strong authentication (MFA)
- Monitor lateral movement behavior
- Use EDR/SIEM to correlate events

__________
-----
### **Then  > I Just filter with** **`smb2`**  and `dcerpc`  for solve this questions :

**To effectively trace the attacker's activities within our network, can you identify the IP address of the machine from which the attacker initially gained access? >>** 

**Use Wireshark to analyze the PCAP file and focus on IP addresses with unusual patterns or high-volume traffic**. 

*Answer : 10.0.0.130* 

-----

![[Pasted image 20260424162659.png]]

**To fully understand the extent of the breach, can you determine the machine's hostname to which the attacker first pivoted?**

Answer : Sales-PC
![[Pasted image 20260424163029.png]]

**Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?**
Answer : ssales
![[Pasted image 20260424163546.png]]

**After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What's the name of the service executable the attacker set up on the target?** 
Answer : PSEXESVC.exe
![[Pasted image 20260424163701.png]]
**We need to know how the attacker installed the service on the compromised machine to understand the attacker's lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?**
Answer : ADMIN$
![[Pasted image 20260424164007.png]]
We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?
Answer : IPC$
 
 ---- 
---
**DCERPC (Distributed Computing Environment Remote Procedure Call)** is a Windows protocol that allows one machine to execute functions or commands on another machine remotely. It is commonly used for administrative tasks like managing services and system operations.

In attacks like PsExec lateral movement, DCERPC is used after the file transfer stage. The attacker first uploads a service executable using SMB, then uses DCERPC to remotely create and start that service on the target machine.

During analysis in Wireshark, I used DCERPC traffic (such as service creation and execution calls) to track the attacker’s actions. By following these DCERPC requests, I was able to identify the second machine the attacker pivoted to, which helped answer Question 7. . 

**Now that we have a clearer picture of the attacker's activities on the compromised machine, it's important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?**

Answer :MARKETING-PC 
![[Pasted image 20260424171759.png]]