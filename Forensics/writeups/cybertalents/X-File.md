### Challenge Overview

The challenge provides a ZIP file called **lost-backup.zip** which appears to contain important data. However, the file is password-protected, and the password is unknown.

---

### 🔍 Investigation Steps

1. **Identify the file type**
    
    `file lost-backup.zip`
    
    This confirmed that the file is a valid ZIP archive.
    
2. **Attempt to extract the archive**
    
    `unzip lost-backup.zip`
    
    The extraction failed because the archive is protected with a password.
    
3. **Crack the ZIP password**  
    A dictionary attack was performed using **fcrackzip** with the famous **rockyou.txt** wordlist:
    
    `fcrackzip -D -u -p /usr/share/wordlists/rockyou.txt lost-backup.zip`
    
    The correct password was successfully recovered.
    
4. **Extract the contents**
    
    `unzip lost-backup.zip`
    
5. **Read the extracted file**
    
    `ls cat presentation.txt`
    
    Inside the text file, the flag was found.
    

---

### 🚩 Flag

`flag{Pa55w0rd_Cracking_is_3asy}`

---

### ✅ Conclusion

This challenge demonstrates how weak passwords can be easily cracked using dictionary attacks. It also highlights the importance of securing backup files, as they are often overlooked but can contain sensitive information.