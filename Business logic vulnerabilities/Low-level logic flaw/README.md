## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e3d012b1-f36b-4f9b-b46c-f6227ad58d90)

## Solution :

Log in as wiener.

Add the leather jacket to our cart. Now add other product also to the cart.

**Add  to cart** sends the following request.

```http
POST /cart HTTP/2
Host: 0ae500ca036e43208a0e1dc600950057.web-security-academy.net
Cookie: session=66vBoLb5gOp0RRyz4awD36C0LmPmoyyC
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: https://0ae500ca036e43208a0e1dc600950057.web-security-academy.net
Referer: https://0ae500ca036e43208a0e1dc600950057.web-security-academy.net/product?productId=2
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

productId=2&redir=PRODUCT&quantity=1
```
Send the request to intruder. Here change the quantitiy value to some other number. 

Now add payload position near the quantity value.

Select **Payload type = NUll paylaods**

> Enable **Continue indefinitly** option.

 ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a9aad06f-5bae-4b90-96ed-0214d1c33936)

> Under `Resource Pool`, set `concurrent requests= 1`

Now click on start attack.

After a few requests we can see that the total price has increased.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/90550a8e-64a1-4cb7-ae4d-7da65b7d4edf)

After letting it some for some more time, we see that the value has changed . The price suddenly switches to a large negative integer and starts counting up towards 0. The **price has exceeded the maximum value permitted for an integer in the back-end programming language (2,147,483,647). As a result, the value has looped back around to the minimum possible value (-2,147,483,648)**. 
