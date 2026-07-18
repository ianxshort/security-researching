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






### Exploitation






### Key Takeaway
