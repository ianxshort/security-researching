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

The application presented a shopping appliation. I navigated to the description page of Product ID 6, which triggered a stock check request used throughout the entirety of the lab


![Product-ID-Page](SQLi_Images/SQLi_home.jpeg)

Enabled Burp Suite Proxy interception and captured the following request: 

![Captured-Request](SQLi_Images/SQLi_post.jpeg)

#### Bypass WAF using encoded entities 

Injected `--'` into the `storeID` field and observed that the application rejected the request stating: `Attack Detected`

![Attack-Detection](SQLi_Images/SQLi_Detect1.jpeg)

The error message suggested the application's WAF was using signature-based detection to block common injection techniques

The WAF's block mechanism seemed to trigger from string matches, because of this I leveraged Burp Suite's extension tool Hackverter to encode the original storeID value of `2` into an XML entity

![Second-Bypass](SQLi_Images/SQLi_secondbypass.jpeg)

Error-free respone and larger units confirmed that XML encoded values bypass the target's WAF 

#### Column Enumeration

To perform UNION BASED SQL injection, the number of columns returned in the injected query must match the number of columns returned in the original query. 
Enumerated the column count to prepare for UNION SELECT SQL Injection

```SQL
1 UNION SELECT NULL
```

Encoded SQL query into XML-entity in order to bypass target WAF 

![Encoded-Union-Enum](SQLi_Images/SQLI_col-enum-encode.jpeg)

Application returned `0 units` for requests containing >1 columns indicating the column count=1

![Column-Count](SQLi_Images/SQLi_col-enum-conf.jpeg)


### Exploitation

#### Credential Exraction

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



#### Authentication

Used adminstrators credentials to log in and solve the lab 

[Flag-Capture](SQLi_Images/SQLi_solve.jpeg)


### Key Takeaways
