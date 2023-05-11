## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1a940179-b958-4d40-84c7-64dde672081d)


## Solution :

When ther lab loads , we see the usual shopping website. Clicking on `My account` takes us to a login page.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/fe68ada1-1668-47f9-bcd7-892351b5b457)

We enter the credentials which is given in the lab description - `wiener:peter`

When we login we get to see the API key of wiener - `T9VQGi3yf81cho2IzdP5jN61hT9zc1YA`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/84a2e6be-c538-4554-8858-8a6d8fd4731e)

To solve the lab , we need to retreive the API key of carlos user.

When we click on `My Account` after loggin in as wiener, a request is sent like this

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8c2b453b-d334-4af8-b1d2-a869eed06f7e)

It contains `?id=wiener` parameter.

Change the value to `carlos` , we get the API key of carlos and thus solved the lab

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b1b61def-c9cc-4865-998f-828d80e7262e)

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/944e43d5-5799-46c4-aced-2f35a3a48854)

 
