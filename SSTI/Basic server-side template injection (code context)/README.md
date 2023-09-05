## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/32e8e5a6-92b0-433b-b5a4-ea39af922b73)

## Solution :

In the My-Account page, we have this **Preferred name** functionality.

On selecting *First name* , a POST request is sent as follows.

```http
POST /my-account/change-blog-post-author-display HTTP/2
Host: 0a0600c203153f8e80d2128500f7005e.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 78
Origin: https://0a0600c203153f8e80d2128500f7005e.web-security-academy.net
Referer: https://0a0600c203153f8e80d2128500f7005e.web-security-academy.net/my-account?id=wiener
Cookie: session=L6vbGPm7HzT2Pdds1yaNqsgg31Tr7Auk
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

blog-post-author-display=user.first_name&csrf=XlGz50yl1jqjs2iKGNHYvMTOsHDYCXlK
```

The `blog-post-author-display=user.first_name` seems interesting, it seems like it takes our first name and passes it as data to the backend code then processes it and sends it back. [Code Context].

