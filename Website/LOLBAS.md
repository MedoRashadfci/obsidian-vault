https://lolbas-project.github.io/

What is LOLBAS?

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/aa5cd3489ac1c2a6c315d637bf4ba8ce.png)  

LOLBAS stands for **L**iving **O**ff the **L**and **B**inaries **A**nd **S**cripts, a project's primary main goal is to gather and document the Microsoft-signed and built-in tools used as  Living Off the Land techniques, including binaries, scripts, and libraries.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/c98596d3c51c192ae9fd415ff06fc6b9.png)  

The LOLBAS project is a community-driven repository gathering a collection of binaries, scripts, libraries that could be used for red team purposes. It allows to search based on binaries, functions, scripts, and ATT&CK info. The previous image shows what the LOLBAS project page looks like at this time. If you are interested in more details about the project, you may visit the project's website [here(opens in new tab)](https://lolbas-project.github.io/).

The LOLBAS website provides a convenient search bar to query all available data. It is straightforward to look for a binary; including the binary name will show the result. However, if we want to look for a specific function, we require providing a / before the function name. For example, if we are looking for all execute functions, we should use /execute. Similarly, in order to look based on types, we should use the # symbol followed by the type name. The following are the types included in the project:  

- Script
- Binary
- Libraries
- OtherMSBinaries

Tools Criteria  

Specific criteria are required for a tool to be a "Living Off the Land" technique and accepted as part of the LOLBAS project:  

- Microsoft-signed file native to the OS or downloaded from Microsoft.  
    
- Having additional interesting unintended functionality not covered by known use cases.  
    
- Benefits an APT (Advanced Persistent Threat) or Red Team engagement.  
    

  

Please note that if you find an exciting binary that adheres to the previously mentioned criteria, you may submit your finding by visiting the [GitHub repo contribution page(opens in new tab)](https://github.com/LOLBAS-Project/LOLBAS#criteria) for more information.  

  

Interesting Functionalities  

The LOLBAS project accepts tool submissions that fit one of the following functionalities:  

- Arbitrary code execution
- File operations, including downloading, uploading, and copying files.
- Compiling code
- Persistence, including hiding data in Alternate Data Streams (ADS) or executing at logon.  
    
- UAC bypass
- Dumping process memory
- DLL injection

-----
