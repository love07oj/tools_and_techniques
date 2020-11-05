# Practical Ethical Hacking - The Complete Course By TCM- Part 3

## Web Application Enumeration, Revisited

### Installing Go

* golang downloads
* get the linux amd64 
* ```tar -xvf gol.13.5.linux -C /usr/local ```
* ```chown -R root:root /usr/local/go ```
* set path 
    * ```
      Edit your ~/.bashrc and add:

      export GOPATH=$HOME/go 

      export GOROOT=/usr/local/go

      export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

      Save and type “source ~/.bashrc”
      ```
      
### Finding Subdomains with Assetfinder

* get tomnomnom/Assetfinder from github
* ```assetfinder tesla.com >> tesla-subs.txt```
* ```assetfinder --subs-only tesla.com ```

* make a run.sh

```bash
#!/bin/bash

url = $1

if [! -d "$url" ];then
        mkdir $url
fi

if [! -d "$url/recon" ];then
        mkdir $url/recon
fi


echo "(+) Harvesting Subdomains with Assetfinder...."
assetfinder  $url >> $url/recon/assets.txt
cat $url/recon/asset.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt

```

### Finding Subdomains with Amass

* github OWASP/Amass
* ```amass enum -d tesla.com ```

* edit the run.sh

```bash
#!/bin/bash

url = $1

if [! -d "$url" ];then
        mkdir $url
fi

if [! -d "$url/recon" ];then
        mkdir $url/recon
fi


echo "[+] Harvesting Subdomains with Assetfinder...."
assetfinder  $url >> $url/recon/assets.txt
cat $url/recon/asset.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt

#echo "[+] Harvesting Subdomains with Amass...."
#amass enum -d $url >> $url/recon/f.txt
#sort -u $url/recon/f.txt >> $url/recon/final.txt
#rm $url/recon/f.txt

```

### Finding Alive Domains with Httprobe

* get tomnomnom/httprobe 
* ``` assetfinder tesla.com | httprobe -s -p https:443 |sed 's/https\?:\/\///' | tr -d ':443' ```
* edit the run.sh

```bash
#!/bin/bash

url = $1

if [! -d "$url" ];then
        mkdir $url
fi

if [! -d "$url/recon" ];then
        mkdir $url/recon
fi


echo "[+] Harvesting Subdomains with Assetfinder...."
assetfinder  $url >> $url/recon/assets.txt
cat $url/recon/asset.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt

#echo "[+] Harvesting Subdomains with Amass...."
#amass enum -d $url >> $url/recon/f.txt
#sort -u $url/recon/f.txt >> $url/recon/final.txt
#rm $url/recon/f.txt

echo "[+] Probing for alive domain...."
cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 |sed 's/https\?:\/\///' | tr -d ':443' >>$url/recon/alive.txt

```

### Screenshotting Websites with GoWitness

* ```gowitness single --url=https://tesla.com ```


### Automating the Enumeration Process


* sumrecon: https://github.com/thatonetester/sumrecon

* TCM's modified script - https://pastebin.com/MhE6zXVt




## Testing the Top 10 Web Application Vulnerabilities


### The OWASP Top 10 and OWASP Testing Checklist


* OWASP Top 10: https://owasp.org/www-pdf-archive/OWASP_Top_10-2017_%28en%29.pdf.pdf

* OWASP Testing Checklist: https://github.com/tanprathan/OWASP-Testing-Checklist

* OWASP Testing Guide: https://owasp.org/www-project-web-security-testing-guide/assets/archive/OWASP_Testing_Guide_v4.pdf


### Installing OWASP Juice Shop


* Installing Docker on Kali: https://medium.com/@airman604/installing-docker-in-kali-linux-2017-1-fbaa4d1447fe

* OWASP Juice Shop: https://github.com/bkimminich/juice-shop


### Installing Foxy Proxy

* foxyproxy standard for firefox

### Exploring Burp Suite

* turbo intruder
* Active Scan ++
* Retired js


###  Introducing the Score Board

* OWASP Score Board
   * localhost:3000/#/score-board

###  SQL Injection Attacks Overview 

* **What is SQL Injection?**

   * SQL injection is an attack in which malicious SQL statement are injected into a SQL database.
   * SQL inkection is easy to avoid , but still happens often!
   * If Successful, we can read sensitove databases , extract information, modify datavases, and potrntially even get a shell.
 
 * **Common SQL Verbs** 
  
   * SQL statement begin with verbs. Let's take a look at a few common verbs:
   
      * SELECT - Retrives data from a table
      * INSERT - Adds data to a table
      * DELETE - Removes data from a table 
      * UPDATE - Modifies data in a table
      * DROP - Delete a table
      * UNION - Combines data from multiple queries
  
  * **Other Common Terms**
  
    * A few extra terms to know:
      
      * WHERE - Filters records based on specific condition
      * AND/OR/NOT - Filter records based on multiple conditions
      * ORDER BY - Sorts records in assending/descending order
      
  * **Special Characters**
  
    * Lets talk some important special characters
      
      * ' and " - string delimiters
      * --,/*,and # - comment delimiters
      * * and % - wildcards
      * ; - Ends SQL statement
      * Plus a bunch of others that follow programmatic logic- =,+,>,<,(),etc  
   
* OWASP A1-Injection: https://www.owasp.org/index.php/Top_10-2017_A1-Injection


### SQL Injection Walkthrough

### SQL Injection Defenses 

* Defense 1: Parameterized Statements
   * Ensure imputs(parameters) are used safely in  SQL statement
   * Example: " SELECT * FROM users WHERE email =?";
   * Example: "SELECT * FROM users WHERE email = '" + email+ "';
   
* Defense 2: Santizing Input


### Broken Authentication Overview and Defenses

* OWASP A2-Broken Authentication: https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication

### Testing for Broken Authentication

### Sensitive Data Exposure Overview and Defenses

* OWASP A3-Sensetive Data Exposure: https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure

### Testing for Sensitive Data Exposure

* use nmap

```nmap --script=ssl-enum-ciphers -p 443 tesla.com ```

### XML External Entities (XXE) Overview

* OWASP A4-XML External Entities: https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)

* attacking systms that parse XML input
* Abuse SYSTEM entity and get malicious
* Attacks include Denial of Service, local file disclosure, remote code execution, and more


```xml
<?xml version = "1.0" encoding="ISO-8859-1"?> 
<!DOCTYPE gift[
       <!ENTITY from "Love07oj&Heath">

]>
<gift>
   <To>Frank</To>
   <From>&from;</Form>
   <Item>Pokemon Cards</Items>
</gift>  
```
* XXE payloads
      * payload all of thethings and look for classic XXE 
      

### XXE Attack and Defense


* File Uploads
   * try to upload a blacklisted files

* Disable  DTD's (external entites)


### Broken Access Control Overview

* OWASP A5-Broken Access Control: https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control

* IDOR (insecure data object reference)

### Broken Access Control Walkthrough

* look for id field

### Security Misconfiguration Attacks and Defenses

* OWASP A6-Security Misconfigurations: https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration

###  Cross-Site Scripting (XSS) Overview

* OWASP A7-Cross Site Scripting: https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)

* DOM Based XSS: https://www.scip.ch/en/?labs.20171214

* XSS Game: https://xss-game.appspot.com/

* 
```php
<?php
$username = $_GET['userename']
echo = Hi $username!";
?>
```

```index.php?username=<script>alert(1)</script> ```


### Reflected XSS Walkthrough

### Stored XSS Walkthrough

* there may be a filtering going on in background 


### Preventing XSS

**How to Prevent XSS**


* 1. **ENCODING**
      * ```<becomes&lt; ```
      * ```<scripy>becomes &lt;script> ```
* 2. **Filtering**
      * ```<script> brcomes script ```
* 3. **Vaildating**
      * Compare input against white list
* 4. **Sanitization**
      * Combination of escaping, filtering, and validation
   
   
   
### Insecure Deserialization

* OWASP A8-Insecure Deserialization: https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization

* ysoserial tool


### Using Components with Known Vulnerabilities

* OWASP A9-Using Components with Known Vulnerabilities: https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities

* Nessus

* Shodan
* Always Patch


### Insufficient Logging and Monitoring


* OWASP A10-Insufficient Logging & Monitoring: https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A10-Insufficient_Logging%252526Monitoring.html




## Wireless Penetration Testing

### 001_Wireless_Penetration_Testing_Overview

* Assesent of Wireless network
      * WPA2 PSK
      * WPA2 Enterprise
* Activities performed
      * Evaluating strength of PSK
      * Reviewing nearby network 
      * Assessing guest networks
      * Checking network access
      
* Tools Being Used
      * Wireless card
      * Router
      * Laptop

* Hacking Process (WPA2 PSK)
      * Place - Place wireless card into monitor mode
      * Discover - Discover information about network
            * Channel
            * BSSID
      * Select - Select network and capture data
      * Perform - Perform deauth attack
      * Capture - Capture WPA handshake
      * Attempt - Attempt to crack the handshake
      
### 002_WPA_PS2_Exploit_Walkthrough

* place
   * ```airmon-ng check kill ```
   * ```airmon-ng start wlan0 ```

* Discover 
   * ```airodump-ng wlan0mon ```

* Select 
   * ```airodump-ng -c 6 --bssid 50:CA:BA:8A:00:73 -w capture wlan0mon ```
   
* Perform
   * ```aireplay-ng -0 1 -a 50:CA:BA:8A:00:73 -c 50:00:00:AF:00:73 ```
   
* Capture 
   * we dont need to deauth if we capture enough data but it take time
   
* Attempt
   * ```arircrack-ng -w rockyou.txt -b 50:CA:BA:8A:00:73 capture02.cap ```
   


## Legal Documents and Report Writing

### 001_Common_Legal_Documents

* Sales
   * Mutual Non-Disclosure Agreement(NDA)
   * Master Service Agreement (MSA)
   * Statement of Work (SOW)
   * Other: Sample Report, Recommendation Letters,etc.

* Before You Test
   * Rules of Engagement(ROE)

* After You Test
   * Findings Report
   


### 002_Pentest_Report_Writing


* Sample Pentest Report: https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report



## Life/Career Advice


* Always set goals for yourself and stay motivated
* Do not become complacent
* Never be afraifd to apply to jobs you're unqualified for 
* Be willing to admit that you don't know 
* Be willing to prove yourself
* Determine what you want from a job and only apply to those that meet your criteria
* Strive to be the dumbest guy or girl in the room 
* Network , Netowrk, network

















