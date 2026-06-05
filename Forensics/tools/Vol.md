# Volatility Framework - Quick Guide

## What is Volatility?

Volatility is an open-source memory forensics framework used to analyze RAM memory dumps collected from Windows, Linux, and macOS systems.

It helps investigators identify:

- Running processes
    
- Network connections
    
- Loaded DLLs
    
- Injected code
    
- Malware artifacts
    
- Registry data
    
- User activity
    
- Rootkits and hidden processes
    

Volatility is one of the most widely used tools in Digital Forensics and Incident Response (DFIR).

---

# Basic Syntax

```bash
vol.py -f memory_dump.raw <plugin>
```

Example:

```bash
vol.py -f memory.raw windows.pslist
```

---

# Essential Commands

## 1. System Information

Displays operating system information.

```bash
vol.py -f memory.raw windows.info
```

Useful for:

- OS Version
    
- Build Number
    
- System Architecture
    
- Memory Acquisition Time
    

---

## 2. List Running Processes

```bash
vol.py -f memory.raw windows.pslist
```

Shows:

- Process Name
    
- PID
    
- Parent PID
    
- Creation Time
    

---

## 3. Display Process Tree

```bash
vol.py -f memory.raw windows.pstree
```

Shows parent-child process relationships.

Useful for detecting:

```text
explorer.exe
 └── cmd.exe
      └── powershell.exe
```

---

## 4. Scan for Hidden Processes

```bash
vol.py -f memory.raw windows.psscan
```

Detects:

- Hidden processes
    
- Unlinked processes
    
- Rootkit activity
    

---

## 5. Command Line Arguments

```bash
vol.py -f memory.raw windows.cmdline
```

Displays command-line arguments used to launch processes.

Useful for detecting:

```text
powershell -enc ...
cmd.exe /c ...
```

---

## 6. Network Connections

```bash
vol.py -f memory.raw windows.netscan
```

Displays:

- Local IP
    
- Remote IP
    
- Ports
    
- Associated PID
    

Useful for identifying C2 communications.

---

## 7. DLL Analysis

```bash
vol.py -f memory.raw windows.dlllist
```

Lists DLLs loaded by processes.

Specific PID:

```bash
vol.py -f memory.raw windows.dlllist --pid 1234
```

---

## 8. Detect Code Injection

```bash
vol.py -f memory.raw windows.malfind
```

Searches for:

- Process Injection
    
- Shellcode
    
- Fileless Malware
    

Indicators:

- MZ headers
    
- RWX memory pages
    
- Suspicious executable memory
    

---

## 9. Analyze Handles

```bash
vol.py -f memory.raw windows.handles
```

Shows:

- Files
    
- Registry Keys
    
- Events
    
- Mutexes
    

Useful for ransomware investigations.

---

## 10. Search for Malware Signatures

```bash
vol.py -f memory.raw windows.yarascan
```

Using YARA rules:

```bash
vol.py -f memory.raw windows.yarascan --yara-file rules.yar
```

---

## 11. List Loaded Drivers

```bash
vol.py -f memory.raw windows.modules
```

Useful for:

- Driver Analysis
    
- Rootkit Detection
    

---

## 12. Scan for Hidden Drivers

```bash
vol.py -f memory.raw windows.driverscan
```

Finds hidden kernel drivers.

---

## 13. SSDT Hook Detection

```bash
vol.py -f memory.raw windows.ssdt
```

Detects SSDT hooks used by rootkits.

---

## 14. Dump Process Memory

Dump all memory pages from a process:

```bash
vol.py -f memory.raw -o dumps windows.memmap.Memmap --pid 1234 --dump
```

Useful for:

- Extracting malware
    
- Recovering URLs
    
- Finding User-Agents
    
- Extracting shellcode
    

---

## 15. Scan Files in Memory

```bash
vol.py -f memory.raw windows.filescan
```

Finds files present in memory.

---

## 16. Extract Files

```bash
vol.py -f memory.raw windows.dumpfiles
```

Useful for recovering malware samples.

---

# Common Investigation Workflow

## Step 1

Gather system information.

```bash
windows.info
```

## Step 2

Identify suspicious processes.

```bash
windows.pslist
windows.pstree
windows.psscan
```

## Step 3

Check network activity.

```bash
windows.netscan
```

## Step 4

Inspect suspicious process.

```bash
windows.cmdline
windows.dlllist
```

## Step 5

Check for code injection.

```bash
windows.malfind
```

## Step 6

Search for malware indicators.

```bash
windows.handles
windows.yarascan
```

## Step 7

Dump suspicious process memory.

```bash
windows.memmap.Memmap --dump
```

---

# Most Important Commands for CTFs

```bash
windows.info
windows.pslist
windows.pstree
windows.psscan
windows.cmdline
windows.netscan
windows.dlllist
windows.handles
windows.malfind
windows.filescan
windows.dumpfiles
windows.yarascan
windows.memmap.Memmap --dump
```

These commands alone are enough to solve the majority of beginner and intermediate Memory Forensics investigations.