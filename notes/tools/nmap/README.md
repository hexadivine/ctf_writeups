![](a6b64823d42120e106cb7e89ceaa4817.png)
## Introduction

`Nmap` is a tool used to scan networks and find out which devices are connected, what services they are running, and if they have any security vulnerabilities. It's like a digital map that helps identify and analyse devices and systems on a network.

Below are some uses of nmap scan

## Nmap Reverse-DNS Lookup

Nmap’s default behaviour is to use reverse-DNS online hosts. Because the hostnames can reveal a lot, this can be a helpful step. However, if you don’t want to send such DNS queries, you use `-n` to skip this step.

## Enumerating target

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

## Nmap Port Scan

### TCP Connect Scan

The `-sT` option in Nmap performs a TCP Connect scan, where Nmap attempts to complete the TCP handshake with the target system to determine if the port is open. This method is less stealthy than other scans but is useful when the user lacks raw socket privileges, as it relies on the operating system's own networking functions.

![](Pasted%20image%2020241123200003.png)

```
$ nmap -sT 10.10.153.107

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 09:53 BST
Nmap scan report for 10.10.153.107
Host is up (0.0024s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

![](Pasted%20image%2020241123200014.png)
### TCP SYN Scan

Unprivileged users are limited to connect scan. However, the default scan mode is SYN scan, and it requires a privileged (root or sudoer) user to run it. SYN scan does not need to complete the TCP 3-way handshake; instead, it tears down the connection once it receives a response from the server. Because we didn’t establish a TCP connection, this decreases the chances of the scan being logged. We can select this scan type by using the `-sS` option. The figure below shows how the TCP SYN scan works without completing the TCP 3-way handshake.

![](Pasted%20image%2020241123202502.png)

```
$ sudo nmap -sS 10.10.67.204

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 09:53 BST
Nmap scan report for 10.10.67.204
Host is up (0.0073s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```

![](Pasted%20image%2020241124080807.png)
### UDP Scan

UDP is a connectionless protocol, and hence it does not require any handshake for connection establishment. We cannot guarantee that a service listening on a UDP port would respond to our packets. However, if a UDP packet is sent to a closed port, an ICMP port unreachable error (type 3, code 3) is returned. You can select UDP scan using the `-sU` option

![](Pasted%20image%2020241124081432.png)
![](Pasted%20image%2020241124081446.png)

```
$ sudo nmap -sU 10.10.104.145

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 09:54 BST
Nmap scan report for 10.10.104.145
Host is up (0.00061s latency).
Not shown: 998 closed ports
PORT    STATE         SERVICE
68/udp  open|filtered dhcpc
111/udp open          rpcbind
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1085.05 seconds
```

![](Pasted%20image%2020241124081500.png)

### Null Scan

The null scan does not set any flag; all six flag bits are set to zero. You can choose this scan using the `-sN` option. A TCP packet with no flags set will not trigger any response when it reaches an open port, as shown in the figure below. Therefore, from Nmap’s perspective, a lack of reply in a null scan indicates that either the port is open or a firewall is blocking the packet.

![](Pasted%20image%2020241124085245.png)

However, we expect the target server to respond with an RST packet if the port is closed. Consequently, we can use the lack of RST response to figure out the ports that are not closed: open or filtered.

![](Pasted%20image%2020241124085310.png)

```
$ sudo nmap -sN 10.10.76.112

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:30 BST
Nmap scan report for 10.10.76.112
Host is up (0.00066s latency).
Not shown: 994 closed ports
PORT    STATE         SERVICE
22/tcp  open|filtered ssh
25/tcp  open|filtered smtp
80/tcp  open|filtered http
110/tcp open|filtered pop3
111/tcp open|filtered rpcbind
143/tcp open|filtered imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 96.50 seconds
```

### FIN Scan

The FIN scan sends a TCP packet with the FIN flag set. You can choose this scan type using the `-sF` option. Similarly, no response will be sent if the TCP port is open. Again, Nmap cannot be sure if the port is open or if a firewall is blocking the traffic related to this TCP port.

![](Pasted%20image%2020241124085607.png)

However, the target system should respond with an RST if the port is closed. Consequently, we will be able to know which ports are closed and use this knowledge to infer the ports that are open or filtered. It's worth noting some firewalls will 'silently' drop the traffic without sending an RST.

![](Pasted%20image%2020241124085629.png)

```
$ sudo nmap -sF 10.10.76.112

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:32 BST
Nmap scan report for 10.10.76.112
Host is up (0.0018s latency).
Not shown: 994 closed ports
PORT    STATE         SERVICE
22/tcp  open|filtered ssh
25/tcp  open|filtered smtp
80/tcp  open|filtered http
110/tcp open|filtered pop3
111/tcp open|filtered rpcbind
143/tcp open|filtered imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 96.52 seconds
```
### Xmas Scan

The Xmas scan gets its name after Christmas tree lights. An Xmas scan sets the FIN, PSH, and URG flags simultaneously. You can select Xmas scan with the option -sX. Like the Null scan and FIN scan, if an RST packet is received, it means that the port is closed. Otherwise, it will be reported as open|filtered. The following two figures show the case when the TCP port is open and the case when the TCP port is closed.

![](Pasted%20image%2020241124085727.png)
![](Pasted%20image%2020241124085740.png)

```
$ sudo nmap -sX 10.10.76.112

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:34 BST
Nmap scan report for 10.10.76.112
Host is up (0.00087s latency).
Not shown: 994 closed ports
PORT    STATE         SERVICE
22/tcp  open|filtered ssh
25/tcp  open|filtered smtp
80/tcp  open|filtered http
110/tcp open|filtered pop3
111/tcp open|filtered rpcbind
143/tcp open|filtered imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 84.85 seconds
```

### Custom scan

If you want to experiment with a new TCP flag combination beyond the built-in TCP scan types, you can do so using `--scanflags`. For instance, if you want to set SYN, RST, and FIN simultaneously, you can do so using `--scanflags RSTSYNFIN`. As shown in the figure below, if you develop your custom scan, you need to know how the different ports will behave to interpret the results in different scenarios correctly.

![](Pasted%20image%2020241124091422.png)
## Nmap Host Discovery Using

`nmap` perform host discovery using `-sn` flag. By default it uses ping request.
### ARP

This scan will send ARP request packets to every IP address on the subnet. We expect live hosts to reply.

![](Pasted%20image%2020241122203954.png)

```
$ sudo nmap -PR -sn 10.10.210.6/24

Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 07:12 BST
Nmap scan report for ip-10-10-210-75.eu-west-1.compute.internal (10.10.210.75)
Host is up (0.00013s latency).
MAC Address: 02:83:75:3A:F2:89 (Unknown)
Nmap scan report for ip-10-10-210-100.eu-west-1.compute.internal (10.10.210.100)
Host is up (-0.100s latency).
MAC Address: 02:63:D0:1B:2D:CD (Unknown)
Nmap scan report for ip-10-10-210-165.eu-west-1.compute.internal (10.10.210.165)
Host is up (0.00025s latency).
MAC Address: 02:59:79:4F:17:B7 (Unknown)
Nmap scan report for ip-10-10-210-6.eu-west-1.compute.internal (10.10.210.6)
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 3.12 seconds

```

- **`-sn`** (Ping Scan): This tells Nmap to only check if hosts are alive by sending various ping probes (but without scanning ports).
- `-PR` (ARP Ping): This specifically uses **ARP** requests to detect whether hosts are online in a local network. ARP is typically used in local networks (same subnet), and it’s very effective because it doesn’t rely on ICMP (ping) which might be blocked by firewalls.

![](Pasted%20image%2020241123073028.png)

### ICMP Echo

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

![](Pasted%20image%2020241123073106.png)

### ICMP Timestamp

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

![](Pasted%20image%2020241123073205.png)

### ICMP Address Mask Request

Similarly, Nmap uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18).

![](Pasted%20image%2020241123072319.png)

```
$ sudo nmap -PM -sn 10.10.68.220/24

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 12:13 EEST
Nmap done: 256 IP addresses (0 hosts up) scanned in 52.17 seconds
```

- `-PM` sends ICMP address mask request
- `-sn` performs live host scan

![](Pasted%20image%2020241123073235.png)

### TCP SYN

We can send a packet with the SYN (Synchronise) flag set to a TCP port, 80 by default, and wait for a response. An open port should reply with a SYN/ACK (Acknowledge); a closed port would result in an RST (Reset).

![](Pasted%20image%2020241123073827.png)

```
$ sudo nmap -PS -sn 10.10.68.220/24
Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:45 EEST
Nmap scan report for 10.10.68.52
Host is up (0.10s latency).
Nmap scan report for 10.10.68.121
Host is up (0.16s latency).
Nmap scan report for 10.10.68.125
Host is up (0.089s latency).
Nmap scan report for 10.10.68.134
Host is up (0.13s latency).
Nmap scan report for 10.10.68.220
Host is up (0.11s latency).
Nmap done: 256 IP addresses (5 hosts up) scanned in 17.38 seconds
```

- `-PS` performs TCP SYN flag

![](Pasted%20image%2020241123073906.png)
### TCP ACK

A TCP ACK Ping Nmap scan sends ACK (Acknowledgement) packets to a target, typically to determine whether a host is up and which ports are filtered, without establishing a full connection. It relies on the behaviour of firewalls or filtering devices, where unfiltered ports will respond with a RST (Reset) packet, while filtered ports will not respond.

![](Pasted%20image%2020241123074237.png)

```
$ sudo nmap -PA -sn 10.10.68.220/24

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:46 EEST
Nmap scan report for 10.10.68.52
Host is up (0.11s latency).
Nmap scan report for 10.10.68.121
Host is up (0.12s latency).
Nmap scan report for 10.10.68.125
Host is up (0.10s latency).
Nmap scan report for 10.10.68.134
Host is up (0.10s latency).
Nmap scan report for 10.10.68.220
Host is up (0.10s latency).
Nmap done: 256 IP addresses (5 hosts up) scanned in 29.89 seconds
```

`-PA` performs tcp ack packet

![](Pasted%20image%2020241123193115.png)
### UDP

In Nmap, a UDP Ping scan for host discovery sends UDP packets (typically to common ports like 53 or 161) to determine if a host is online. If the host is responsive, it may reply with a corresponding ICMP "Port Unreachable" message, indicating the host is reachable, while a lack of response suggests the host may be offline or the port is filtered.

![](Pasted%20image%2020241123193429.png)

```
$ sudo nmap -PU -sn 10.10.68.220/24

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-02 13:45 EEST
Nmap scan report for 10.10.68.52
Host is up (0.10s latency).
Nmap scan report for 10.10.68.121
Host is up (0.10s latency).
Nmap scan report for 10.10.68.125
Host is up (0.14s latency).
Nmap scan report for 10.10.68.134
Host is up (0.096s latency).
Nmap scan report for 10.10.68.220
Host is up (0.11s latency).
Nmap done: 256 IP addresses (5 hosts up) scanned in 9.20 seconds
```

## Spoofing and Decoys

The following figure shows the attacker launching the command `nmap -S <spoofed ip> 10.10.76.112`. Consequently, Nmap will craft all the packets using the provided source IP address `SPOOFED_IP`. The target machine will respond to the incoming packets sending the replies to the destination IP address `SPOOFED_IP`. For this scan to work and give accurate results, the attacker needs to monitor the network traffic to analyse the replies.

![](Pasted%20image%2020241124091831.png)

You can launch a decoy scan by specifying a specific or random IP address after `-D`. For example, `nmap -D 10.10.0.1,10.10.0.2,ME 10.10.76.112` will make the scan of 10.10.76.112 appear as coming from the IP addresses 10.10.0.1, 10.10.0.2, and then `ME` to indicate that your IP address should appear in the third order. Another example command would be `nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME 10.10.76.112`, where the third and fourth source IP addresses are assigned randomly, while the fifth source is going to be the attacker’s IP address. In other words, each time you execute the latter command, you would expect two new random IP addresses to be the third and fourth decoy sources.

![](Pasted%20image%2020241124092055.png)

## Fine-Tuning Scope and Performance

You can specify the ports you want to scan instead of the default 1000 ports. Specifying the ports is intuitive by now. Let’s see some examples:

- port list: `-p22,80,443` will scan ports 22, 80 and 443.
- port range: `-p1-1023` will scan all ports between 1 and 1023 inclusive, while `-p20-25` will scan ports between 20 and 25 inclusive.

You can control the scan timing using `-T<0-5>`. `-T0` is the slowest (paranoid), while `-T5` is the fastest. According to Nmap manual page, there are six templates:

- paranoid (0)
- sneaky (1)
- polite (2)
- normal (3)
- aggressive (4)
- insane (5)

Alternatively, you can choose to control the packet rate using `--min-rate <number>` and `--max-rate <number>`. For example, `--max-rate 10` or `--max-rate=10` ensures that your scanner is not sending more than ten packets per second.

Moreover, you can control probing parallelization using `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>`. For instance, `--min-parallelism=512` pushes Nmap to maintain at least 512 probes in parallel; these 512 probes are related to host discovery and open ports.

## Summary

| Scan Type                      | Example Command                                          | Purpose                                                                        |
| ------------------------------ | -------------------------------------------------------- | ------------------------------------------------------------------------------ |
| TCP Connect Port Scan          | `nmap -sT 10.10.67.57`                                   | non-sudo users to scan ports with 3 way handshake.                             |
| TCP SYN Port Scan              | `sudo nmap -sS <ip>`                                     | sudo users can scan with incomplete handshake. Reduces noise. Increases speed. |
| UDP Port Scan                  | `sudo nmap -sU <ip>`                                     | unreliable port scan. Revives ICMP type 3 if port is closed                    |
| All port scan                  | `nmap -p- <ip>`                                          | scans all ports                                                                |
| Limited port scan              | `nmap -p1-1023 <ip>`                                     | scan ports 1 to 1023                                                           |
| Common port scan               | `nmap -F <ip>`                                           | 100 most common ports                                                          |
| Consecutive scan               | `nmap -F -r <ip>`                                        | scan ports in consecutive order                                                |
| Speed customisation            | `nmap -T<0-5> <ip>`                                      | `-T0` being the slowest and `-T5` the fastest                                  |
| Rate customisation             | `nmap --max-rate 50 <ip>`                                | rate <= 50 packets/sec                                                         |
| Rate customisation             | `nmap --min-rate 15 <ip>`                                | rate >= 15 packets/sec                                                         |
| Probe customisation            | `nmap --min-parallelism 100 <ip>`                        | at least 100 probes in parallel                                                |
| TCP Null Scan                  | `sudo nmap -sN 10.10.249.146`                            |                                                                                |
| TCP FIN Scan                   | `sudo nmap -sF 10.10.249.146`                            |                                                                                |
| TCP Xmas Scan                  | `sudo nmap -sX 10.10.249.146`                            |                                                                                |
| TCP Maimon Scan                | `sudo nmap -sM 10.10.249.146`                            |                                                                                |
| TCP ACK Scan                   | `sudo nmap -sA 10.10.249.146`                            |                                                                                |
| TCP Window Scan                | `sudo nmap -sW 10.10.249.146`                            |                                                                                |
| Custom TCP Scan                | `sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.249.146` |                                                                                |
| Spoofed Source IP              | `sudo nmap -S SPOOFED_IP 10.10.249.146`                  |                                                                                |
| Spoofed MAC Address            | `--spoof-mac SPOOFED_MAC`                                |                                                                                |
| Decoy Scan                     | `nmap -D DECOY_IP,ME 10.10.249.146`                      |                                                                                |
| Idle (Zombie) Scan             | `sudo nmap -sI ZOMBIE_IP 10.10.249.146`                  |                                                                                |
| Fragment IP data into 8 bytes  | `-f`                                                     |                                                                                |
| Fragment IP data into 16 bytes | `-ff`                                                    |                                                                                |

| Port Scan Type                 | Example Command                                          |
| ------------------------------ | -------------------------------------------------------- |
| TCP Null Scan                  | `sudo nmap -sN 10.10.249.146`                            |
| TCP FIN Scan                   | `sudo nmap -sF 10.10.249.146`                            |
| TCP Xmas Scan                  | `sudo nmap -sX 10.10.249.146`                            |
| TCP Maimon Scan                | `sudo nmap -sM 10.10.249.146`                            |
| TCP ACK Scan                   | `sudo nmap -sA 10.10.249.146`                            |
| TCP Window Scan                | `sudo nmap -sW 10.10.249.146`                            |
| Custom TCP Scan                | `sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.249.146` |
| Spoofed Source IP              | `sudo nmap -S SPOOFED_IP 10.10.249.146`                  |
| Spoofed MAC Address            | `--spoof-mac SPOOFED_MAC`                                |
| Decoy Scan                     | `nmap -D DECOY_IP,ME 10.10.249.146`                      |
| Idle (Zombie) Scan             | `sudo nmap -sI ZOMBIE_IP 10.10.249.146`                  |
| Fragment IP data into 8 bytes  | `-f`                                                     |
| Fragment IP data into 16 bytes | `-ff`                                                    |