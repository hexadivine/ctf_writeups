![](Pasted%20image%2020241115021456.png)
# Introduction

Command injection vulnerability allows attacker to inject a bash command allowing them to interact with shell. The types include

- Blind command injection
	- No direct output. Requires investigation.
- Verbose command injection
	- There is direct feedback. Ex., running `whoami` on a webpage returns response in `div` tag.

# Discovery

- Try inserting `|`, `&&`, `#` and then shell command and observe the behaviour
- Look for `exec`, `Passthru`, `system` functions.
- Commonly found in PHP, python, node.js as they can directly interact with the shell.

![](Pasted%20image%2020241115015640.png)

![](Pasted%20image%2020241115015653.png)
## In blind command Injection

- detect by forcing output using `>`
- use curl, burp suit.
