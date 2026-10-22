# Security Research & Exploitation

**Hands-on vulnerability research, penetration testing labs, and technical writeups.**

`Web Application Security` · `Active Directory` · `Offensive Security` · `CTF Research`

---

## About This Repository

This repository documents my hands-on cybersecurity research across **TryHackMe, Hack The Box, and PortSwigger Web Security Academy**.

My focus is on understanding how vulnerabilities arise, how they can be exploited, and how they can be mitigated.

Each writeup emphasizes:
- **Reconnaissance & Enumeration** — Identifying attack surfaces and gathering relevant information.
- **Vulnerability Analysis** — Understanding the root cause of security weaknesses.
- **Exploitation** — Demonstrating and validating vulnerabilities in authorized lab environments.
- **Lessons Learned** — Documenting technical findings, security implications, and potential mitigations.

## Featured Research Areas

| Research Area | Topics Explored |
|---|---|
| **Active Directory Security** | Kerberos, Delegation, Credential Attacks |
| **SQL Injection** | Authentication Bypass, XML Filter Bypass |
| **Cross-Site Scripting** | DOM XSS, Client-Side Injection |
| **Authentication Security** | Username Enumeration, Timing Attacks |
| **Server-Side Vulnerabilities** | SSRF, Command Injection |

---

## Research Library

### TryHackMe

| Lab | Focus Area |
|---|---|
| [AD Proxy](writeups/thm/ad-proxy.md) | Active Directory / Delegation |
| [Practical SQLi](writeups/thm/practical-sqli.md) | SQL Injection |
| [Recruit](writeups/thm/recruit.md) | Server-Side Request Forgery |

### PortSwigger Web Security Academy

| Lab | Focus Area |
|---|---|
| [SQLi XML Filter Bypass](writeups/portswigger/sqli-xml-filter-bypass.md) | SQL Injection / XML |
| [Username Enumeration via Timing](writeups/portswigger/username_enum_timing.md) | Authentication Security |
| [DOM XSS](writeups/portswigger/dom-xss.md) | Cross-Site Scripting |

### Hack The Box

| Machine | Focus Area |
|---|---|
| [Crocodile](writeups/htb/crocodile.md) | Enumeration / Credential Exposure |
| [Oopsie](writeups/htb/oopsie.md) | Web Exploitation / Privilege Escalation |
| [Three](writeups/htb/three.md) | Cloud Storage Misconfiguration |

---

## Tools & Technologies

**Web Application Security**

`Burp Suite` `SQLmap` `FFUF` `OWASP`

**Network & Enumeration**

`Nmap` `Gobuster` `Linux`

**Active Directory & Credential Security**

`Impacket` `BloodHound` `Kerbrute` `Hashcat`

**Programming & Scripting**

`Python` `Bash` `SQL`

---

##  Research Methodology

**Reconnaissance → Enumeration → Vulnerability Analysis → Exploitation → Reporting**

My approach emphasizes not just achieving successful exploitation, but understanding the underlying security weakness, documenting the methodology, and evaluating the potential impact.

---

## Disclaimer

All security testing is performed within authorized lab environments for educational purposes. Research is intended to demonstrate security concepts, improve technical understanding, and promote responsible security practices.

*Writeups are published subject to the respective training platform's content-sharing policies.*





