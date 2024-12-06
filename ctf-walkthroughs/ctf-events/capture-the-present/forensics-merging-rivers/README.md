# Description

We have noticed lately that some Pages in our Knowledge base have been altered and reflected bad practices! We have checked our Confluence users but we didn't find anything out of place... However, we did have a network capture somewhere that might be of help! Can you help us out by finding how the attacker managed to get access to our Knowledge base? Hopefully when found, we can fix it so it wont happen again!

# Approach

Extracted given zip file. Got capture.pcapng file

![](Pasted%20image%2020241202135547.png)

Checking http protocol as description says this is regarding confluence page.

![](Pasted%20image%2020241202141245.png)

After checking some packets, it was clear that attacker was abusing `CVE-2023-22527`. 
After going through all http packets lastly I found the packet where attacker was trying to get RCE.

![](Pasted%20image%2020241202141540.png)

After decoding the `jwt.private.key` I found the flag.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ echo SFRCe0FuMHRoM3JfZDR5XzRuMHRoM3JfYzBuZmx1M25jM19SQzMhIX0= | base64 -d
HTB{An0th3r_d4y_4n0th3r_c0nflu3nc3_RC3!!}
```