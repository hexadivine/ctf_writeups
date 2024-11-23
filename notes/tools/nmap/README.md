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

### Nmap Host Discovery 

nmap per

#### Using ARP

This scan will send ARP request packets to every IP address on the subnet. We expect live hosts to reply.

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
- `-PR` (ARP Ping): This specifically uses **ARP** requests to detect whether hosts are online in a local network. ARP is typically used in local networks (same subnet), and it’s very effective because it doesn’t rely on ICMP (ping) which might be blocked by firewalls.

### Nmap Host Discovery Using ICMP

This scan will send ICMP echo packets to every IP address on the subnet. Again, we expect live hosts to reply.

![](Pasted%20image%2020241122210254.png)

```
$ sudo nmap -PE -sn 10.10.68.220/24

Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 10:16 BST
Nmap scan report for ip-10-10-68-50.eu-west-1.compute.internal (10.10.68.50)
Host is up (0.00017s latency).
MAC Address: 02:95:36:71:5B:87 (Unknown)
Nmap scan report for ip-10-10-68-52.eu-west-1.compute.internal (10.10.68.52)
Host is up (0.00017s latency).
MAC Address: 02:48:E8:BF:78:E7 (Unknown)
Nmap scan report for ip-10-10-68-77.eu-west-1.compute.internal (10.10.68.77)
Host is up (-0.100s latency).
MAC Address: 02:0F:0A:1D:76:35 (Unknown)
Nmap scan report for ip-10-10-68-110.eu-west-1.compute.internal (10.10.68.110)
Host is up (-0.10s latency).
MAC Address: 02:6B:50:E9:C2:91 (Unknown)
Nmap scan report for ip-10-10-68-140.eu-west-1.compute.internal (10.10.68.140)
Host is up (0.00021s latency).
MAC Address: 02:58:59:63:0B:6B (Unknown)
Nmap scan report for ip-10-10-68-142.eu-west-1.compute.internal (10.10.68.142)
Host is up (0.00016s latency).
MAC Address: 02:C6:41:51:0A:0F (Unknown)
Nmap scan report for ip-10-10-68-220.eu-west-1.compute.internal (10.10.68.220)
Host is up (0.00026s latency).
MAC Address: 02:25:3F:DB:EE:0B (Unknown)
Nmap scan report for ip-10-10-68-222.eu-west-1.compute.internal (10.10.68.222)
Host is up (0.00025s latency).
MAC Address: 02:28:B1:2E:B0:1B (Unknown)
Nmap done: 256 IP addresses (8 hosts up) scanned in 2.11 seconds
```

- `-PE` option tells Nmap to use ICMP echo requests.
- `-sn` option tells nmap to perform live host scan

Because ICMP echo requests tend to be blocked, you might also consider ICMP Timestamp or ICMP Address Mask requests to tell if a system is online.

![](Pasted%20image%2020241123071959.png)

```
$ sudo nmap -PP -sn 10.10.68.220/24

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 12:06 EEST
Nmap scan report for 10.10.68.50
Host is up (0.13s latency).
Nmap scan report for 10.10.68.52
Host is up (0.25s latency).
Nmap scan report for 10.10.68.77
Host is up (0.14s latency).
Nmap scan report for 10.10.68.110
Host is up (0.14s latency).
Nmap scan report for 10.10.68.140
Host is up (0.15s latency).
Nmap scan report for 10.10.68.209
Host is up (0.14s latency).
Nmap scan report for 10.10.68.220
Host is up (0.14s latency).
Nmap scan report for 10.10.68.222
Host is up (0.14s latency).
Nmap done: 256 IP addresses (8 hosts up) scanned in 10.93 seconds
```

- `-PP` option tells Nmap to use ICMP timestamp requests.
- `-sn` option tells nmap to perform live host scan

Similarly, Nmap uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18).

![](Pasted%20image%2020241123072319.png)

```
$ sudo nmap -PM -sn 10.10.68.220/24

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 12:13 EEST
Nmap done: 256 IP addresses (0 hosts up) scanned in 52.17 seconds
```

- `-PM` sends ICMP address mask request
- `-sn` performs live host scan

