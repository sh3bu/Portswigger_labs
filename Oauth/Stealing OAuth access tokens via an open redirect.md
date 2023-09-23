## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b18ff067-d52b-4107-8230-92fda78a6d32)

## Overview :

## Solution :

### Openredirect vulnerability in the same domain -

When we click on a blog post, we have the option at the bottom of the page called **Next post**. This has a **openredirect vulnerability.** On clicking the button, a *Get* request is sent as follows,

```http
GET /post/next?path=/post?postId=5 HTTP/2
Host: 0a5d001e03be99e881e248de00430091.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: https://0a5d001e03be99e881e248de00430091.web-security-academy.net/post?postId=4
Cookie: session=heLQIqwBKAilLmexsIGfhl0l7Kj5YlH2
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```
On changing the path patth parameter to **https://google.com** , we have a redirect to google.com. This confirms that it is vulnerable to openredirect.

```http
HTTP/2 302 Found
Location: http://google.com
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5be335f2-1c1d-4eb6-ba5f-496e663c7b3e)

### Chaining Openredirect with Oauth to leak tokens -

We can login to the site via social media. The authorization request sent is as follows.

```http
GET /auth?client_id=h7r0ea0g4w5u7fp3o7f8g&redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback&response_type=token&nonce=-1658510961&scope=openid%20profile%20email HTTP/1.1
Host: oauth-0aa300b503cb99fd81f0468d02cf0006.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: close
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
```

Here if we give the **redirect_uri=https://google.com**, we get a server error.

```http
HTTP/2 400 Bad Request
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Content-Type: text/html; charset=utf-8
Date: Sat, 23 Sep 2023 20:05:44 GMT
Keep-Alive: timeout=5
Content-Length: 2190
```
This means there is some kind of pretection to prevent stealing of tokens.

We can chain it using the openredirect vuln we found earlier to exploit this & steal admin's token.

The redirect_uri value is `redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback` in the authorization request.

- If we give `redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback/post/next?path=google.com`, we still don't get an redirect.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/74cb4bdd-6997-4775-8873-369fbc4f520e)

> This is because the uri for the next parameter should be `website.com/post/next?path=` and not `website.com/oauth-callback/post/next?path=` as this is invalid uri !

### Directory traversal -

Now we can use directory traversal to traverse up a directory & try to see if redirect works.

