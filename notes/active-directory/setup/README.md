# [Requirements]()

- Download [windows server 2022](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022), [windows enterprise](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) and virtual box.
- It is required to have these VMs: 1 windows server, and 2 windows 10 enterprise clients on same network, hence update the network tab to below after creating VMs.

![](assets/Pasted%20image%2020241215170959.png)

# [Installation]()
## [Windows Server 2022]()

- While creating windows server 2022 on virtual box click on skip unattended installation to manually install.

![](assets/Pasted%20image%2020241215171549.png)
![](assets/Pasted%20image%2020241215171852.png)
![](assets/Pasted%20image%2020241215171938.png)
![](assets/Pasted%20image%2020241215172005.png)
![](assets/Pasted%20image%2020241215172037.png)
![](assets/Pasted%20image%2020241215172108.png)

- click on custom install

![](assets/Pasted%20image%2020241215172134.png)
![](assets/Pasted%20image%2020241215172156.png)

- click on new and apply

![](assets/Pasted%20image%2020241215172225.png)
![](assets/Pasted%20image%2020241215172248.png)
![](assets/Pasted%20image%2020241215172514.png)
![](assets/Pasted%20image%2020241215172647.png)
![](assets/Pasted%20image%2020241215173702.png)

- After successful install of windows server 2022 it should look like below.

![](assets/Pasted%20image%2020241215174101.png)

- Now the windows server 2022 has been successfully installed.

# [Setup]()

## [Windows server 2022]()

### [Change PC name]()

- setup the windows server name to `DC` (domain controller)

![](assets/Pasted%20image%2020241215174425.png)
![](assets/Pasted%20image%2020241215174511.png)
![](assets/Pasted%20image%2020241215174543.png)
![](assets/Pasted%20image%2020241215175148.png)

- After reboot configure the adaptor to update the name and assign static IP

![](assets/Pasted%20image%2020241215175739.png)
![](assets/Pasted%20image%2020241215175805.png)

### [Setup adaptor]()

- After clicking on change adaptor option right click on Ethernet and rename to `internal` for better understanding.

![](assets/Pasted%20image%2020241215175958.png)
![](assets/Pasted%20image%2020241215180410.png)

- After changing name update the properties by right click on `internal` adaptor > properties > Internet Protocol version 4 > properties

![](assets/Pasted%20image%2020241215180606.png)

- Change the setting to below.

```
IP address: 192.168.0.2
Subnet mask: 255.255.255.0
Default gateway: 192.168.0.1

Preferred DNS Server: 127.0.0.1
```

![](assets/Pasted%20image%2020241215180851.png)

### [Setup AD DS]()

- Click on manage > add roles and features.

![](assets/Pasted%20image%2020241215181640.png)
![](assets/Pasted%20image%2020241215181659.png)
![](assets/Pasted%20image%2020241215181721.png)
![](assets/Pasted%20image%2020241215181844.png)
