![](846c4a9abc7e7d6b2f564c685fe98336.png)

## [Introduction]()

**Active recon** requires you to interact with target to gather the information. This contact can be a phone call or a visit to the target company under some pretence to gather more information, usually as part of social engineering. Alternatively, it can be a direct connection to the target system, whether visiting their website or checking if their firewall has an SSH port open. Think of it like you are closely inspecting windows and door locks. Hence, it is essential to remember not to engage in active reconnaissance work before getting signed legal authorisation from the client.

## [Common Tools]()
### [Web-browser]()

There are also plenty of addons for Firefox and Chrome that can help in penetration testing. Here are a few examples:

- **Developer tools** lets you inspect many things that your browser has received and exchanged with the remote server.
- **FoxyProxy** lets you quickly change the proxy server you are using to access the target website. This browser extension is convenient when you are using a tool such as Burp Suite or if you need to switch proxy servers regularly. You can get FoxyProxy for Firefox from [here](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard).
- **User-Agent Switcher and Manager** gives you the ability to pretend to be accessing the web-page from a different operating system or different web browser. In other words, you can pretend to be browsing a site using an iPhone when in fact, you are accessing it from Mozilla Firefox. You can download User-Agent Switcher and Manager for Firefox [here](https://addons.mozilla.org/en-US/firefox/addon/user-agent-string-switcher).
- **Wappalyzer** provides insights about the technologies used on the visited websites. Such extension is handy, primarily when you collect all this information while browsing the website like any other user. A screenshot of Wappalyzer is shown below. You can find Wappalyzer for Firefox [here](https://addons.mozilla.org/en-US/firefox/addon/wappalyzer).

### [ping]()

`ping` is used to check network connectivity. It sends an ICMP Echo packet to a remote system. If the remote system is online, and the ping packet was correctly routed and not blocked by any firewall, the remote system should send back an ICMP Echo Reply. Similarly, the ping reply should reach the first system if appropriately routed and not blocked by any firewall.

```
$ ping -c 5 10.10.62.190

PING 10.10.62.190 (10.10.62.190) 56(84) bytes of data.
64 bytes from 10.10.62.190: icmp_seq=1 ttl=64 time=0.636 ms
64 bytes from 10.10.62.190: icmp_seq=2 ttl=64 time=0.483 ms
64 bytes from 10.10.62.190: icmp_seq=3 ttl=64 time=0.396 ms
64 bytes from 10.10.62.190: icmp_seq=4 ttl=64 time=0.416 ms
64 bytes from 10.10.62.190: icmp_seq=5 ttl=64 time=0.445 ms

--- 10.10.62.190 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4097ms
rtt min/avg/max/mdev = 0.396/0.475/0.636/0.086 m
```

### [Traceroute]() 

`Traceroute` is a network diagnostic tool that shows the path data takes from one device to another over the internet, identifying each hop and measuring the time it takes to reach each point.

```
$ traceroute google.com

traceroute to google.com (142.250.76.174), 30 hops max, 60 byte packets
 1  _gateway (192.168.0.1)  0.930 ms  1.378 ms  1.381 ms
 2  192.168.1.1 (192.168.1.1)  3.044 ms  4.044 ms  5.284 ms
 3  183.87.210.10.broad-band.jprdigital.in (183.87.210.10)  30.308 ms  30.878 ms  33.311 ms
 4  * * *
 5  10.20.20.17 (10.20.20.17)  35.245 ms  35.431 ms  35.653 ms
 6  10.20.20.5 (10.20.20.5)  35.144 ms  36.137 ms  36.321 ms
 7  72.14.209.97 (72.14.209.97)  36.894 ms  7.371 ms  26.403 ms
 8  192.178.110.123 (192.178.110.123)  28.072 ms  28.044 ms 192.178.110.221 (192.178.110.221)  28.751 ms
 9  216.239.46.137 (216.239.46.137)  30.186 ms 74.125.253.165 (74.125.253.165)  30.123 ms 216.239.46.137 (216.239.46.137)  31.537 ms
10  bom12s09-in-f14.1e100.net (142.250.76.174)  32.646 ms  32.942 ms  33.265 ms
```

### [telnet]()

`telnet` uses the TELNET protocol for remote administration. The default port used by telnet is 23. From a security perspective, `telnet` sends all the data, including usernames and passwords, in clear text. Sending in clear text makes it easy for anyone, who has access to the communication channel, to steal the login credentials.

```
$ telnet 10.10.62.190 80

Trying 10.10.62.190...
Connected to 10.10.62.190.
Escape character is '^]'.
GET / HTTP/1.1
host: telnet

HTTP/1.1 200 OK
Server: nginx/1.6.2
Date: Tue, 17 Aug 2021 11:13:25 GMT
Content-Type: text/html
Content-Length: 867
Last-Modified: Tue, 17 Aug 2021 11:12:16 GMT
Connection: keep-alive
ETag: "611b9990-363"
Accept-Ranges: bytes
```

### [netcat]()

`Netcat` (nc) is a versatile networking tool used for reading from and writing to network connections, useful for troubleshooting, security testing, and creating network-based applications.

```
$ nc 10.10.62.190 80

GET / HTTP/1.1
host: netcat

HTTP/1.1 200 OK
Server: nginx/1.6.2
Date: Tue, 17 Aug 2021 11:39:49 GMT
Content-Type: text/html
Content-Length: 867
Last-Modified: Tue, 17 Aug 2021 11:12:16 GMT
Connection: keep-alive
ETag: "611b9990-363"
Accept-Ranges: bytes
...
```

### [nmap]() 

`Nmap` (Network Mapper) is an open-source tool used for network discovery and security auditing. It can scan networks to detect devices, open ports, services, and potential vulnerabilities. More on nmap [here](https://hexadivine.gitbook.io/hd/notes/tools/nmap)

```
$ nmap -sV 10.10.62.190

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-22 17:48 IST
Nmap scan report for 10.10.62.190
Host is up (0.29s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
80/tcp  open  http    Apache httpd 2.4.61 ((Debian))
111/tcp open  rpcbind 2-4 (RPC #100000)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.10 seconds

```

## [Discover subdomain]()

### [sublist3r]()

If there is an error (`[!] Error: Virustotal probably now is blocking our requests`) remove the `virustotal` class \[[ref](https://github.com/aboul3la/Sublist3r/issues/343)]

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ sublist3r -d tesla.com

                 ____        _     _ _     _   _____
                / ___| _   _| |__ | (_)___| |_|___ / _ __
                \___ \| | | | '_ \| | / __| __| |_ \| '__|
                 ___) | |_| | |_) | | \__ \ |_ ___) | |
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|

                # Coded By Ahmed Aboul-Ela - @aboul3la
    
[-] Enumerating subdomains now for tesla.com
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..

[-] Total Unique Subdomains Found: 356
www.tesla.com
CitiApiEncProdv4.tesla.com
CitiApiEncSandboxv4.tesla.com
CitiApiSslProdv4.tesla.com
CitiApiSslSandboxv4.tesla.com
CitiBankStatementSHA512.tesla.com
.
[many results in between]
.
www-uat2.tesla.com
www45.tesla.com
xmail.tesla.com
```

## [crt.sh]()

Check this website [here](https://crt.sh/)

![](Pasted%20image%2020241203191947.png)
![](Pasted%20image%2020241203192001.png)


## Burp suite - site map

Burp suite helps get the sitemap by crawling through the website. This shows routing handled by the server. More on burp suite [here](https://hexadivine.gitbook.io/hrushikeshdolas/notes/tools/burp-suite).

![](Pasted%20image%2020241203203042.png)

