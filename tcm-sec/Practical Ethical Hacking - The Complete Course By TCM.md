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
```

### Functions


```python3
def who_am_i():  #this is a function
    name = "love07oj"
    age = 30  
    print(" my name is" + name + "and I am " + str(age) + " years old ")
who_am_i()

#adding parameters
def add_one_hundred(num):
      print(num+100)
add_one_hundred(100)      

#multiple parameters

def add(x,y):
      pritn(x,y)
add(7,7)

def multiply(x,y):
      return x * y
print(multiply(7,7))

```

### Boolean Expressions


```python3
#(true or false)
print("boolean expressioms:")

bool1 = True
bool2 = 3*3 ==9
bool3 = False
bool4 = 3*3 !=9
print(bool1,bool2,bool3,bool4)
print(type(bool1))
```


### Relational and Boolean Operators


```python3
greter_than = 7 > 5
less_than = 5 > 7
greater_than_equal_to = 7 >=7
less_than_equal_to = 7 <= 7

test_and = (7 > 5) and (5 < 7) #True
test_and2 = (7 > 5) and (5 > 7) #False
test_or = (7 > 5) and (5 < 7) #True
test_or2 = (7 > 5) and (5 > 7) #True

test_not = not True #False

```

### Conditional Statements


```python3
def drink(money):
        if money >= 2:
                return "you got a drink!"
         else:
                return "NO drink for you!"
print(drink(3))
print(drink(1))

def alcohol(age,money):
    if(age >= 21) and (money >= 5):
        return "we get a drink"
    elif (age >= 21) and (money < 5):
        return "come back with more money"
    elif (age < 21) and (money >=5):
        return "Nice try kid "
    elif:
         return "you are too poor and too young"
print(alcohol(21,5))
print(alcohol(21,4))
print(alcohol(20,4))
print(alcohol(20,5))

```

### Lists


* these are data structures
* everything inside them are called item
* they have brackets []

```python3 
movies = ["abcd","wxyz","ijkl"]
print(movies[0])
print(movies[0:2]) #stop after 2nd item
print(movies[0:]) #print all list items
print(movies[:2] #anyting before 2nd item
print(movies[-1] #last item of list 
print(len(movies)
movies.append("qwer")
print(movies)

movies.pop() #delete from last 
print(movies)

movies.pop(0) #delete from position 
print(movies)
```

### Tuples

* they are imutable (cant be changed)

```python3
grades = ("a", "b", "c", "d", "f")
print(grades[1])
```

### Looping


```python3

vegetable = ["cucumber", "spanish", "cabbage"]
for x in vegitables:
    print(x)

i = 1

while  i < 10:
      print(i)
      i += 1
```


### Importing Modules

```python3
import sys #system function and parameters
import datetime import datetime as dt #import with alias

print(sys.version)
print(dt.now())

```

### Advanced Strings


```python3 

my_name = "Heath"
print(my_name[0])
print(my_name[-1])

sentence = "This is a sentence."
print(sentence[:4])

print(sentence.split())

sentence_split = sentence.split()
sentence_join = ' '.join(sentence_split)
print(sentence_join)

quote = "He said, \"give me all your money\""  #chracter escaping 
print(quote)

too_much_space = "                      hello                        "
print(too_much_space.strip())

print('A' in "Apple") #does A is there in word apple

letter = "A"
word = "Apple"
print(letter.lower() in word.lower()) 

movie = "The Hangover"
print("My favourite movie is {}.".format(movie))

```

### Dictionaries


```python3
drink = {"white Russian": 7, "old Fashion": 10, "Lemon Soda": 8}
print(drink)

employees = {"Finance": ["Bob", "Linda", "Tina"], "IT": ["Gene", "Louise", "Teddy"], "HR":["Jimmy","Mort"]}
print(employees)
employees['Legal']=["Mr. Frond"] #add new key:value pair
print(employees)

employees.update({"Sales":["Andie","Oilie"]}) #add new key:value pair
print(employees)
```


### Sockets


```python3
#!/bin/python3

import socket

HOST = "127.0.0.1"
PORT = 7777

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST,PORT))
```















































