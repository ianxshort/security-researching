## Target: Oopsie
## Platform: HTB
## Date: 06/12/26
## Difficulty: Easy

### Recon 
 
 bash 
 nmap -sC -sV -Pn 10.129.76.27

 Scan found two open ports 
 - 22/tcp ssh (OpenSSH 7.6p1)
 - 80/tcp http (Apache 2.4.29)

 Visited the site and proxied traffic through Burpsuite. Burpsuite's passive sitemap revealed a  hidden login page /cdi-cgi/login, which was not located anywhere on the initial page. 

 Upon visiting the hidden page a login prompt was presented. Clicking the option titled "Login as Guest" under the credential prompt allowed access to a variety of user navigation options. 



### Exploitation

** IDOR + Broken Access Control **





### Post Exploitation


### Key Takeaway


