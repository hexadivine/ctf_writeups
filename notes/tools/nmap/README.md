![](a6b64823d42120e106cb7e89ceaa4817.png)
## Introduction

`Nmap` is a tool used to scan networks and find out which devices are connected, what services they are running, and if they have any security vulnerabilities. It's like a digital map that helps identify and analyse devices and systems on a network.

Below are some uses of nmap scan

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

## Nmap Host Discovery Using ARP


