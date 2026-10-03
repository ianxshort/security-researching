Template 
## Target: Recruit
## Platform: TryHackMe
## Date: 
## Difficulty: Medium
## Tools: 
- gobuster

---

### Recon 

We begin by performing a port scan on the target machine

```bash
nmap -sV 10.145.183.53
```

Port scan reveals a running web service operating on port 80. We investigate further by visiting the servers address on our browser. 

![login-page](recruit-images/recruit_loginpage.jpeg)

We are presented with a login form after navigating to the address. Below the login form, we see a hyperlink titled "Access API"

![API-Access-Page](recruit-images/recruit_accessAPI.jpeg)

It appears the endpoint `file.php` accepts a `cv` URL parameter and retrieves the resource specified. Because the server makes a request based on a user-supplied URL, this makes the endpoint an interesting target for attempting to access resources that should otherwise be inaccessible. 


#### Directory Enumeration

Continuing our web server reconnaissance, we use `Gobuster` to enumerate hidden files and directories.

![Go-Buster](recruit-images/recruit_gobuster.jpeg)
> Outputs shows intriguing pages such as `mail`, `assets` and `phpmyadmin`

We begin by returning to the browser and visiting the `mail` page that was discovered in our `gobuster` scan

![Mail-Page](recruit-images/recruit_hr_username.jpeg)

The mail page reveals several important details about the web application, including username details (`username:hr`) and sensative file information. The mail document states that HR login credentials can be found in `config.php`. In many applications `config.php` holds sensitive information such as database passwords, API keys, and other configuration details. 




Found mail page that revealed hr credential info to investigate 

y


---

### Initial Access 



### Exploiting SSRF for Local File Read 


Server-side URL fetching abused for arbitrart local file read via file:// scheme 




## Privilege Esclation




#### Manual UNION-Based SQL Injection







### Key Takeaway
