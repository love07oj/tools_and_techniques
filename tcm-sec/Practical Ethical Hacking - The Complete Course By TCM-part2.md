# Practical Ethical Hacking - The Complete Course By TCM - Part 2

## Active Directory Overview

**What is Active Directory?**

* Directory service developed by Microsoft to manage windows domain network
* stores info related to objects, such as computer , users, printers, etc.
  * think about it as a phonr book for windows
* Authenticates using Kerberos tickets.
  * non-windows devices, such as Linux machines, firewalls, etc . can also authenticate to Active Directory via RADIUS or LDAP
  
**Why Active Directory ?**

* Active Directory is the most commonly used identity management service in the world
  * 95% of Fortune 1000 companies implement the service in their network
* Can be exploited without ever attacking patchable exploits.
  * insted, we abuse features, trust, components, and more.


### Physical Active Directory Components

**Domain Controllers**

* A domain controller is a server with AD DS server role installe that has specifically been promoted to a domain controller

* Host a copy of the AD DS directory store
* Provide authentication and authorization serivces
* Replicate updates to other domain controllers in the domain and forest
* Allow administrative access to manage user account and network resources

**AD DS Data Store**

* The AD DS data store contains the database files and processes that store and manage directory info for users, service, and applications.

* The AD DS data store:
  * Consist of the NTDS.dit file
  * it stored by default in the %SystemRoot%\NTDS folder on all domain controllers
  * us accessible only through the domain controller processes and protocols.


### Logical Active Directory Components

**AD DS Schema**

* Defines every type of object that can be stored in the directory 
* Enforces rules regulations object creation and configuration


**Domain**

* domains are used to group and manage object in an organization 

* an administrative boundary for applying polices to group of objects
* a replication boundary for replacing data between domain controllers
* an Authetication and all authorization boundary that provides a way to limit the scope of access to resources.


**Tree**

* A domain tree is a hierarchy of domain in AD DS

* All domains in the tree:
  * shares a contiguous namespace with the parent domain
  * can have additional child domains
  * By default create a two-way transitive trust with other domains






















