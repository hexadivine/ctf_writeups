![](a6b64823d42120e106cb7e89ceaa4817.png)
## Introduction

`Nmap` is a tool used to scan networks and find out which devices are connected, what services they are running, and if they have any security vulnerabilities. It's like a digital map that helps identify and analyse devices and systems on a network.

Below are some uses of nmap scan

### Enumerating target

```
$ nmap -sL -n 10.10.12.13/29

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-22 19:15 IST
Nmap scan report for 10.10.12.8
Nmap scan report for 10.10.12.9
Nmap scan report for 10.10.12.10
Nmap scan report for 10.10.12.11
Nmap scan report for 10.10.12.12
Nmap scan report for 10.10.12.13
Nmap scan report for 10.10.12.14
Nmap scan report for 10.10.12.15
Nmap done: 8 IP addresses (0 hosts up) scanned in 0.00 seconds
```

- `-sL`: tells nmap to simply list the targets **without** actually scanning them, meaning it will resolve and display the host-names and IP addresses of the specified range.
- `-n`: tells nmap not to perform DNS resolution

### Nmap Host Discovery Using ARP

![](Pasted%20image%2020241122203954.png)

```
$ nmap  -sn -PR 10.10.210.6/24

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-22 20:32 IST
Nmap scan report for 10.10.210.28
Host is up (0.28s latency).
Nmap scan report for 10.10.210.37
Host is up (0.27s latency).
Nmap scan report for 10.10.210.48
Host is up (0.39s latency).
Nmap scan report for 10.10.210.71
Host is up (0.39s latency).
Nmap scan report for 10.10.210.91
Host is up (0.38s latency).
Nmap scan report for 10.10.210.93
Host is up (0.27s latency).
Nmap scan report for 10.10.210.95
Host is up (0.50s latency).
Nmap scan report for 10.10.210.120
Host is up (0.32s latency).
Nmap scan report for 10.10.210.131
Host is up (0.40s latency).
Nmap scan report for 10.10.210.133
Host is up (0.40s latency).
Nmap scan report for 10.10.210.185
Host is up (0.40s latency).
Nmap scan report for 10.10.210.226
Host is up (0.43s latency).
Nmap scan report for 10.10.210.248
Host is up (0.41s latency).
Nmap done: 256 IP addresses (13 hosts up) scanned in 18.28 seconds

```

- **`-sn`** (Ping Scan): This tells Nmap to only check if hosts are alive by sending various ping probes (but without scanning ports).
- **`-PR`** (ARP Ping): This specifically uses **ARP** requests to detect whether hosts are online in a local network. ARP is typically used in local networks (same subnet), and it’s very effective because it doesn’t rely on ICMP (ping) which might be blocked by firewalls.

