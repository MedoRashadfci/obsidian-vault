**Steghide** is a tool used to hide and extract secret data inside files like images or audio.  
It is commonly used in digital forensics and CTF challenges.

It supports formats like:

- JPG
    
- BMP
    
- WAV
    
- AU

## ✅ Why do we use Steghide?

Because it can:

- Hide a text or a file inside an image
    
- Extract hidden files from images
    
- Protect hidden data using a password

## 1. Extract hidden data

This is the most common use:

`steghide extract -sf image.jpg`

- `extract` = remove the hidden data
    
- `-sf` = stego file (the file that contains the secret)
    

If there is a password, it will ask for it.  
If no password, press **Enter**.

## 2. Hide a file inside an image

`steghide embed -cf picture.jpg -ef secret.txt`

- `-cf` = cover file (the normal picture)
    
- `-ef` = embedded file (the hidden file)
    

If you want a password:

`steghide embed -cf picture.jpg -ef secret.txt -p mypassword`

---

## 3. Extract with password

`steghide extract -sf picture.jpg -p mypassword`

---

# 📌 Extra useful commands

### Show info about the stego file

`steghide info picture.jpg`

Example output:

`embedded data: yes encryption: AES`

---

# 📁 Example Scenario

### Hide data:

`steghide embed -cf dog.jpg -ef flag.txt`

### Extract data:

`steghide extract -sf dog.jpg`

Result:

`wrote extracted file "flag.txt"`

Now open flag.txt and you see the secret.

