![](8f20f6a9ac029fba68066a06cb17611e.png)
## Introduction

**Passive recon** involves gathering information without directly interacting with the target system, often using publicly available sources like websites, social media, and domain records.

## whois

The `whois` command is used to query domain registration information, providing details about the domain owner, registrar, and registration dates.

```
$ whois tryhackme.com

[Querying whois.verisign-grs.com]
[Redirected to whois.namecheap.com]
[Querying whois.namecheap.com]
[whois.namecheap.com]
Domain name: tryhackme.com
Registry Domain ID: 2282723194_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2021-05-01T19:43:23.31Z
Creation Date: 2018-07-05T19:46:15.00Z
Registrar Registration Expiration Date: 2027-07-05T19:46:15.00Z
Registrar: NAMECHEAP INC
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.6613102107
Reseller: NAMECHEAP INC
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: 
Registrant Name: Withheld for Privacy Purposes
Registrant Organization: Privacy service provided by Withheld for Privacy ehf
[...]
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2021-08-25T14:58:29.57Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
```

## nslookup

`nslookup` (Name Server Lookup) is  used for querying DNS (Domain Name System) servers to retrieve domain name or IP address information. It allows users to check and troubleshoot DNS records, such as A records (IP addresses), MX records (mail servers), and others.

|Query type|Result|
|---|---|
|A|IPv4 Addresses|
|AAAA|IPv6 Addresses|
|CNAME|Canonical Name|
|MX|Mail Servers|
|SOA|Start of Authority|
|TXT|TXT Records|

```
$ nslookup -type=A tryhackme.com 1.1.1.1

Server:		1.1.1.1
Address:	1.1.1.1#53

Non-authoritative answer:
Name:	tryhackme.com
Address: 172.67.69.208
Name:	tryhackme.com
Address: 104.26.11.229
Name:	tryhackme.com
Address: 104.26.10.229
```

```
$ nslookup -type=MX tryhackme.com

Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
tryhackme.com	mail exchanger = 5 alt1.aspmx.l.google.com.
tryhackme.com	mail exchanger = 1 aspmx.l.google.com.
tryhackme.com	mail exchanger = 10 alt4.aspmx.l.google.com.
tryhackme.com	mail exchanger = 10 alt3.aspmx.l.google.com.
tryhackme.com	mail exchanger = 5 alt2.aspmx.l.google.com.
```

## dig

`dig` (Domain Information Groper) is used to query DNS servers. It provides more detailed and customised results compared to `nslookup`. `dig` allows users to query specific record types, set timeouts, and perform reverse lookups.

```
$ dig thmlabs.com TXT

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> thmlabs.com TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 26321
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 0
;; QUESTION SECTION:
;thmlabs.com.			IN	TXT

;; ANSWER SECTION:
thmlabs.com.		300	IN	TXT	"THM{a5b83929888ed36acb0272971e438d78}"

;; Query time: 233 msec
;; SERVER: 192.168.0.1#53(192.168.0.1) (UDP)
;; WHEN: Fri Nov 22 16:57:56 IST 2024
;; MSG SIZE  rcvd: 90
```

## DNSDumpster

[DNSDumpster](https://dnsdumpster.com/) is an online tool that helps gather DNS records and details about a domain, including subdomains, IP addresses, and associated infrastructure.

![](Pasted%20image%2020241122170212.png)
![](Pasted%20image%2020241122170249.png)

