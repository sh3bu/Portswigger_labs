## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2831b77f-66b6-4d2b-a3cd-d1b8b6676942)


## Solution :

When the lab website loads, we straighaway see the **Admin panel** link present.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6192ae47-7d9b-4867-8452-a8e71312b635)

When we click it we get **Access Denied**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d491435a-6df6-4dce-bf4e-8fdbb1005228)

We capture the request in burp .It looks like

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0c40e61d-2744-4374-b599-6da31d14bb5f)

The hint given in the lab description helps us to bypass this ,

> Hint - The back-end application is built on a framework that supports the `X-Original-URL` header.

Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as `X-Original-URL` and `X-Rewrite-URL`. If a web site uses rigorous front-end controls to restrict access based on URL, but the application allows the URL to be overridden via a request header, then it might be possible to bypass the access controls using a request like the following:

```http
POST / HTTP/1.1
X-Original-URL: /admin/deleteUser
```

Top bypass this 403, we should remove the **/admin** which is with the **GET** method in the request & add the **X-Original-Url : /admin** to the body of the request.

Common mistake is that we add the endpoint both in the X-Original-Url & also at the GET method. 

> [Ref - https://github.com/sting8k/BurpSuite_403Bypasser/issues/4]

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b6e47af5-7e2a-4e2d-aa3d-9ab463000698)


So the request which we send has to look like this ,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/148686a5-6e42-4b1d-a39f-2f6a0a1232a0)

Thus we got the admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cc922759-d1c6-40f0-9c38-5b33147d40a2)

If we click on the link to delete user carlos, we again get *access-denied*.

So we have to add the custom header in this request too !

#### Actual request - 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d6476bc9-ad92-4194-aef8-5a024a3a6ac1)


#### Modified request

- Add the `X-Original-Url: /admin/delete` header
- Provide the `username=carlos` parameter as it is in the real query string.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3a95314a-ffe9-4646-8613-9b40642963fe)



![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c75449e0-ae59-474b-953a-64196706b59f)

