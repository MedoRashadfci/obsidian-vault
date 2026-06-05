### Sender Policy Framework (SPF)

Dmarcian, a platform dedicated to helping organizations protect email domains from phishing, defines the **Sender Policy Framework** ([**SPF**(opens in new tab)](https://dmarcian.com/what-is-spf/)) as follows:

“Sender Policy Framework (SPF) is used to authenticate the sender of an email. With an SPF record in place, Internet Service Providers can verify that a mail server is authorized to send email for a specific domain. An SPF record is a DNS TXT record containing a list of the IP addresses that are allowed to send email on behalf of your domain.”

Let's take a look at a workflow diagram for SPF

![A visual workflow diagram for the Sender Policy Framework (SPF) showing an email being sent, the recipient email server looking up the SPF record from the DNS server, and either authenticating it or rejecting it.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759814016781.svg)

So, in essence, when an email is sent, the receiving mail server checks the domain's SPF record to verify whether the sending server is authorized to send messages on behalf of that domain. The delivery of the email (intended action) is based on the result of the SPF record verification.

|Verification Result|Intended Action|
|---|---|
|Pass, Neutral, None|Accept (Allow and process the email)|
|SoftFail, PermError|Flag (Mark as suspicious but allow)|
|Fail, TempError|Reject (Immediately discard the email)|

## SPF Records

Let's take a look at a sample SPF record and break down its format. Further information on SPF Record Syntax can be found [here(opens in new tab)](https://dmarcian.com/spf-syntax-table/).

`v=spf1 ip4:127.0.0.1 include:_spf.google.com -all`

- `v=spf1` Signifies the start of the SPF record
- `ip4:127.0.0.1` Specifies which IP can send mail (IPv4 in this case)
- `include:_spf.google.com` Specifies which domain can send mail
- `-all` Non-authorized emails will be rejected

## Tools

The **[SPF Surveyor(opens in new tab)](https://dmarcian.com/spf-survey/)** tool from dmarcian enables us to gain a visual look at DNS records. It also helps ensure the record uses the correct syntax.

Let's take a look at **TryHackMe**'s SPF record as an example.

![A screenshot of tryhackme.com's SPF record taken from the demarcian SPF surveyor tool. The record shows the domains that are allowed to send emails on behalf of TryHackMe.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759814814615.svg)

You may notice that no IP addresses are visibly present in the main record, but three domains are listed. In this case, all IP addresses authorized by the domains will be recognized as legitimate senders.

- `_spf.google.com`
- `email.chargebee.com`
- `7168674.spf05.hubspotemail.net`

**Google Admin Toolbox** [**Messageheader**(opens in new tab)](https://toolbox.googleapps.com/apps/messageheader/) allows you to analyze delivery details using an email's full header. It shows various record results, including SPF. Check out the example below, which shows the SPF record as `softfail with IP Unknown!`. A `SoftFail` means the sending mail server is not listed as an authorized sender, but the receiving server will still accept the email and flag it as suspicious. In this case, the sending IP address could not be verified.

![The results of analyzing a mock email header using Google Admin Toolbox Messageheader in which the SPF record comes back as a softfail.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759814814606.svg)


Answer the questions below

Based on TryHackMe's SPF record above, how many domains are authorized to send email on its behalf?

3

What is the intended action of an email that returns a `SoftFail` verification result?

flag

-------
-----
### DomainKeys Identified Mail (DKIM)

**DomainKeys Identified Mail** (**[DKIM(opens in new tab)](https://dmarcian.com/what-is-dkim/)**) is defined as follows:

“DKIM stands for DomainKeys Identified Mail and is used for the authentication of an email that’s being sent. Like SPF, DKIM is an open standard for email authentication that is used for DMARC alignment. A DKIM record exists in the DNS, but it is more complex than SPF. DKIM’s advantage is that it can survive forwarding, which makes it superior to SPF and a foundation for securing your email.”

Having a look at the workflow diagram for DKIM

![A visual workflow diagram for the DomainKeys Identified Mail (DKIM) showing a private key signing an email message, the email server retrieving the public key from the DNS server, a key match, and delivery to the recipient's inbox.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759815078151.svg)

Let's break it down. When an email is sent, the sending mail server uses a private key to add a digital signature to the email. The receiving server retrieves the public key from the domain's DKIM record to verify that the message truly came from the domain. If the signature matches, the email is authentic; otherwise, it may be flagged or rejected.

## DKIM Records

Here is a sample DKIM record, along with its components.

`v=DKIM1; k=rsa; p=<public_key>`

- `v=DKIM1` Specifies the version of DKIM being used (optional)
- `k=rsa` The key type. The RSA encryption algorithm is standard
- `p=` This is the public key that will be matched to the private key to verify the DKIM signature

Note: _DKIM record formats vary and may include other tags depending on the mail provider or implementation._

Dmarcian has some great [resources(opens in new tab)](https://dmarcian.com/dkim-selectors/) if you wish to learn further about DKIM. You can also check out their [DKIM Record Checker(opens in new tab)](https://dmarcian.com/dkim-inspector/) and [Validator(opens in new tab)](https://dmarcian.com/dkim-validator/).

**Sample Spam Email Header**

The image below is a snippet of an email header that was marked as spam with a DKIM result of `permerror`, indicating a permanent failure in DKIM verification. This could be the result of an invalid signature, a missing or incorrect DNS record, a forwarding server making a modification, or a misconfiguration in DKIM setup.

**![A sample email header showing a dkim=permerror (no key for signature) to indicate that the DKIM check failed.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759815967486.svg)**

Answer the questions below

Based on the sample header above, what is the reason for the `permerror`?

no key for signature
----
---------
------
### Domain-Based Message Authentication, Reporting, and Conformance (DMARC)

**Domain-Based Message Authentication, Reporting, and Conformance** ([**DMARC**(opens in new tab)](https://dmarcian.com/getting-started-with-dmarc/)) is defined as follows: 

“DMARC, an open source standard, uses a concept called alignment to tie the result of two other open source standards,  SPF (a published list of servers that are authorized to send email on behalf of a domain) and DKIM (a tamper-evident domain seal associated with a piece of email), to the content of an email.”

This means that DMARC ensures the sender's domain matches the domains verified by SPF and DKIM. If the alignment fails, DMARC instructs the recipient server on how to handle the email based on a policy specified in the record.

## DMARC Records

Here is a breakdown of a DMARC record and its components. 

`v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com`

- `v=DMARC1`: The version of DMARC (required)
- `p=quarantine` The DMARC policy (quarantine = move to the spam folder)
- `rua=mailto:postmaster@website.com` An optional tag. In this case, aggregate reports will be sent to the email specified

Here is some [further reading(opens in new tab)](https://dmarcian.com/what-is-a-dmarc-record/) about DMARC records if you're interested in learning more. 

## Domain Checker

Another great [tool(opens in new tab)](https://dmarcian.com/domain-checker/) by dmarcian that inspects DMARC, SPF, and DKIM records to identify any issues. Let's try it out on `microsoft.com`.

In the screenshot below, you can see that `microsoft.com` passed all the checks. Clicking the details will allow you to examine each record and policy more closely. As shown in the record below, all emails that fail the DMARC check will be rejected based on the `p=reject` policy tag.

![The result of running microsoft.com against demarcian's domain checker. Microsoft.com passed the DMARC, DKIM, and SPF checks. The DMARC record details are expanded, showing the DMARC record.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1759817024989.svg)

Answer the questions below

Which DMARC policy provides the greatest amount of protection by blocking emails that fail the DMARC check?

***p=reject***

----
-----
### Secure/Multipurpose Internet Mail Extensions (S/MIME)

**Secure/Multipurpose Internet Mail Extensions** ([**S/MIME**(opens in new tab)](https://learn.microsoft.com/en-us/exchange/security-and-compliance/smime-exo/smime-exo)) is a standard protocol for sending digitally signed and encrypted messages. It is based on public key cryptography, where the private key is never shared and the public key can be distributed openly. The two main components and security features of S/MIME are:

**Digital Signature**

The sender signs the message with their private key, the recepient verifies the sender's identity using the sender's public key. This security feature provides:

- **Authentication**: Confirms the sender's identity through their digital certificate
- **Non-repudiation**: Ensures the sender cannot deny sending the message
- **Data Integrity**: Detects any changes to the message after it's signed

**Encryption**

The sender encrypts the message using the recipient's public key, allowing only the recipient to decrypt it with their private key. This security feature provides:

- **Confidentiality**: Keeps the content private and readable only by the intended recipient

## S/MIME Example

- Bob wishes to securely send the email with S/MIME to Mary
- Bob creates a digital certificate to generate the **Digital Signature**
- Bob "signs" the email message with his private key (**Digital Signature**)
- Bob openly shares his public key with Mary (**Digital Signature**)
- Bob also asks the public key of Mary to encrypt the email (**Encryption**)  
      
    
- Mary verifies Bob's message with Bob's public key (**Digital Signature**)
- Mary decrypts the received message with her private key (**Encryption**)
- To send a reply email, Mary performs the same procedure from the start
- Both Bob and Mary will now have each other's certificates for future correspondence

Check out the graphic below for a visual representation of how public key cryptography works.
  
![A visual flow of how encryption in public key cryptography works.](https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1766401669103.png)

Answer the questions below

Which S/MIME component ensures that only the intended recipient can read the contents of an email message?

### Encryption

----
----
