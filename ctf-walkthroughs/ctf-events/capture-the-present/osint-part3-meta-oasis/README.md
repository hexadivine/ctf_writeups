# Description

Were you aware that software used to create documents automatically includes a type of information known as metadata, which can be described as data describing data?

# Attempt

Searching on google found [this ](https://www.scribd.com/document/747494457/Samantha-Zephyr-Williams-CV)pdf file. Found her blog page [here](https://sam4nexpl01t3r.blogspot.com/2023/02/find-my-cv-here.html) and the cv uploaded [here](https://drive.google.com/file/d/1n0kXyt49Qxf-jdGB7CSgP6wlIJKhz9Pj/view). After downloading CV I ran exiftool that gave the flag

```
┌──[hexadivine@hackthebox]─[~/ctf/htb]
└──╼ $ exiftool Samantha\ “Zephyr”\ Williams\ -\ CV.pdf 
ExifTool Version Number         : 12.57
File Name                       : Samantha “Zephyr” Williams - CV.pdf
Directory                       : .
File Size                       : 50 kB
File Modification Date/Time     : 2024:12:02 15:51:57+05:30
File Access Date/Time           : 2024:12:02 15:51:58+05:30
File Inode Change Date/Time     : 2024:12:02 15:54:38+05:30
File Permissions                : -rw-r--r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
Linearized                      : No
Page Count                      : 2
PDF Version                     : 1.5
Create Date                     : 0000:01:01 00:00:00
Producer                        : 
Title                           : 
Author                          : sam4nexpl01t3r
Creator                         : 
Modify Date                     : 0000:01:01 00:00:00
Flag                            : HTB{met4d4t4_m1ghT_3xp053_1nf0}
Subject                         : 
```
