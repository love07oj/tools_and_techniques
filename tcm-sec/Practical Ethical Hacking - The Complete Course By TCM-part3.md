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
































