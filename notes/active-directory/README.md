![](assets/Pasted%20image%2020241207124120.png)
# [Introduction]()

Directory service was developed by Microsoft to manage windows domain networks. Roughly **90%** of organisations worldwide use Active Directory as the user identity repository, acting as the primary source of trust for identity and access. It stores information related to objects such as computers, users, printers (like a phonebook, or identity management system). Authenticates using Kerberos tickets (non windows devices like Linux, firewalls, printers can be authenticated using Active Directory (AD) via RADIUS or LDAP)

AD can be exploited without ever attacking pachable exploits. Instead attacker abuse features, trusts components and more.

# [Active Directory Components]()

## [Physical Controller]()

This include various components such as domain controller, data store, global catalogue server, read-only domain controller.

### [Domain Controller]()

It controls everything. It host copy of active directory domain service (AD DS). Provides authentication & authorisation services. It replicates updates to other domain controller in the domain and forest. Allows admin access to manage user accounts and network resources.

![](assets/Pasted%20image%2020241207133558.png)

### [Data Store]()

This contains the database files and process that stores and manage directory information for users, services and applications. This contains Ntds.dit file. This is stored by default in `%SystemRoot%\NTDS` folder on all domain controllers. It is accessible only through domain controller process and protocols.

![](assets/Pasted%20image%2020241207133621.png)
## [Logical Controller]()

This includes components such as partitions, schema, domains, domain trees, forests, sites, Organisation units.

### [AD Schema]()

It's a like a blueprint. Defines every type of schema that can be stored in the directory. Enforces rules regarding object creation and configuration.

![](assets/Pasted%20image%2020241207133435.png)

### [Domains]()



![](assets/Pasted%20image%2020241207133800.png)