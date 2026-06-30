## Target: Three 
## Platform: HTB
## Date: 06/27/26
## Difficulty: Easy
## Tools: 



### Recon 
 Scanned target for open ports 
 ```bash
sudo nmap -sV 10.129.126.39
```
![Enumeration](Images/three_enumeration.jpeg)

Found two open ports 
- 22/tcp ssh(OpenSSH 7.6p1)
- 80/tcp http (Apache 2.4.29)

Visited the website of the target IP and viewed the page source. 

![Website-Home-Page](Images/three_home.jpeg)

Found contact form containing a reference to a .php page indicating the web application backend is built using php 

![Website-Page-Source](Images/three_pagesource)

Navigated to the broken Contact page which contained an email "thetoppers.htb"

***Subdomain Enumeration***

Began subdomain enumeration of the target by appending "thetoppers.htb"

```bash
gobuster vhosts -w /usr/share/seclists/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb --append-domain=true
```

![Vhost-Enumeration](Images/three_vhostenum.jpeg)

Gobuster revealed that the sub-domain s3.thetoppers.htb exists. Visited the subdomain using a browser to confirm that the service is indeed running.

![S3-Status](Images/three_s3-confirmation.jpeg)


Searched through to find objects within the bucket 

```bash
aws --endpoint=http://thetoppers.htb s3 ls s3://thetoppers.htb
```

![S3-Bucket-Objects](Images/three_bucket-objects.jpeg)







### Exploitation





### Post Exploitation

**Lateral Movement**



**Privilege Escalation**



### Key Takeaway


