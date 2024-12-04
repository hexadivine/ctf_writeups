![](8f20f6a9ac029fba68066a06cb17611e.png)
## [Introduction]()

**Passive recon** involves gathering information without directly interacting with the target system, often using publicly available sources like websites, social media, and domain records.

## [Search engines]()

- Check out [advanced operations search list](https://github.com/cipher387/Advanced-search-operators-list)  for google dorks and more

## [Basic Google dorks]()

- **"keyword"**: Searches for an exact phrase.
- **site:domain**: Limits search results to a specific website or domain (e.g., `site:example.com`).
- **intitle:keyword**: Finds pages with a specific keyword in the title.
- **inurl:keyword**: Finds pages with a specific keyword in the URL.
- **intext:keyword**: Searches for a specific keyword in the text of the webpage (e.g., `intext:"confidential data"`).
- **filetype:extension**: Finds files of a particular type (e.g., `filetype:pdf confidential`).
- **AND, OR**: Combines multiple search conditions (e.g., `intitle:"index of" AND filetype:pdf`).
- \*: Wildcard operator used to replace unknown words (e.g., `intitle:"index of *" inurl:password`).
- **cache:URL**: Finds the cached version of a web page.
## [Specialized Search Engines]()

- [Shodan](https://www.shodan.io), a search engine for devices connected to the Internet. check out [shodan search examples](https://www.shodan.io/search/examples)
- [Censys](https://search.censys.io/)focuses on Internet-connected hosts, websites, certificates, and other Internet assets. check out [search queries](https://support.censys.io/hc/en-us/categories/25499474122004-Censys-Search-Language)
- [VirusTotal](https://www.virustotal.com/) is an online website that provides a virus-scanning service for files using multiple antivirus engines
- [Have I Been Pwned](https://haveibeenpwned.com/) (HIBP) tells an email address has appeared in a leaked data breach.

## [whois]()

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

## [nslookup]()

`nslookup` (Name Server Lookup) is  used for querying DNS (Domain Name System) servers to retrieve domain name or IP address information. It allows users to check and troubleshoot DNS records, such as A records (IP addresses), MX records (mail servers), and others.

| Query type |       Result       |
| :--------: | :----------------: |
|     A      |   IPv4 Addresses   |
|    AAAA    |   IPv6 Addresses   |
|   CNAME    |   Canonical Name   |
|     MX     |    Mail Servers    |
|    SOA     | Start of Authority |
|    TXT     |    TXT Records     |

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

## [dig]()

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

## [DNSDumpster]()

[DNSDumpster](https://dnsdumpster.com/) is an online tool that helps gather DNS records and details about a domain, including subdomains, IP addresses, and associated infrastructure.

![](Pasted%20image%2020241122170212.png)
![](Pasted%20image%2020241122170249.png)

## [Vulnerabilities and Exploits]()

- use [CVE Program](https://www.cve.org/) or [National Vulnerability Database](https://nvd.nist.gov/) (NVD) for info about CVEs
- [Exploit Database](https://www.exploit-db.com/). lists exploit codes
- [GitHub](https://github.com), can contain many tools related to CVEs, along with proof-of-concept (PoC) and exploit codes.

## [Email recon]()

Emails of employees can be searched using [clearbit](https://chromewebstore.google.com/detail/clearbit-connect-free-ver/pmnhcgfcafcnkbengdcanjablaabjplo)

![](Pasted%20image%2020241203183559.png)

[hunter.io](https://hunter.io/verify/hexadivine@gmail.com) can help verify the email address. With employee email you can find email of other employees.

![](Pasted%20image%2020241203183939.png)

<https://dehashed.com/>  shows compromised data (including email, username, name, address..etc.); however this is not free.

![](Pasted%20image%2020241203190241.png)![](Pasted%20image%2020241203190610.png)

## [Website build with info]()

<https://builtwith.com/instagram.com> shows tech-stack, framework and how the site is uses other resources to achieve various goals.

![](Pasted%20image%2020241203201728.png)

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
