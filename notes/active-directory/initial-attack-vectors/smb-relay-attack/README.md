
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

- Configure the responder for this attack (turn off SMB and HTTP)

```
sudo nano /etc/responder/Responder.conf
```
![](assets/Pasted%20image%2020241217204838.png)

- 