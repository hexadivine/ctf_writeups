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

- In this scenario we will copy administrator to create new user and save some time. (This is a big **no no** as copied user will get all admin access which is dangerous.)
- Right click on Administrator and click copy

![](assets/Pasted%20image%2020241215194251.png)

![](assets/Pasted%20image%2020241215195602.png)
![](assets/Pasted%20image%2020241215195639.png)
![](assets/Pasted%20image%2020241215195658.png)

- Similarly we will create a new service user called `SQLService` by coping administrator.

![](assets/Pasted%20image%2020241215195020.png)
![](assets/Pasted%20image%2020241215195108.png)

- After this we will create low privileged users. 
- Click New > User create `Tony Stark` and `Peter Parker`

![](assets/Pasted%20image%2020241215200037.png)
![](assets/Pasted%20image%2020241215200245.png)
![](assets/Pasted%20image%2020241215200411.png)

- Finally it should look like below.

![](assets/Pasted%20image%2020241215201016.png)

### [Setup File share]()

- Go to File and storage Services > Shares > Tasks > New Share

![](assets/Pasted%20image%2020241215201336.png)

![](assets/Pasted%20image%2020241215201420.png)
![](assets/Pasted%20image%2020241215201439.png)
![](assets/Pasted%20image%2020241215201515.png)
![](assets/Pasted%20image%2020241215201533.png)
![](assets/Pasted%20image%2020241215201547.png)
![](assets/Pasted%20image%2020241215201600.png)
![](assets/Pasted%20image%2020241215201614.png)

### [Setup SPN]()

The `setspn` command is used to manage Service Principal Names for accounts in Active Directory. SPNs are unique identifiers for services running on servers, allowing clients to authenticate to those services using Kerberos authentication.

```
setspn -a MARVAL/SQLService.MARVAL.local:60111 AVENGERS\SQLService
```

![](assets/Pasted%20image%2020241215202057.png)

- `-a`: This option is used to add a new SPN.
- `MARVAL/SQLService.MARVAL.local:60111`: This specifies the SPN being added. It follows the format `serviceclass/hostname:port`, where:
    - **serviceclass**: Indicates the type of service (e.g., MARVAL).
    - **hostname**: The fully qualified domain name (FQDN) of the server hosting the service.
    - **port**: The port number on which the service is listening (in this case, 60111).
- `AVENGERS\SQLService`: This is the account (user or computer) to which the SPN is being assigned

### [Setup Group Policy]()

- For this lab we will disable Microsoft firewall to launch various attacks (not safe). This policy will be pushed to entire domain.

![](assets/Pasted%20image%2020241215202742.png)
![](assets/Pasted%20image%2020241215202854.png)

- Right click on domain and click Create a GPO in this domain...

![](assets/Pasted%20image%2020241215203138.png)
![](assets/Pasted%20image%2020241215203250.png)

- Right click on newly created GPO and click Edit

![](assets/Pasted%20image%2020241215203328.png)
![](assets/Pasted%20image%2020241215203536.png)

- Double click on `Turn off Microsoft Defender Antivirus` click `enabled` and apply.

![](assets/Pasted%20image%2020241215203739.png)
![](assets/Pasted%20image%2020241215203841.png)

- After this close and click on enforced

![](assets/Pasted%20image%2020241215204000.png)
![](assets/Pasted%20image%2020241215204018.png)

![](assets/Pasted%20image%2020241215204035.png)

- Windows servers has been successfully setup.

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

### [Login to domain]()

- Search for domain > connect.

![](assets/Pasted%20image%2020241215211035.png)
![](assets/Pasted%20image%2020241215211139.png)
![](assets/Pasted%20image%2020241215211214.png)

- We will click on `Join this device to a local AD domain`

![](assets/Pasted%20image%2020241215211324.png)
![](assets/Pasted%20image%2020241215211405.png)
![](assets/Pasted%20image%2020241215211436.png)

- After this restart the device and login to to `MARVEL\Administrator` account.

![](assets/Pasted%20image%2020241215223655.png)

- We can confirmed that the clients are connected from DC

![](assets/Pasted%20image%2020241215223737.png)

### [Update local users and groups]()

- Update local groups and users on both `spiderman` and `ironman` VM. For this; on `spiderman` VM, login to `MARVEL\Administrator` account.

![](assets/VirtualBox_Spiderman_15_12_2024_23_53_28.png)

- Search edit local user and groups

![](assets/Pasted%20image%2020241215224900.png)

- Set password for Administrator

![](assets/Pasted%20image%2020241215225025.png)
![](assets/Pasted%20image%2020241215225300.png)

- After setting the password double click on `Administrator` and on click on `Account is disabled` to unselect and apply (In best practice scenarios it should be disabled.)

![](assets/Pasted%20image%2020241215225430.png)

- After this add `pparker` to local administrator group.

![](assets/Pasted%20image%2020241215225954.png)
![](assets/Pasted%20image%2020241215230026.png)

- Click on add and search for `pparker` and click on check names. 

![](assets/Pasted%20image%2020241215230137.png)

- After this click on ok to add the user to local admin group. 
- Similarly add `tstark` to `spiderman` vm (for further attack vectors)

![](assets/Pasted%20image%2020241215232337.png)

- You should see below. Next, click on apply.

![](assets/Pasted%20image%2020241215232418.png)

- In the `ironman` VM follow same process (login to `MARVEL\Administrator` on `ironman` VM, change admin password and enable it as shown for `spiderman` VM) 

- In the `Administrator Properties` window for `Ironman` VM add only `tstark`.
- It should look like below on `ironman` VM.

![](assets/Pasted%20image%2020241215232638.png)

### [Map network drive]()

- Login to `.\spiderman`

![](assets/Pasted%20image%2020241215233620.png)

- Here we will map a network drive for further attack vectors.

![](assets/Pasted%20image%2020241215234102.png)

![](assets/Pasted%20image%2020241215234140.png)
![](assets/Pasted%20image%2020241215234206.png)

- The setup is now completed.