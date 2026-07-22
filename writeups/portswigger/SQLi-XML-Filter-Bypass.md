## Target: 
## Platform: PortSwigger
## Date: 07/07/26
## Difficulty: Practitioner
## Tools: 
- Burp Suite Repeater 
- Burp Suite Proxy 
- Hackvector


### Recon 

**HomePage**

Presented with a shopping appliation and navigated to the description page of product ID:6 


![Product-ID-Page](SQLi_Images/SQLi_home.jpeg)

Turned intercept on in Proxy and captured a POST /product/stock request 

![Captured-Request](SQLi_Images/SQLi_post.jpeg)

#### Bypass WAF using encoded entities 

Inserted `--'` into the `storeID` field and observed that the request had been blocked 

![Attack-Detection](SQLi_Images/SQLi_Detect1.jpeg)

Used the extension tool Hackverter to encode the original storeID value of `2` into XML 

![Second-Bypass](SQLi_Images/SQLi_secondbypass.jpeg)

Confirmed that XML encoded values bypass the target's WAF 

#### Column Enumeration

Enumerated the column count to prepare for UNION SELECT SQL Injection

```SQL
1 UNION SELECT NULL
```

Encoded SQL query into XML-entity in order to bypass target WAF 

![Encoded-Union-Enum](SQLi_Images/SQLI_col-enum-encode.jpeg)

Application returned `0 units` for requests containing >1 columns indicating the column count=1

![Column-Count](SQLi_Images/SQLi_col-enum-conf.jpeg)


### Exploitation

Concatenated username and password from username table 

```SQL
SELECT username || ':' || password FROM users
```

Encoded query into XML entity to bypass WAF

![Exploit-Encoded](SQLi_Images/SQLi_exploit-enc.jpeg)

Observed response and dumped credentials 

![Dump-Creds](SQLi_Images/SQLi_got-creds.jpeg)

- Collected Credentials 

Username: administrator
Password: jm920ccdtmbvmcctlngy



### Room Complete 

Used adminstrators credentials to log in and solve the lab 

[Flag-Capture](SQLi_Images/SQLi_solve.jpeg)


### Key Takeaway
