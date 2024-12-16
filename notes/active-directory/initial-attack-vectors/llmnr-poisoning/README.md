![](assets/Pasted%20image%2020241216164444.png)

# [What's LLMNR?]()

**LLMNR** stands for **Link-Local Multicast Name Resolution**. This is used if the DNS is not in use.  It is a protocol used on local networks to resolve the names of computers (hostnames) to their respective IP addresses without requiring a DNS server. LLMNR operates on the local link (Layer 2) and uses multicast communication to perform name resolution. It is primarily used in small networks or environments where a DNS server is unavailable. However LLMNR can pose security risks: It is vulnerable to **man-in-the-middle (MITM)** attacks and spoofing.

# [LLMNR Poisoning]()

In this attacker tricks a computer on a local network into sending sensitive information, like **usernames** and **password hashes**, to the attacker's system instead of the intended destination.

## [How does it work?]()

-  **LLMNR request**: When a computer on the network cannot resolve a hostname (e.g., "FileServer") via DNS, it broadcasts an LLMNR request, asking: _"Does anyone here know the IP for 'FileServer'?"_

![](assets/Pasted%20image%2020241216165512.png)

-  **Attacker responds**: An attacker intercepts this request and replies falsely: _"Yes, I am 'FileServer'—here’s my IP address!"_

![](assets/Pasted%20image%2020241216165958.png)

-  **Victim connects**: The victim connects to the attacker's machine, thinking it's the legitimate server.

![](assets/Pasted%20image%2020241216170210.png)

-  **Password hash theft**: During this connection attempt, the victim's computer automatically sends its credentials (in hashed form) to authenticate. The attacker can capture these password hashes and attempt to crack them offline.

![](assets/Pasted%20image%2020241216170234.png)

