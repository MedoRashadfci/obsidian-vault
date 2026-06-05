to get the password we want to crack the file using a tool called: **Aircrack-ng**

Aircrack-ng is a complete suite of tools to assess WiFi network security.

It focuses on different areas of WiFi security:

- Monitoring: Packet capture and export of data to text files for further processing by third party tools
- Attacking: Replay attacks, deauthentication, fake access points and others via packet injection
- Testing: Checking Wi-Fi cards and driver capabilities (capture and injection)
- Cracking: WEP and WPA PSK (WPA 1 and 2)

to use it we will need a wordlist and the most common wordlist is **rockyou**

usage:

`aircrack-ng authed.cap -w /usr/share/wordlists/rockyou.txt`