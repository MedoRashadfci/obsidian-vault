
Network Security Challenge Write-Up

## Challenge Overview

Lazania is a network security challenge from CyberTalents that focuses on **network traffic analysis using Wireshark and tshark**.  
The goal is to extract hidden data exfiltrated over HTTP traffic and recover the final flag.

The attacker used **covert channels** inside normal-looking HTTP requests to hide:

1. An encrypted ZIP file.
    
2. The ZIP file password.
    

---

## Step 1: Initial Traffic Inspection

After opening `Lazania.pcapng`, the traffic immediately shows:

- A large number of repeated HTTP requests
    
- Almost no HTTP responses
    
- Similar structure across packets
    

This pattern indicates **intentional data transfer**, not normal web browsing.

---

## Step 2: Extract Destination IP Addresses (Hidden Password)

The first suspicious pattern appears in the **destination IP addresses**.

Each HTTP request is sent to a different IP, but the change happens only in the **last octet** of the destination IP.

### Command used:

`tshark -r Lazania.pcapng -Y "http.request" -T fields -e ip.dst | awk -F. '{print $4}'`

### Explanation:

- `-r Lazania.pcapng` → read the capture file
    
- `-Y "http.request"` → filter only HTTP requests
    
- `-T fields -e ip.dst` → extract destination IP addresses
    
- `awk -F. '{print $4}'` → extract the last octet
    

### Result:

A sequence of numbers such as:

`102 108 97 103 ...`

These values represent **ASCII decimal codes**.

When converted to ASCII characters, they form a readable string, which is used as the **ZIP file password**.

---

## Step 3: Extract Hidden Payload from TCP Segment Data

The second covert channel is hidden inside the **TCP segment data** of HTTP packets.

### Command used:

`tshark -r Lazania.pcapng -Y 'http' -T fields -e tcp.segment_data | tr "\n" " "`

### Explanation:

- Extracts raw TCP payload data from HTTP packets
    
- Joins all payloads into one continuous stream
    

The extracted data appears encoded and unreadable at first glance.

---

## Step 4: Identify the Extracted File

After saving the extracted data into a file (e.g. `download.zip`), the file type is checked.

### Command:

`file download.zip`

### Result:

The output indicates that the file is a **ZIP archive**, even though it does not behave like a normal ZIP file.

---

## Step 5: Attempt to Extract the ZIP File

A normal unzip attempt fails:

`unzip download.zip`

This happens because:

- The ZIP file is encrypted
    
- The ZIP structure is slightly non-standard
    

---

## Step 6: Use the Extracted Password

The password recovered earlier from the destination IP addresses is then used to extract the ZIP file successfully (using a compatible tool such as `7z` if needed).

Inside the archive, the file `flag.txt` is found, which contains the challenge flag.