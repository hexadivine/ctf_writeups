
![](Pasted%20image%2020241126174453.png)

# [Introduction]()

At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorised access to resources that are usually restricted from the users.

# [Enumeration]()

Enumeration is the first step you have to take once you gain access to any system.

## [hostname]()

The `hostname` command will return the hostname of the target machine. Although this value can easily be changed or have a relatively meaningless string (e.g. Ubuntu-3487340239), in some cases, it can provide information about the target system’s role within the corporate network (e.g. SQL-PROD-01 for a production SQL server).

```
$ hostname

wade7363
```

## [uname -a]()

Will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.

```
$ uname -a

Linux wade7363 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x
86_64 x86_64 x86_64 GNU/Linux
```

## [/proc/version]()

The proc filesystem (procfs) provides information about the target system processes. You will find proc on many different Linux flavours, making it an essential tool to have in your arsenal.

Looking at `/proc/version` may give you information on the kernel version and additional data such as whether a compiler (e.g. GCC) is installed.

```
$ cat /proc/version

Linux version 3.13.0-24-generic (buildd@panlong) (gcc version 4.8.2 (Ubuntu 4.
8.2-19ubuntu1) ) #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014
```

## [/etc/issue]()

Systems can also be identified by looking at the `/etc/issue` file. This file usually contains some information about the operating system but can easily be customised or changed. While on the subject, any file containing system information can be customised or changed. For a clearer understanding of the system, it is always good to look at all of these.

```
$ cat /etc/issue

Ubuntu 14.04 LTS \n \l
```

## [ps Command]()

`ps` show processes for the current shell.

The output of the `ps` (Process Status) will show the following;

- PID: The process ID (unique to the process)
- TTY: Terminal type used by the user
- Time: Amount of CPU time used by the process (this is NOT the time this process has been running for)
- CMD: The command or executable running (will NOT display any command line parameter)

The “ps” command provides a few useful options.

- `ps -A`: View all running processes
- `ps axjf`: View process tree (see the tree formation until `ps axjf` is run below)

![](https://i.imgur.com/xsbohSd.png)  

- `ps aux`: The `aux` option will show processes for all users (a), display the user that launched the process (u), and show processes that are not attached to a terminal (x). Looking at the ps aux command output, we can have a better understanding of the system and potential vulnerabilities.
## [env]()

The `env` command will show environmental variables.
  
![](https://i.imgur.com/LWdJ8Fw.png)

The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

## [Id]()

The `id` command will provide a general overview of the user’s privilege level and group memberships. It is worth remembering that the `id` command can also be used to obtain the same information for another user as seen below.

![](https://i.imgur.com/YzfJliG.png)

## [/etc/passwd]()

Reading the `/etc/passwd` file can be an easy way to discover users on the system.

```
$ cat /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
usbmux:x:103:46:usbmux daemon,,,:/home/usbmux:/bin/false
dnsmasq:x:104:65534:dnsmasq,,,:/var/lib/misc:/bin/false
avahi-autoipd:x:105:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
kernoops:x:106:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
rtkit:x:107:114:RealtimeKit,,,:/proc:/bin/false
saned:x:108:115::/home/saned:/bin/false
whoopsie:x:109:116::/nonexistent:/bin/false
speech-dispatcher:x:110:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/sh
avahi:x:111:117:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
lightdm:x:112:118:Light Display Manager:/var/lib/lightdm:/bin/false
colord:x:113:121:colord colour management daemon,,,:/var/lib/colord:/bin/false
hplip:x:114:7:HPLIP system user,,,:/var/run/hplip:/bin/false
pulse:x:115:122:PulseAudio daemon,,,:/var/run/pulse:/bin/false
matt:x:1000:1000:matt,,,:/home/matt:/bin/bash
karen:x:1001:1001::/home/karen:
sshd:x:116:65534::/var/run/sshd:/usr/sbin/nologin
```

That this will return all users, some of which are system or service users that would not be very useful. Another approach could be to grep for “home” as real users will most likely have their folders under the “home” directory.

```
$ cat /etc/passwd | grep home

syslog:x:101:104::/home/syslog:/bin/false
usbmux:x:103:46:usbmux daemon,,,:/home/usbmux:/bin/false
saned:x:108:115::/home/saned:/bin/false
matt:x:1000:1000:matt,,,:/home/matt:/bin/bash
karen:x:1001:1001::/home/karen:
```

## [history]()

Looking at earlier commands with the `history` command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

## [ifconfig]()

The target system may be a pivoting point to another network. The `ifconfig` command will give us information about the network interfaces of the system. The example below shows the target system has three interfaces (eth0, tun0, and tun1). Our attacking machine can reach the eth0 interface but can not directly access the two other networks.

![](https://i.imgur.com/hcdZnwK.png)

This can be confirmed using the `ip route` command to see which network routes exist.

![](https://i.imgur.com/PSrmz5O.png)
## [netstat]()

Following an initial check for existing interfaces and network routes, it is worth looking into existing communications. The `netstat` command can be used with several different options to gather information on existing connections.

- `netstat -a`: shows all listening ports and established connections.
- `netstat -at` or `netstat -au` can also be used to list TCP or UDP protocols respectively.
- `netstat -l`: list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports that are listening using the TCP protocol (below)

```
$ netstat -l

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 *:ssh                   *:*                     LISTEN     
tcp        0      0 localhost:ipp           *:*                     LISTEN     
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN     
tcp6       0      0 ip6-localhost:ipp       [::]:*                  LISTEN     
udp        0      0 *:bootpc                *:*                                
udp        0      0 *:ipp                   *:*                                
udp        0      0 *:mdns                  *:*                                
udp        0      0 *:24810                 *:*                                
udp        0      0 *:49486                 *:*                                
udp6       0      0 [::]:38942              [::]:*                             
udp6       0      0 [::]:mdns               [::]:*                             
udp6       0      0 [::]:35201              [::]:*                             
Active UNIX domain sockets (only servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ACC ]     STREAM     LISTENING     9884     /tmp/.X11-unix/X0
unix  2      [ ACC ]     STREAM     LISTENING     9484     /var/run/acpid.socket
unix  2      [ ACC ]     STREAM     LISTENING     10521    /var/run/cups/cups.sock
unix  2      [ ACC ]     STREAM     LISTENING     9883     @/tmp/.X11-unix/X0
unix  2      [ ACC ]     STREAM     LISTENING     10702    @/tmp/dbus-E3WLb7aKEy
unix  2      [ ACC ]     STREAM     LISTENING     10750    @/tmp/dbus-lkX1e3FTf3
unix  2      [ ACC ]     STREAM     LISTENING     9770     /var/lib/amazon/ssm/ipc/termination
unix  2      [ ACC ]     SEQPACKET  LISTENING     7512     /run/udev/control
unix  2      [ ACC ]     STREAM     LISTENING     11360    /run/user/112/pulse/native
unix  2      [ ACC ]     STREAM     LISTENING     7101     @/com/ubuntu/upstart
unix  2      [ ACC ]     STREAM     LISTENING     7803     /var/run/avahi-daemon/socket
unix  2      [ ACC ]     STREAM     LISTENING     10971    @/com/ubuntu/upstart-session/112/1354
unix  2      [ ACC ]     STREAM     LISTENING     9769     /var/lib/amazon/ssm/ipc/health
unix  2      [ ACC ]     STREAM     LISTENING     7607     /var/run/dbus/system_bus_socket
unix  2      [ ACC ]     STREAM     LISTENING     7879     /var/run/sdp
```

`netstat -s`: list network usage statistics by protocol (below) This can also be used with the `-t` or `-u` options to limit the output to a specific protocol.

```
$ netstat -s

Ip:
    1103 total packets received
    0 forwarded
    0 incoming packets discarded
    1098 incoming packets delivered
    1107 requests sent out
Icmp:
    1 ICMP messages received
    0 input ICMP message failed.
    ICMP input histogram:
        destination unreachable: 1
    0 ICMP messages sent
    0 ICMP messages failed
    ICMP output histogram:
IcmpMsg:
        InType3: 1
Tcp:
    86 active connections openings
    6 passive connection openings
    54 failed connection attempts
    0 connection resets received
    2 connections established
    1063 segments received
    1020 segments send out
    16 segments retransmited
    0 bad segments received.
    54 resets sent
Udp:
    107 packets received
    0 packets to unknown port received.
    0 packet receive errors
    142 packets sent
```

- `netstat -tp`: list connections with the service name and PID information.

![](Pasted%20image%2020241126182005.png)

- `netstat -i`: Shows interface statistics. We see below that “eth0” and “tun0” are more active than “tun1”.

```
$ netstat -i

Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       9001 0      1275      0      0 0          1329      0      0      0 BMRU
lo        65536 0       127      0      0 0           127      0      0      0 LRU
```

- `netstat -ano` which could be broken down as follows;
	- `-a`: Display all sockets
	- `-n`: Do not resolve names
	- `-o`: Display timers
```
$ netstat -ano
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       Timer
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 10.10.197.63:33588      169.254.169.254:80      TIME_WAIT   timewait (0.00/0/0)
tcp        0      0 10.10.197.63:22         10.100.1.234:55868      ESTABLISHED keepalive (6283.53/0/0)
tcp        0      1 10.10.197.63:40244      91.189.91.83:80         SYN_SENT    on (25.38/5/0)
tcp        0      1 10.10.197.63:51670      91.189.91.81:80         SYN_SENT    on (25.38/5/0)
tcp        0      0 10.10.197.63:22         10.100.1.234:55852      ESTABLISHED keepalive (6283.53/0/0)
tcp6       0      0 :::22                   :::*                    LISTEN      off (0.00/0/0)
tcp6       0      0 ::1:631                 :::*                    LISTEN      off (0.00/0/0)
tcp6       1      0 ::1:53631               ::1:631                 CLOSE_WAIT  off (0.00/0/0)
udp        0      0 0.0.0.0:68              0.0.0.0:*                           off (0.00/0/0)
udp        0      0 0.0.0.0:631             0.0.0.0:*                           off (0.00/0/0)
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           off (0.00/0/0)
udp        0      0 0.0.0.0:24810           0.0.0.0:*                           off (0.00/0/0)
udp        0      0 0.0.0.0:49486           0.0.0.0:*                           off (0.00/0/0)
udp6       0      0 :::38942                :::*                                off (0.00/0/0)
udp6       0      0 :::5353                 :::*                                off (0.00/0/0)
udp6       0      0 :::35201                :::*                                off (0.00/0/0)
Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  2      [ ACC ]     STREAM     LISTENING     9884     /tmp/.X11-unix/X0
unix  2      [ ACC ]     STREAM     LISTENING     9484     /var/run/acpid.socket
unix  2      [ ACC ]     STREAM     LISTENING     10521    /var/run/cups/cups.sock
unix  2      [ ACC ]     STREAM     LISTENING     9883     @/tmp/.X11-unix/X0
unix  2      [ ACC ]     STREAM     LISTENING     10702    @/tmp/dbus-E3WLb7aKEy
```

## [find Command]()

Below are some useful examples for the “find” command.

**Find files:**

- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 day
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files with a 50 MB size
- `find / -size +50M`: find files with a size above 50MB; `-50M` for below.
- `find / -writable -type d` : Find world-writeable folders
- `find / -perm -222 -type d`: Find world-writeable folders
- `find / -perm -o w -type d`: Find world-writeable folders

use  `2>/dev/null` to hide error messages, in first case it is  
`find . -name flag1.txt 2>/dev/null`

# [Automated Enumeration Tools]()

Several tools can help you save time during the enumeration process. These tools should only be used to save time knowing they may miss some privilege escalation vectors. Below is a list of popular Linux enumeration tools with links to their respective Github repositories.

- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)[](https://github.com/rebootuser/LinEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

# [Examples of various privilege escalation]()

Below are some examples of common privilege escalation.

## [Kernel Exploit]()

The Kernel exploit methodology is simple;

1. Identify the kernel version
2. Search and find an exploit code for the kernel version of the target system
3. Run the exploit

