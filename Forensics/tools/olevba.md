## Overview

Olevba is a Python-based forensic and malware analysis tool that is part of the Oletools suite. It is designed to analyze Microsoft Office documents and extract VBA macros that may contain malicious code.

The tool is widely used in malware analysis, incident response, digital forensics, and threat hunting to identify malicious Office documents used in phishing campaigns, malware delivery, and initial access attacks.

Supported file types include:

- .doc
    
- .docm
    
- .dot
    
- .xls
    
- .xlsm
    
- .ppt
    
- .pptm
    
- OpenXML Office documents
    
- OLE files
    

---

## Installation

### Using pip

```bash
pip install oletools
```

### Verify Installation

```bash
olevba --help
```

---

## Basic Usage

Analyze a suspicious Office document:

```bash
olevba sample.docm
```

---

# Common Forensic Use Cases

## 1. Extract VBA Macros

```bash
olevba sample.docm
```

Displays all VBA macro code found inside the document.

---

## 2. Display VBA Source Code Only

```bash
olevba -c sample.docm
```

Shows clean VBA source code without additional information.

---

## 3. Decode Obfuscated Strings

```bash
olevba --decode sample.docm
```

Attempts to decode:

- Hex strings
    
- Base64 strings
    
- VBA encoded strings
    

---

## 4. Analyze All Files in a Directory

```bash
olevba /evidence/
```

Recursively scans Office files.

---

## 5. Recursive Directory Scan

```bash
olevba -r /evidence/
```

Useful during malware triage investigations.

---

## 6. Display Detailed Analysis

```bash
olevba -a sample.docm
```

Shows:

- AutoExec keywords
    
- Suspicious functions
    
- IOCs
    
- Encoded strings
    

---

## 7. Show Metadata

```bash
olevba --reveal sample.docm
```

Reveals hidden VBA strings and encoded content.

---

## 8. Output JSON

```bash
olevba --json sample.docm
```

Useful for automation and SIEM ingestion.

---

## 9. Quiet Mode

```bash
olevba -q sample.docm
```

Suppresses banner and non-essential output.

---

## 10. Verbose Mode

```bash
olevba -v sample.docm
```

Displays additional debugging information.

---

# Indicators Olevba Can Detect

## Auto-Execution Macros

Examples:

```vb
AutoOpen
Document_Open
Workbook_Open
Auto_Close
```

These execute automatically when a document is opened.

---

## Command Execution

Examples:

```vb
Shell()
CreateObject("Wscript.Shell")
```

Used to launch commands and malware.

---

## PowerShell Execution

Example:

```vb
Shell "powershell -enc ..."
```

Common malware delivery method.

---

## Download Functions

Examples:

```vb
URLDownloadToFile
XMLHTTP
WinHttpRequest
MSXML2.XMLHTTP
```

Used to download payloads from the internet.

---

## File Operations

Examples:

```vb
Open
Write
SaveAs
Kill
```

Used to manipulate files.

---

## Registry Modifications

Examples:

```vb
RegWrite
RegRead
```

Used for persistence.

---

## Obfuscation Techniques

Examples:

```vb
Chr()
ChrW()
StrReverse()
Base64
Hex Encoding
```

Used to hide malicious behavior.

---

# Typical Malware Analysis Workflow

## Step 1

Identify whether macros exist:

```bash
olevba sample.docm
```

## Step 2

Extract macro code:

```bash
olevba -c sample.docm
```

## Step 3

Decode obfuscation:

```bash
olevba --decode sample.docm
```

## Step 4

Extract Indicators of Compromise (IOCs)

Look for:

- URLs
    
- IP addresses
    
- Domains
    
- PowerShell commands
    
- Download locations
    

## Step 5

Pivot into malware analysis and threat intelligence.

---

# Useful Commands Summary

```bash
olevba sample.docm

olevba -c sample.docm

olevba -a sample.docm

olevba --decode sample.docm

olevba --json sample.docm

olevba -r /directory/

olevba -q sample.docm

olevba -v sample.docm

olevba --reveal sample.docm

olevba --help
```

---

# Why Olevba is Important in Forensics

Olevba helps investigators:

- Detect malicious Office documents.
    
- Identify phishing payloads.
    
- Extract VBA macros.
    
- Recover hidden commands.
    
- Detect PowerShell execution.
    
- Discover download URLs and C2 infrastructure.
    
- Identify persistence mechanisms.
    
- Generate indicators of compromise (IOCs).
    

It is one of the most commonly used tools in malware triage and forensic investigations involving Microsoft Office documents.