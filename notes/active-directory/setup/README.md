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

## [Windows 10 Enterprise]()

- Similar to windows servers installation keep the skip unattended installation ticked and finish (make sure it has proper network configuration mentioned in the requirements section)

![](assets/Pasted%20image%2020241215184657.png)

![](assets/Pasted%20image%2020241215184857.png)
![](assets/Pasted%20image%2020241215184915.png)
![](assets/Pasted%20image%2020241215184936.png)

- Click on custom install

![](assets/Pasted%20image%2020241215185009.png)
![](assets/Pasted%20image%2020241215184953.png)
![](assets/Pasted%20image%2020241215185038.png)

![](assets/Pasted%20image%2020241215185526.png)
![](assets/Pasted%20image%2020241215185547.png)
![](assets/Pasted%20image%2020241215185604.png)
![](assets/Pasted%20image%2020241215185640.png)

- Click on `Domain join instead`

![](assets/Pasted%20image%2020241215190036.png)
![](assets/Pasted%20image%2020241215190122.png)
![](assets/Pasted%20image%2020241215190220.png)
![](assets/Pasted%20image%2020241215190241.png)
![](assets/Pasted%20image%2020241215190457.png)
![](assets/Pasted%20image%2020241215190513.png)

- After this windows 10 will boot. Follow the same process to create new user named `Ironman`
# [Setup]()

## [Windows server 2022 (DC)]()

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

- Click on Active Directory Domain Services and Add Features.

![](assets/Pasted%20image%2020241215181844.png)
![](assets/Pasted%20image%2020241215181943.png)
![](assets/Pasted%20image%2020241215182011.png)
![](assets/Pasted%20image%2020241215182030.png)
![](assets/Pasted%20image%2020241215182058.png)
![](assets/Pasted%20image%2020241215182203.png)
![](assets/Pasted%20image%2020241215182311.png)

- Click on promote this server to domain controller.

![](assets/Pasted%20image%2020241215182404.png)
![](assets/Pasted%20image%2020241215182506.png)
![](assets/Pasted%20image%2020241215182526.png)
![](assets/Pasted%20image%2020241215182604.png)
![](assets/Pasted%20image%2020241215182635.png)
![](assets/Pasted%20image%2020241215182655.png)
![](assets/Pasted%20image%2020241215182728.png)
![](assets/Pasted%20image%2020241215182750.png)
![](assets/Pasted%20image%2020241215182930.png)
![](assets/Pasted%20image%2020241215183115.png)

### [Setup certificate services]()

- manage > add roles > click next 3 times > select ADCS as shown below

![](assets/Pasted%20image%2020241215183722.png)

- Keep defaults and click next till you see below and click install.

![](assets/Pasted%20image%2020241215183846.png)
![](assets/Pasted%20image%2020241215183910.png)

- Click on Configure AD CS on the destination server.

![](assets/Pasted%20image%2020241215183954.png)
![](assets/Pasted%20image%2020241215184033.png)
![](assets/Pasted%20image%2020241215184051.png)
![](assets/Pasted%20image%2020241215184110.png)
![](assets/Pasted%20image%2020241215184123.png)
![](assets/Pasted%20image%2020241215184138.png)
![](assets/Pasted%20image%2020241215184152.png)
![](assets/Pasted%20image%2020241215184208.png)
![](assets/Pasted%20image%2020241215184228.png)
![](assets/Pasted%20image%2020241215184243.png)

- Click on configure

![](assets/Pasted%20image%2020241215184259.png)
![](assets/Pasted%20image%2020241215184315.png)
![](assets/Pasted%20image%2020241215184341.png)

### [Add Groups OU]()

- First step we'll do is we will create `Groups` OU (organisational unit) and move all the security groups from `Users` to `Groups`
- Click Tools > AD Users and computers.

![](assets/Pasted%20image%2020241215193521.png)

- Create new Organisational Unit called `Groups.

![](assets/Pasted%20image%2020241215193632.png)
![](assets/Pasted%20image%2020241215193805.png)

- Select `Security Groups` and move to `Groups` OU

![](assets/Pasted%20image%2020241215193922.png)
![](assets/Pasted%20image%2020241215194021.png)
![](assets/Pasted%20image%2020241215194034.png)

- Note: logo near Guest shows it is disabled.

### [Create users]()

- In this scenario we will copy administrator to create new user and save some time. (This is a big **no no** as copied user will get all admin access which is dangarous.)
- Right click on Administrator and click copy

![](assets/Pasted%20image%2020241215194251.png)

![](assets/Pasted%20image%2020241215194525.png)
![](assets/Pasted%20image%2020241215194607.png)
![](assets/Pasted%20image%2020241215194625.png)

- Similarly we will create a new service user called `SQLService` by coping administrator.

![](assets/Pasted%20image%2020241215195020.png)
![](assets/Pasted%20image%2020241215195108.png)


## [Windows 10 Enterprise (Client)]()

### [Change adaptor settings]()

- Follow the same process as mentioned above and change the IP to below

```
IP address: 192.168.0.3
Subnet mask: 255.255.255.0
Default gateway: 192.168.0.1

Preferred DNS Server: 192.168.0.2 (DC ip address)
```

![](assets/Pasted%20image%2020241215191533.png)
![](assets/Pasted%20image%2020241215191751.png)

