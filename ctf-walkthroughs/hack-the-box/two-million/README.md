
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

Data contains encrypted message with `rot13`.

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

