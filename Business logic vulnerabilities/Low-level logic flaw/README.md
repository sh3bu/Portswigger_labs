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

The maximum quantity is 99. If we add 100 items to cart, it shows this error message.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/db7874ba-68a7-41b5-a679-1745f77383c5)


Send the request to intruder.  

- Now add payload position near the quantity value.
- Select **Payload type = NUll paylaods**
- Enable **Continue indefinitly** option.
- Start attack

 ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a9aad06f-5bae-4b90-96ed-0214d1c33936)

> Under `Resource Pool`, set `maximum concurrent requests= 1`

Now click on start attack.

After a few requests we can see that the total price has increased.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/90550a8e-64a1-4cb7-ae4d-7da65b7d4edf)

After letting it some for some more time, we see that the value has changed . The price suddenly switches to a large negative integer and starts counting up towards 0. The **price has exceeded the maximum value permitted for an integer in the back-end programming language (2,147,483,647). As a result, the value has looped back around to the minimum possible value (-2,147,483,648)**. 

### Exploiting this low level logic flaw -

Send the POST request sent to /cart to intruder tab.

- Add payload position next to value of `quantitiy=99`.
- Set payload type as  *NUll payload*.
- Set count to **323**
- Under **Reseource Pool**, set **Maximum concurrent requests to 1**.
- Start  attack.

Once the attack is finished we have the price in negative value.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8e699b8f-8925-417a-bff4-54ecababc59f)

Now onto stage 2 ,

- Change  `quantity=1` for  the leather jacket.
- Send the request for **47 times**.

Now the price is comparatively easy for us to bring it down to an amount within our $100 store credit.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6127518c-e0a9-4c19-b15c-68b94aee7f73)

Add another product and **send it multiple times so that the negative values change to a positive values  & is also within $100 dollars.**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c2e91969-df72-4932-b8da-ce58458baadf)

Now we can buy a huge quantity of leather jackets just for *$5* !.

Place the order to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5597dc57-9361-4a6a-9687-c9ec958b1e4e)


  
