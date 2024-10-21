![](banner.png)
###### [more](https://tryhackme.com/r/room/relevant)

# [Recon]()

- nmap scan

> nmap -sCSV -T4 $ip

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-21 19:28 AEDT
Nmap scan report for 10.10.185.191 (10.10.185.191)
Host is up (0.31s latency).
Not shown: 995 filtered tcp ports (no-response)
PORT     STATE SERVICE        VERSION
80/tcp   open  http           Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp  open  msrpc          Microsoft Windows RPC
139/tcp  open  netbios-ssn    Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds   Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server?
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2024-10-20T08:26:35
|_Not valid after:  2025-04-21T08:26:35
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-10-21T01:30:06-07:00
|_clock-skew: mean: 2h20m00s, deviation: 4h02m31s, median: 0s
| smb2-time: 
|   date: 2024-10-21T08:30:08
|_  start_date: 2024-10-21T08:26:36
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 156.27 seconds
```

- Searching file/dirs on webserver - files not found
- Interacting with sbmserver as there is no password (understood from nmap recon)

> smbclient -L \\\\$ip -U guest 

```
Password for [WORKGROUP\guest]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	nt4wrksv        Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.185.191 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

> smbclient \\\\\\\\$ip\\\\nt4wrksv -U guest 

```
Password for [WORKGROUP\guest]:
Try "help" to get a list of possible commands.
smb: \> l
  .                                   D        0  Mon Oct 21 19:48:19 2024
  ..                                  D        0  Mon Oct 21 19:48:19 2024
  passwords.txt                       A       98  Sun Jul 26 01:15:33 2020

		7735807 blocks of size 4096. 4945380 blocks available
smb: \> get passwords.txt 
getting file \passwords.txt of size 98 as passwords.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
```

- checking passwords.txt file

> cat passwords.txt  

```
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk  
```

- decoding via base64

> echo Qm9iIC0gIVBAJCRXMHJEITEyMw== |  base64 -d 

```
Bob - !P@$$W0rD!123 
```

> echo QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk |  base64 -d 

```
Bill - Juw4nnaM4n420696969!$$$   
```

- Tried password to smb and rdp but didn't work.
-  looking for vulnerabilities

# [Vulnerability Discovery](#)

- using nmap scripts for vulnerability scanning

> nmap --script vuln $ip 

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-21 20:58 AEDT
Nmap scan report for relevant.thm (10.10.183.232)
Host is up (0.35s latency).
Not shown: 995 filtered tcp ports (no-response)
PORT     STATE SERVICE
80/tcp   open  http
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-aspnet-debug: ERROR: Script execution failed (use -d to debug)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-vuln-cve2014-3704: ERROR: Script execution failed (use -d to debug)
|_http-dombased-xss: Couldn't find any DOM based XSS.
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server

Host script results:
|_smb-vuln-ms10-054: false
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
|_      https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_smb-vuln-ms10-061: ERROR: Script execution failed (use -d to debug)

Nmap done: 1 IP address (1 host up) scanned in 111.02 seconds
```

- learning about the CVE-2017-0143


