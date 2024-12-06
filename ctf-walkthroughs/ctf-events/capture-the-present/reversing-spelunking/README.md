# Description

An ancient treasure has been located deep underground in a complex network of caves. Unfortunately, a cave-in has made it impossible to access it through normal means...

# Attempt

Extracted provided zip file

![](../assets/Pasted%20image%2020241202142030.png)

Checking file.

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_spelunking]
└──╼ $ file spelunking 
spelunking: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=23cd0c4f434f18bf0207efc4e1e087546faa8c64, for GNU/Linux 3.2.0, not stripped
```

Tried to check with strings. Below gave the flag.

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/rev_spelunking]
└──╼ $ strings spelunking  | grep htb -i
HTB{tr34sur3s_bur13d_d33p_und3rgr0und}
```
