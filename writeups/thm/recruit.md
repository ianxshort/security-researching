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

The `/mail` directory revealed a `mail.log` file containing information about the HR account. The log disclosed the username hr and indicated that the corresponding credentials could be found in `config.php`.

This gave us two useful pieces of information: a valid application username and a specific configuration file worth targeting. Since config.php commonly contains application configuration and credential material, I treated it as a high-value target.



---

### Exploiting the Access API 


We return to the `file.php` lead we investigated earlier. The endpoint accepts a URL through the `cv` parameter, causing the server to retrieve the specified resource. I began by manually probing the endpoint with arbitrary values, followed by testing HTTP and HTTPS URI schemes. During my testing I encountered an error stating "only local files are allowed".

![Error-Response](recruit-images/recruit_LFI_error.jpeg)

 This error changed my approach and I began looking for ways in which a local file could be represented as a URI. The `file://` URI scheme is used to locate and access files on the local file systems, which made it a fitting candidate given the applications error response.

 We then sent a GET request to the `file.php` endpoint, specifying `config.php` as the target resource using the `cv` parameter.


![LFI](recruit-images/recruit_hr_password.jpeg)

The GET request returned a `200 OK` status confirming that the resource was successfully retrieved. The response body contained the contents of `config.php`, including the HR credentials.


## Privilege Escalation




#### Manual UNION-Based SQL Injection







### Key Takeaway
