
![](Pasted%20image%2020241112023907.png)

# Introduction

SSRF stands for server side request forgery in which attacker makes modified requests from the backend of the server to  any (internal/external) resource to get more or secret information. There are two types of SSRF

- Regular SSRF
	- Response is returned to attacker
- Blind SSRF
	- No response is provided to the attacker

# Example 

Below is the workflow of SSRF.

![](Pasted%20image%2020241112025440.png)

In below example <https://website.thm/item/2?server=> the server query parameter takes the values that used as a subdomain.

![](Pasted%20image%2020241112030409.png)
![](Pasted%20image%2020241112030514.png)

This can be exploited by requesting to different domain with the help of server parameter.

![](Pasted%20image%2020241112030750.png)

In above example server parameter take the value "server.website.thm/flag?id=9&idc=" and use it as a subdomain, making the request <https://server.website.thm/flag?id=9&idc=.website.thm/api/item?id=2>  (where idc part is ignored)

![](Pasted%20image%2020241112031057.png)

# Discovery

Some possible discovery vectors include (but not limited to) below

![](Pasted%20image%2020241112031707.png)

# References 

- <https://tryhackme.com/r/room/ssrfqi>

