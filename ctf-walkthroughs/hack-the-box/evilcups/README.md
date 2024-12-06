# Enumeration

Checking what ports are open

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ sudo nmap -p- -sS -T5 -r -v evilcups.htb | grep open
Discovered open port 22/tcp on 10.10.11.40
Discovered open port 631/tcp on 10.10.11.40
```

Getting more information on port 22 and 631

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ nmap -sCV -p22,631 evilcups.htb -T5
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-01 23:00 IST
Nmap scan report for evilcups.htb (10.10.11.40)
Host is up (0.31s latency).

PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 36:49:95:03:8d:b4:4c:6e:a9:25:92:af:3c:9e:06:66 (ECDSA)
|_  256 9f:a4:a9:39:11:20:e0:96:ee:c4:9a:69:28:95:0c:60 (ED25519)
631/tcp open  ipp     CUPS 2.4
|_http-title: Bad Request - CUPS v2.4.2
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 75.59 seconds
```

