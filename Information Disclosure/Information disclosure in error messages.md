## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e9606ff7-36e9-4366-9a19-1c31ed74e971)

## Solution :

Click on any product in the website & monitor the request in burp. When we click on any product, a GET request is sent as follows.

```
GET /product?productId=' HTTP/2
Host: 0a9d00d603eb4f23862fc5ba002c00ba.web-security-academy.net
Cookie: session=OaYIhOmuX3vsYvEVsRjNg2iTSDFwza8x
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a9d00d603eb4f23862fc5ba002c00ba.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

The above GET request has `?productTd=` parameter which is set to the id of the product (1).

We try to input `{';|` which may cause an error if there is some kind of sql injection/SSTI/command injection vulnerability.

As expected we get a **verbose error message** in the response.

```
HTTP/2 500 Internal Server Error
Content-Length: 1675

Internal Server Error: java.lang.NumberFormatException: For input string: "'"
	at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
	at java.base/java.lang.Integer.parseInt(Integer.java:654)
	at java.base/java.lang.Integer.parseInt(Integer.java:786)
	at lab.c.f.o.i.R(Unknown Source)
	at lab.f.u.n.y.F(Unknown Source)
.
.
.	at lab.k.r.lambda$consume$0(Unknown Source)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
	at java.base/java.lang.Thread.run(Thread.java:833)

Apache Struts 2 2.3.31
```
At the end of the response, we have the name & version of the software that is being used in the abckend - `Apache Struts 2 2.3.31`

Submit the answer to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c65174b8-a2ba-4f07-9730-f97b92eb99fe)















