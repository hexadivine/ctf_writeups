# Description

Before you enter the Oasis, you'll need to acquire top-of-the-line tech to give you the edge. You've ordered a brand-new X1 Haptic Bootsuit - now you just need to unpack it.

# Attempt

Checking files

![](Pasted%20image%2020241202142743.png)

Performing strings check 

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_unpacking]
└──╼ $ strings unpacking | grep htb -i
b$nd new X1 HTB (HapTic Boot
HTB{T6-Ultimat
```

Got first half. Finding second half. Got last part.

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_unpacking]
└──╼ $ strings unpacking | grep -E .*-.*
nux-x86-
HTB{T6-Ultimat
n-cker}
$Id: UPX 4.01 Copyright (C) 1996-2022 the UPX Team. All Rights Reserved. $
-ps?
GCC: (Debian 10.2.1-6)	 
no.gnu.build-id
ABI-(g
 ?4-
```

Above part showed the last part of flag but more importantly it showed that it is UPX file. Hence decompiling it.

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_unpacking]
└──╼ $ upx -d unpacking 
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.2       Markus Oberhumer, Laszlo Molnar & John Reiser    Jan 3rd 2024

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
     28571 <-      5308   18.58%   linux/amd64   unpacking

Unpacked 1 file.
```

Running strings again.

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_unpacking]
└──╼ $ strings unpacking | grep -E .*-.*
/lib64/ld-linux-x86-64.so.2
HTB{The-Ultimate-Elf-Unpacker}
GCC: (Debian 10.2.1-6) 10.2.1 20210110
.note.gnu.build-id
.note.ABI-tag
```

Found the flag `HTB{The-Ultimate-Elf-Unpacker}`