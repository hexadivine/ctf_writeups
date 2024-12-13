
# [Requirements]()

- Download [windows server 2022](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022), [windows enterprise](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) and virtual box.

# [Installation]()
## [Windows Server 2022]()

- While creating windows server 2022 on virtual box click on skip unattended installation to manually install.

![](assets/Pasted%20image%2020241209173217.png)

- After successful install of windows server 2022 it should look like below.

![](assets/Pasted%20image%2020241209173529.png)

- Click on add roles and features.

![](assets/Pasted%20image%2020241209173638.png)
- Follow below steps to install ADDS (Active Directory Domain Services)

![](assets/Pasted%20image%2020241209173737.png)
![](assets/Pasted%20image%2020241209173805.png)
![](assets/Pasted%20image%2020241209174229.png)
![](assets/Pasted%20image%2020241209174447.png)
![](assets/Pasted%20image%2020241209174528.png)
![](assets/Pasted%20image%2020241209174620.png)
![](assets/Pasted%20image%2020241209174645.png)
![](assets/Pasted%20image%2020241209174718.png)
![](assets/Pasted%20image%2020241209174731.png)
![](assets/Pasted%20image%2020241209183239.png)

- After that click on promote this server to domain controller. Here we need to create a new forest and set up a password for DSRM.

![](assets/Pasted%20image%2020241209183715.png)
![](assets/Pasted%20image%2020241209183925.png)
![](assets/Pasted%20image%2020241209184325.png)
![](assets/Pasted%20image%2020241209184359.png)
![](assets/Pasted%20image%2020241209184433.png)
![](assets/Pasted%20image%2020241209184511.png)
![](assets/Pasted%20image%2020241209184642.png)
![](assets/Pasted%20image%2020241209184745.png)
![](assets/Pasted%20image%2020241209184933.png)

- After reboot login in to MARVEL/Administrator and use the administrator password set while creating the winserver 2022. 
- After this we will set the certificate services. Used to verify identities on DS and set up secure LDAP. Follow same process Manage > Add roles and features > (next)x3 > 

![](assets/Pasted%20image%2020241209190016.png)
![](assets/Pasted%20image%2020241209190031.png)

- After keeping default and press next 3 times and install.

![](assets/Pasted%20image%2020241209190145.png)
![](assets/Pasted%20image%2020241209190155.png)
![](assets/Pasted%20image%2020241209190242.png)

- Click on configure AD certificate services on destination server.

![](assets/Pasted%20image%2020241209190341.png)
![](assets/Pasted%20image%2020241209190422.png)
![](assets/Pasted%20image%2020241209190432.png)![](assets/Pasted%20image%2020241209190505.png)
![](assets/Pasted%20image%2020241209190546.png)
![](assets/Pasted%20image%2020241209190600.png)
![](assets/Pasted%20image%2020241209190611.png)
![](assets/Pasted%20image%2020241209190627.png)
![](assets/Pasted%20image%2020241209190637.png)
![](assets/Pasted%20image%2020241209190654.png)

- Lastly, reboot the server.

## [User Machines]()

- Similar to windows server, while creating Screenshot at 2024-12-13 12-37-00virtual box for windows enterprise click on skip unattended installation and follow below 

![](assets/Pasted%20image%2020241213123946.png)
![](assets/Pasted%20image%2020241213125146.png)
![](assets/Pasted%20image%2020241213125211.png)
![](assets/Pasted%20image%2020241213125238.png)
![](assets/Pasted%20image%2020241213125258.png)

- After this perform basic setup

![](assets/Pasted%20image%2020241213130339.png)
![](assets/Pasted%20image%2020241213130400.png)
![](assets/Pasted%20image%2020241213130444.png)

 - Click on 'Domain join instead', set up name, password, security question

![](assets/Pasted%20image%2020241213130002.png)
![](assets/Pasted%20image%2020241213130608.png)
![](assets/Pasted%20image%2020241213130713.png)

- Perform same process for another user as we need 2 users.

# [User, Groups & Policies]()

- Bootup windows server vm

## [Create group and move security groups]()

- Go to tools > Active Directory Users and Computers

![](assets/Pasted%20image%2020241213135346.png)
![](assets/Pasted%20image%2020241213135442.png)

- Create a new organisational unit for groups, and move all 'security groups' to Groups OU.

![](assets/Pasted%20image%2020241213135546.png)
![](assets/Pasted%20image%2020241213135615.png)
![](assets/Pasted%20image%2020241213135741.png)
![](assets/Pasted%20image%2020241213135829.png)

## [Create new users and service]()

- In this scenario, we are copying administrator user settings to create new users for simplicity (**which is a big no no as administrator has multiple elevated accesses**)
- Right click on administrator and click on copy.

![](assets/Pasted%20image%2020241213141029.png)
![](assets/Pasted%20image%2020241213141130.png)
![](assets/Pasted%20image%2020241213141230.png)
![](assets/Pasted%20image%2020241213141247.png)

- Perform same settings to create Peter Parker user and SQL Service user.

![](assets/Pasted%20image%2020241213141629.png)
![](assets/Pasted%20image%2020241213141902.png)


