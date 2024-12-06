# Description

Meet dSCADA, the next-generation SCADA and HMI Panel for Industrial Systems. The panel is currently in the beta stage and is operating on a prototype. Can you take a look and see if it contains any security vulnerabilities?

# Recon

Checking provided ip <http://94.237.62.166:51028/>

![](Pasted%20image%2020241202164434.png)

Able to login with admin:admin

![](Pasted%20image%2020241202165028.png)

Checking settings. 

![](Pasted%20image%2020241202165048.png)

# Exploitation

The response message show that it is executing shell command. We can exploit Command Injection.

![](Pasted%20image%2020241202173143.png)
![](Pasted%20image%2020241202173123.png)

Executing the `readflag` file gave the flag - `HTB{gr3p3d_my_w4y_thr0ugH_rc3}`