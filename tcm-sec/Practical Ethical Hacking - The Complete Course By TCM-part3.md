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

### 



















































