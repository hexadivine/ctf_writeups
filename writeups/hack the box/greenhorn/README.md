# Sea

![](Pasted%20image%2020241111191207.png)

## Enumeration

- nmap scan

> sudo nmap -sCSV greenhorn.htb

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-11 19:30 AEDT
Nmap scan report for greenhorn.htb (10.10.11.25)
Host is up (0.033s latency).
Other addresses for greenhorn.htb (not scanned): 10.10.11.25
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 57:d6:92:8a:72:44:84:17:29:eb:5c:c9:63:6a:fe:fd (ECDSA)
|_  256 40:ea:17:b1:b6:c5:3f:42:56:67:4a:3c:ee:75:23:2f (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/data/ /docs/
3000/tcp open  ppp?
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Content-Type: text/html; charset=utf-8
|     Set-Cookie: i_like_gitea=4590d6d975c07bd5; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=M04J2ykt9B2tK069Jrn6jAEgem06MTczMTMxMzgwNzk3NTIzODQwMw; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Mon, 11 Nov 2024 08:30:07 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-auto">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>GreenHorn</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR3JlZW5Ib3JuIiwic2hvcnRfbmFtZSI6IkdyZWVuSG9ybiIsInN0YXJ0X3VybCI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOiJpbWFnZS9wbmciLCJzaXplcyI6IjUxMng1MTIifSx7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvYX
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=9aa2c5bf411e46bb; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=NLA28QaUuEkcmvmYE9vYvhu9Wuk6MTczMTMxMzgxMzQyMzA2NzU5MQ; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Mon, 11 Nov 2024 08:30:13 GMT
|_    Content-Length: 0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.94SVN%I=7%D=11/11%Time=6731C098%P=x86_64-pc-linux-gnu%
SF:r(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\
SF:x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20B
SF:ad\x20Request")%r(GetRequest,37DB,"HTTP/1\.0\x20200\x20OK\r\nCache-Cont
SF:rol:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nC
SF:ontent-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_gi
SF:tea=4590d6d975c07bd5;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Co
SF:okie:\x20_csrf=M04J2ykt9B2tK069Jrn6jAEgem06MTczMTMxMzgwNzk3NTIzODQwMw;\
SF:x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-Op
SF:tions:\x20SAMEORIGIN\r\nDate:\x20Mon,\x2011\x20Nov\x202024\x2008:30:07\
SF:x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"th
SF:eme-auto\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"width=de
SF:vice-width,\x20initial-scale=1\">\n\t<title>GreenHorn</title>\n\t<link\
SF:x20rel=\"manifest\"\x20href=\"data:application/json;base64,eyJuYW1lIjoi
SF:R3JlZW5Ib3JuIiwic2hvcnRfbmFtZSI6IkdyZWVuSG9ybiIsInN0YXJ0X3VybCI6Imh0dHA
SF:6Ly9ncmVlbmhvcm4uaHRiOjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly9ncmVlbm
SF:hvcm4uaHRiOjMwMDAvYXNzZXRzL2ltZy9sb2dvLnBuZyIsInR5cGUiOiJpbWFnZS9wbmciL
SF:CJzaXplcyI6IjUxMng1MTIifSx7InNyYyI6Imh0dHA6Ly9ncmVlbmhvcm4uaHRiOjMwMDAv
SF:YX")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20
SF:text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\
SF:x20Request")%r(HTTPOptions,197,"HTTP/1\.0\x20405\x20Method\x20Not\x20Al
SF:lowed\r\nAllow:\x20HEAD\r\nAllow:\x20GET\r\nCache-Control:\x20max-age=0
SF:,\x20private,\x20must-revalidate,\x20no-transform\r\nSet-Cookie:\x20i_l
SF:ike_gitea=9aa2c5bf411e46bb;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\n
SF:Set-Cookie:\x20_csrf=NLA28QaUuEkcmvmYE9vYvhu9Wuk6MTczMTMxMzgxMzQyMzA2Nz
SF:U5MQ;\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Fr
SF:ame-Options:\x20SAMEORIGIN\r\nDate:\x20Mon,\x2011\x20Nov\x202024\x2008:
SF:30:13\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,67,"HTTP/1
SF:\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/plain;\x20charset
SF:=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20Request");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 141.30 seconds
```

- checking <http://greenhorn.htb/?file=welcome-to-greenhorn>

![](Pasted%20image%2020241111201258.png)

- checking login page on <http://greenhorn.htb/login.php>

![](Pasted%20image%2020241111201427.png)

- checking <http://greenhorn.htb:3000>

![](Pasted%20image%2020241111194432.png)

![](Pasted%20image%2020241111194418.png)

- Found the source code at <http://greenhorn.htb:3000/GreenAdmin/GreenHorn>

![](Pasted%20image%2020241111194458.png)

## Vulnerability Discovery

- Found interesting code at <http://greenhorn.htb:3000/GreenAdmin/GreenHorn/src/branch/main/login.php>

![](Pasted%20image%2020241111201634.png)

- Found value of $ww at <http://greenhorn.htb:3000/GreenAdmin/GreenHorn/src/branch/main/data/settings/pass.php>

![](Pasted%20image%2020241111201930.png)

- Cracked the sha512 hash from <https://crackstation.net/> (value: iloveyou1)

![](Pasted%20image%2020241111202121.png)

- filling the password at login page <http://greenhorn.htb/login.php>

![](Pasted%20image%2020241111202219.png)
![](Pasted%20image%2020241111202240.png)

- Searching for an exploit for [pluck 4.7.18](http://www.pluck-cms.org)
- found [CVE-2023-50564](https://nvd.nist.gov/vuln/detail/CVE-2023-50564): allows arbitrary file upload

## #Exploit

- Uploading zip file contain php rev shell

![](Pasted%20image%2020241111204528.png)![](Pasted%20image%2020241111204651.png)

- got reverse shell after uploading module

![](Pasted%20image%2020241111204732.png)

- checked junior user
- used same password (iloveyou1) to login to "junior" user

![](Pasted%20image%2020241111204937.png)

- got user flag

![](Pasted%20image%2020241111205118.png)

## Privilege Escalation

- checking pdf file - "Using OpenVAS.pdf"
- starting python webserver to download file

![](Pasted%20image%2020241111205650.png)

![](Pasted%20image%2020241111205728.png)

- need to de-pixalise password
- extracting de-pixalised image from pdf

![](0.png)

- using depix tool - <https://github.com/spipm/Depix> (stenography tool to discover blurred text)

> python3 depix.py -p ~/0.png -s images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png -o depixed.png

```
2024-11-11 21:03:21,505 - Loading pixelated image from /home/lnxuser/0.png
2024-11-11 21:03:21,555 - Loading search image from images/searchimages/debruinseq_notepad_Windows10_closeAndSpaced.png
2024-11-11 21:03:22,079 - Finding color rectangles from pixelated space
2024-11-11 21:03:22,080 - Found 252 same color rectangles
2024-11-11 21:03:22,080 - 190 rectangles left after moot filter
2024-11-11 21:03:22,080 - Found 1 different rectangle sizes
2024-11-11 21:03:22,080 - Finding matches in search image
2024-11-11 21:03:22,080 - Scanning 190 blocks with size (5, 5)
2024-11-11 21:03:22,103 - Scanning in searchImage: 0/1674
2024-11-11 21:04:01,936 - Removing blocks with no matches
2024-11-11 21:04:01,936 - Splitting single matches and multiple matches
2024-11-11 21:04:01,939 - [16 straight matches | 174 multiple matches]
2024-11-11 21:04:01,939 - Trying geometrical matches on single-match squares
2024-11-11 21:04:02,180 - [29 straight matches | 161 multiple matches]
2024-11-11 21:04:02,180 - Trying another pass on geometrical matches
2024-11-11 21:04:02,395 - [41 straight matches | 149 multiple matches]
2024-11-11 21:04:02,395 - Writing single match results to output
2024-11-11 21:04:02,396 - Writing average results for multiple matches to output
2024-11-11 21:04:05,630 - Saving output image to: depixed.png
```

- got depixed image. 
- got root password: "sidefromsidetheothersidesidefromsidetheotherside"

![](depixed.png)

# Personal Learnings

- After finding exploit, don't just rely on exploit script, but understand its working and modify the methodology accordingly. Recon more on other found ports.