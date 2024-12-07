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

There could be multiple domains. DC controls such domains. A domain is a logical group of objects (like users, groups, and computers) that share a common directory database.
Domains are the basic unit of administration in Active Directory.

![](assets/Pasted%20image%2020241207133800.png)

### [Trees]()

It follows hierarchical structure of domains. A tree is a collection of one or more domains that share a common schema, global catalogue, and directory configuration. Domains in a tree are connected in a transitive trust relationship, meaning that if domain A trusts domain B, and domain B trusts domain C, then domain A will automatically trust domain C.

![](assets/Pasted%20image%2020241207170745.png)

### [Forest]()

A forest is a broader term that encompasses a collection of one or more trees. All trees in a forest share a common global catalogue, directory schema, and configuration, but they can have different domain names. A tree is a subset of a forest that shares a contiguous namespace.

![](assets/Pasted%20image%2020241207170812.png)

### [Organisational Unit]()

An Organisational Unit (OU) in Active Directory (AD) is a container object used to organise and manage resources within a domain. It allows administrators to group objects (such as users, computers, groups, and printers) for easier management and delegation of administrative tasks. OUs provide a way to structure AD in a way that reflects the organisational hierarchy or functional divisions within an organisation.

![](assets/Pasted%20image%2020241207171345.png)

### [Trusts]()

In Active Directory (AD), a trust is a relationship established between two domains (or forests) that enables users in one domain to access resources in another domain. Trusts allow for authentication to be shared across domains, providing a way to access resources in different domains without needing separate credentials for each.

![](assets/Pasted%20image%2020241207171611.png)

### Objects

An **object** is a distinct entity or resource that is stored within the directory and has a specific set of attributes. These objects can represent users, computers, groups, printers, organizational units (OUs), and more. Each object in Active Directory has a unique **distinguished name (DN)**, which helps to identify it within the directory.

![](assets/Pasted%20image%2020241207172018.png)