# Practical Ethical Hacking - The Complete Course

                                                   -by TCM (the cyber mentor)
                                                   

## These are some notes and techniques for some tools that have been discussed in these course

## Network Refresher

### IP Addresses

**Types**

* IPv4

* IPv6

**Private IP Address**

These are not used anywher on public internet, reserved for private LANs

Network Class | Network Numbers | Network Mask | No of Networks | No of Hosts per Network
--------------|-----------------|--------------|----------------|------------------------
Class A | 10.0.0.0 | 255.0.0.0 | 126 | 16,646,144
Class B | 172.16.0.0 to 172.31.0.0 | 255.255.0.0 | 16,383 | 65,024
Class C | 192.168.0.0 to 192.168.255.255 | 255.255.255.0 | 2,097,151 | 254
LoopBack (localhost) | 127.0.0.0 to 127.0.0.7 | 255.255.255.0

### MAC Addresses

eg: 00:0c:29:0a:42:05

the first three ```00:0c:29 ``` are the identifiers , they are used to tell the device manifacture.


### TCP, UDP, and the Three-Way Handshake


**TCP**

* these are connection oriented protocol
* it work on three way handshake
    SYN > SYN ACK > ACK


**UDP** 

* these are connection less protocol


### Common Ports and Protocols

**TCP**

* FTP(21)
* SSH(22)
* Telnet(23)
* SMTP(25)
* DNS(53)
* HTTP(80)/HTTPS(443)
* POP3(110)
* SMB(139+445)
* IMAP(143)


**UDP**

* DNS(53)
* DHCP(67,68)
* TFTP(69)
* SNMP(161)



### The OSI Model

* 1 **P** physical - data cables, cat6
* 2 **D** data - switching, MAC addresses
* 3 **N** network - IP addresses, routing
* 4 **T** transport - TCP/UDP
* 5 **S** Session - session management
* 6 **P** presentation - WMV, JPEG, MOV
* 7 **A** application - HTTP, SMTP


### Subnetting 


[Subnet guide](https://github.com/love07oj/tools_and_techniques/edit/main/tcm-sec/Subnet-Guide.xlsx)


[example](https://www.youtube.com/watch?v=ZxAwQB8TZsM)






































