# Practical Ethical Hacking - The Complete Course

                                                   -by TCM (the cyber mentor)
                                                   

### These are some notes and techniques for some tools that have been discussed in these course

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


## Subnetting 


[Subnet guide](https://github.com/love07oj/tools_and_techniques/edit/main/tcm-sec/Subnet-Guide.xlsx)


[example](https://www.youtube.com/watch?v=ZxAwQB8TZsM)



## Introduction to Linux



**Change password**

```passwd ```


### Users and Privileges

* if there is a ```d ``` then its mean its a directory

* the ``` rwxr-xr-x ``` the first set of three is for the recent user , the second set is fo the group and last set is for others. Here r-readable, w- writable, x- executable

**chmod**

```chmod +x ```

* 777 is for full permissions

**adduser**

```adduser john ```

**su** --> switch user 

### Common Network Commands 

* ifconfig
* iwconfig
* ping
* arp -a : show the IP address it talks to and MAC associated with them.
* netstat -ano : active connections running on machine
* route : print the routing table


### Viewing, Creating, and Editing Files

**echo**

* ```echo "hello" ```

* for overwrite```echo "hey" > hey.txt ```

* for append ```echo "hey again again" > hey.txt ```


**create a file**

* **touch**
* **nano**
* **vi**
* **vim**
* **gedit**

###  Starting and Stopping Kali Services

* to start a servicce: ```service apache2 start ```
* to start a normal python web server ``` python -m SimpleHTTPServer 80 ````
* to stop ```service apache2 stop ```

* start any service while booting ```systemctl enable postgresql ```


### Installing and Updating Tools


```apt update && apt upgrade ```

* install a specific tool ```apt install python-pip ```

* get a repo from github  ```git clone impaket.github.com ```


### Scripting with Bash

**Narrow down result**

```ping 192.168.123.1 | grep "64 bytes" ```

**Narrow down some more**

```ping 192.168.123.1 | grep "64 bytes" | cut -d " " -f 4 ```

where -d is delimiter which here is delemate all spaces

and -f is the field numer which is 4 in this command

**sweep** 

```ping 192.168.123.1 | grep "64 bytes" | cut -d " " -f 4 | tr -d ";" ```


**writing a script**

```bash
#!/bin/bash
if [ "$1" == "" ]
then
echo "You forgot an IP address!"
echo "Syntax: ./ipsweep.sh 192.168.1"

else
for ip in `seq 1 254`; do
ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &  #here & is used for threading amd $1 is used for getting an input from a user
done
fi #this is if backward to close the if we used earlier above
```

**One liner**

```for ip in $(cat ip.txt); do nmap -sS -p 80 -T4 $ip & done ```


## Introduction to Python


### Strings 

```python3
print("Hello world")
print ("""this strings runs
multiple lines """)

pritn ("This string is "+"awsome")
```

```gedit first.py& ``` this is to open and run the gedit and script at same time

```python first.py ```  this is to run the code


### Math

```python3
print(50+50) #add
print(50-50) #subtract 
print(50*50) #multiply
print(50/50) #divide
print(50 ** 2) # exponents
print(50 % 6) #modulo
print(50 // 6) #no leftovers
```

### Variables and Methods

```python3
quote = "all is well"
print(quote)
print(quote.upper()) #makes the string upper
print(quote.title()) 
print(len(quote))

name = "love07oj" #string
age = 30 #int
gpa = 4.3 #float

print(int(age))
pritn(int(30.1))
print(" my name is" + name + "and I am " + str(age) + " years old ") 

age +=1
print(age)




















