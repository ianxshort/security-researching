Template 
## Target: Recruit
## Platform: TryHackMe
## Date: 
## Difficulty: Medium
## Tools: 
- gobuster



### Recon 

We begin by performing a port scan on the target machine

```bash
nmap -sV 10.145.183.53
```

Port scan reveals a running web service operating on port 80. We investigate further by visiting the servers address on our browser. 

![login-page](recruit-images/recruit_loginpage.jpeg)

We are presented with a login form after navigating to the address. Below the login form, we see a hyperlink titled "Access API"

![API-Access-Page](recruit-images/recruit_accessAPI.jpeg)

It appears the endpoint `file.php` accepts a `cv` URL parameter and retrieves the resource specified by the URL. Because the server makes a request based on a user-supplied URL, this makes the endpoint an interesting target for attempting to access resources that should otherwise be inaccessible. 


#### Directory Enumeration

Continuing our web server reconnaissance, we use `Gobuster` to enumerate hidden files and directories.

![Go-Buster](recruit-images/recruit_gobuster.jpeg)




#### Directory Enumeration



Found mail page that revealed hr credential info to investigate 





### Initial Access 



### Exploiting SSRF for Local File Read 


Server-side URL fetching abused for arbitrart local file read via file:// scheme 




## Privilege Esclation




#### Manual UNION-Based SQL Injection







### Key Takeaway
