In the previous two rooms of the [Phishing Analysis module](https://tryhackme.com/module/phishing), you explored the fundamentals of email header and body analysis, along with how to investigate links and attachments by examining the email source code. In this room, you will build on that foundation by learning about the tools that enable deeper analysis, then apply your skills by investigating three real-world phishing cases.

## Objectives

- Understand what information should be collected during an email analysis
- Learn about the tools that enable effective email investigation
- Explore IP and URL analysis, including reputation lookups
- Investigate phishing cases to apply your understanding and strengthen your analysis skills

## Prerequisites

- Check out [Phishing Analysis Fundamentals](https://tryhackme.com/room/phishingemails1tryoe) for an overview of email communications and analysis
- Cover [Phishing Emails in Action](https://tryhackme.com/room/phishingemails2rytmuv) to gain experience analyzing phishing emails


-------
-----
### Identifying Artifacts

When analyzing a suspicious or malicious email, your first goal is to collect key artifacts that can help determine its legitimacy and intent. These artifacts provide the foundation for deeper investigation, such as reputation checks, threat intelligence lookups, and behavioral analysis.

## Header Artifacts

When you begin your analysis with the email header, you should take note of the following points: 

- **Sender email address:** Where did the email originate from?
- **Sender IP address:** What is the source IP, and what does a reverse lookup reveal?
- **Email subject line:** Does it contain urgency or a call to action?
- **Recipient email address:** Who is the intended recipient (To/CC/BCC)?
- **Reply-To email address:** Where are responses being directed?
- **Date and time:** When was the email sent?

## Body Analysis

Next comes the email body, which has its own set of artifacts to be aware of: 

- **URLs and hyperlinks:** Identify all links and expand shortened URLs to reveal their true destination
- **Attachment name(s):** What files are included, and do their names or extensions appear suspicious
- **Attachment hash:** Generate a hash value for threat intelligence lookups

![A mock phishing email designed to show the various artifacts to be aware of when analyzing potentially suspicious emails.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774336189276.svg)


-----
----
### Email Header Analysis

Some of the key information we need to collect can be viewed directly within an email or web client, such as Gmail or Yahoo!. However, other details, such as the sender’s IP address and Reply-To information, can only be obtained from the email header. In [Phishing Analysis Fundamentals](https://tryhackme.com/room/phishingemails1tryoe), we explored how to manually extract this data by analyzing the email’s source code. Now, we will look at tools that can help streamline and automate this process.

## Mail Header Analysis

[Messageheader(opens in new tab)](https://toolbox.googleapps.com/apps/messageheader/analyzeheader), part of the Google Admin Toolbox, helps analyze email headers. By simply pasting the full header into the tool, you can quickly extract key details such as the sender’s IP address, routing path, and potential misconfigurations.

![An email opened within Google's Messageheader tool analyzing the email.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774336606174.png)

Another great tool, [Message Header Analyzer(opens in new tab)](https://mha.azurewebsites.net/), can perform the same type of analysis. Check it out in the screenshot below.

![A email open in the Message Header Analyzer tool.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774352885892.png)

## IP and URL Reputation Analysis

When investigating suspected phishing emails, it is important to determine the origin of any identified IP addresses and assess the reputations of any associated IPs or URLs. These indicators can reveal whether the infrastructure is legitimate or tied to known malicious activity. Fortunately, several free tools are available that allow analysts to quickly perform these checks and gain valuable insight into potential threats.

[IPinfo(opens in new tab)](https://ipinfo.io/) is a simple and effective tool for gathering information about an IP address. By entering an IP, you can quickly view details such as its geographic location and associated organization. This helps analysts determine whether an IP is potentially malicious.

![The IPinfo homepage showing a sample IP address analysis.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774336608146.png)

[URLScan.io(opens in new tab)](https://urlscan.io/) is a tool that enables analysts to safely investigate websites without visiting them directly. By submitting a URL, the service simulates a real user browsing session and records all activity generated by the page. It also captures a screenshot of the site and provides insight into its behavior, helping analysts quickly identify suspicious or potentially malicious content.

![The URLScan URL analysis tool with the website capitai-one.com being analyzed.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774336606250.png)

Talos [IP & Domain Reputation Center (opens in new tab)](https://talosintelligence.com/reputation_center/)is a threat intelligence tool from Cisco that enables analysts to assess the reputation of IP addresses, domains, and networks. By submitting an indicator, you can quickly assess whether it has been associated with malicious activity and view its classification.

![The Talos IP & Domain Reputation Center website with an IP address being analyzed.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774336606179.png)

Answer the questions below

Use [Talos Reputation Center (opens in new tab)](https://talosintelligence.com/reputation_center/) to look up `malware-test.com`.  
What content category is listed for the domain?

Computer Security
-----
-----
----
### Email Body Analysis

Now it’s time to shift your focus to the email body, where the true intent of a phishing message is often revealed. This is typically where the malicious payload is delivered, either as a hyperlink designed to lure users to a phishing site or as an attachment intended to compromise the system. During analysis, links can be extracted manually from the visible email content or by examining the underlying HTML and raw source to uncover hidden or obfuscated URLs.

## Mail Body Analysis

Determining the destination of a link within an email can be as easy as right-clicking the link and selecting `Copy link address` (the option's name may vary depending on your browser or email provider). From here, you can safely assess the URL without actually following it to the destination page.

![A phishing email highlighting a call-to-action button and the actual destination URL](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774354137498.svg)

Another effective way to identify URLs in an email is to use a [URL extraction tool(opens in new tab)](https://www.convertcsv.com/url-extractor.htm). These tools let you paste raw email content and automatically parse all embedded links, saving time and reducing the risk of missing hidden or obfuscated URLs. Tools like [CyberChef(opens in new tab)](https://gchq.github.io/CyberChef/#recipe=Extract_URLs\(false,false,false\)) can also perform this function, making them a versatile option for email analysis.

![The URL Extractor website showing the uploaded message header and extracted URLs.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774354137496.png)

**Email Attachments**

If the email you are investigating contains an attachment, it may be tempting to download it directly using your email client. However, attachments should only be downloaded in a controlled environment, such as a virtual machine or sandbox, to reduce the risk of accidental execution. Once safely obtained, you can use tools like the `sha256sum` command in a Linux environment to generate a hash value for further analysis and reputation checks.

Obtain SHA256 Hash

```shell-session
user@tryhackme$ sha256sum shady_attachment.pdf
025ba9ce4a2118a9ca7b115c8869ff73bc16bad3732ba359cef1e60ad8f961f9 shady_attachment.pdf
```

Once you've obtained the hash value, several sites can safely assist with analyzing the attached file. Let's return to Talos [IP & Domain Reputation Center(opens in new tab)](https://talosintelligence.com/reputation_center/) from the previous task and analyze the hash value from above. In this case, the file is labeled as _Phishing_, _Malicious_, and _Spam_, indicating that this file is likely harmful.

![The Talos reputation center and analysis of the file hash from the previous portion of the task.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774337125465.png)

[VirusTotal(opens in new tab)](https://www.virustotal.com/gui/) is a widely used tool that allows analysts to check the reputation of files, URLs, IP addresses, and domains using data from dozens of security vendors. By submitting a file or indicator, you can quickly determine whether it has been flagged as malicious and review detailed detection results. VirusTotal also allows users to upload files and submit URLs for analysis, making it a powerful resource for investigating suspicious artifacts.

![The VirusTotal hash analysis of the file from the previous portion of the task.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774337125351.png)

Answer the questions below

What command can you use in a Linux environment to obtain the `SHA256` hash value of an attachment?

sha256sum
----
--------------
-----
### Malware Sandboxes

Fortunately, as defenders, we don’t need advanced malware analysis skills to fully understand a malicious attachment. Instead, we can use online tools and services known as malware sandboxes, which allow files to be uploaded and analyzed in a controlled environment. This enables us to safely observe the behavior of a potentially malicious file without risking our own systems. For example, by uploading an attachment from a suspicious email, we can identify the URLs it attempts to contact, any additional payloads it downloads, and other indicators of compromise (IOCs).

**ANYRUN**

[ANY.RUN(opens in new tab)](https://app.any.run/) is an interactive malware sandbox that allows analysts to safely execute and observe suspicious files and URLs in real time. Unlike traditional sandboxes, it provides a hands-on experience in which you can interact with the environment, monitor processes, view network activity, and analyze system changes as they happen. This makes it a powerful tool for understanding how malware behaves and identifying key indicators of compromise in a controlled setting.

![The ANYRUN homepage showing the submit file, email. URL, and check suspicious links options.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774337451514.png)

**Hybrid Analysis**

Similar to the tool above, [Hybrid Analysis(opens in new tab)](https://hybrid-analysis.com/) is a free malware analysis sandbox that allows analysts to upload and examine suspicious files in a controlled environment. It provides detailed insights into file behavior, including system changes, network activity, and indicators of compromise, helping analysts better understand the potential impact of a malicious file.

![The Hybrid Analysis homepage showing the File/URL upload section.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774337450417.png)

**JOESandbox**

JOESecurity created the [JOESandbox(opens in new tab)](https://www.joesandbox.com/) for advanced malware analysis. It performs both static and dynamic analysis on suspicious files and URLs. It generates comprehensive reports that highlight behavior, indicators of compromise, and threat classifications.

![The JOESandbox homepage showing the file upload options.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1774337450437.png)

----
----
