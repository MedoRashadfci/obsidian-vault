**ExifTool** is a command-line tool used to read, write, and edit metadata in files.

## What is metadata?

Metadata = information about a file.

Example:

- Camera used
    
- Author name
    
- GPS coordinates
    
- Creation date
    
- Software used
    
- Hidden comments
    

ExifTool works with many files:

- Images (JPG, PNG)
    
- PDF
    
- Video
    
- Audio
    

---

# ⭐ Why we use ExifTool in forensics?

Because hidden information or CTF flags are often inside metadata.

Example:

- Hidden message in the “Comment” field
    
- Flag in the “Author” tag
    

---

# ✔️ Basic Commands

## 1. Show metadata of a file

`exiftool image.jpg`

Output example:

`Author            : admin Comment           : Secret flag inside! Software          : Photoshop`

---

## 2. Show specific field only

`exiftool -Comment image.jpg`

Another example:

`exiftool -Author image.jpg`

---

## 3. Remove metadata

`exiftool -all= image.jpg`

(Removes all metadata)

---

## 4. Add metadata

`exiftool -Comment="Hello world" image.jpg`

---

## 5. Save output into a file

`exiftool image.jpg > output.txt`