## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6fba9ea2-8a18-4fc8-bd4c-ae3094bf98dc)

## Overview :

A common feature is the option to stay logged in even after closing a browser session. This is usually a simple checkbox labeled something like "Remember me" or "Keep me logged in".

This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cookie.

Some websites assume that if the cookie is encrypted in some way it will not be guessable even if it does use static values. While this may be true if done correctly, naively **encrypting the cookie** using a simple two-way encoding like Base64 offers no protection whatsoever. Even using proper encryption with a one-way hash function is not completely bulletproof.

> If the attacker is able to *easily identify the hashing algorithm*, and *no salt8 is used, they can potentially *brute-force the cookie* by simply 
> hashing their wordlists. This method can be used to bypass login attempt limits if a similar limit isn't applied to cookie guesses. 

## Solution :

CLick on `My-Account` which takes us to the login page which conatins the **Stay-logged-in** cookie. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9935a449-df73-41a9-bd8f-cda6907ec58e)

Login as `wiener:peter`

The request contains a **stay-logged-in=on** parameter.

```http
POST /login HTTP/1.1
Host: 0aee00de039c1dcf826f476b00d500dd.web-security-academy.net
Cookie: session=4aNT56nIC1uAl0Qx34nUoN6y2WDANDOS
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 48
Origin: https://0aee00de039c1dcf826f476b00d500dd.web-security-academy.net
Referer: https://0aee00de039c1dcf826f476b00d500dd.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

username=wiener&password=peter&stay-logged-in=on
```

The next request made contained the **stay-logged-in** cookie.

```http
GET /my-account HTTP/2
Host: 0aee00de039c1dcf826f476b00d500dd.web-security-academy.net
Cookie: session=21ZsoMCCtGAxPDsKIvVbnd5xRAGxdqEt; stay-logged-in=d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0aee00de039c1dcf826f476b00d500dd.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

Once logged in we get a page to update our emailID,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c3e502a4-8669-449b-9a25-b8ee1d382ac5)

### Analysis if stay-logged-in cookie -

The request to `/my-account` had the stay-logged-in cookie. Examining the cookie value in *Inspector Tab* reveals that it is a a **base64 encoded data which contains username and hash of the password**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a852488d-6a9e-4a4f-8894-d403e31c2a3f)






