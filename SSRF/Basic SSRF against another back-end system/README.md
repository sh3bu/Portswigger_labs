## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0acd4dd9-2089-4dee-9e4c-60b650cceccb)


## Overview :

Another type of trust relationship that often arises with server-side request forgery is where the application server is able to interact with other back-end systems that are not directly reachable by users.

> Most of the internal back-end systems contain sensitive functionality that can be accessed without authentication by anyone who is able to interact with the systems. 

Here, an attacker can exploit the SSRF vulnerability to access the administrative interface of a backend system  by submitting the following request:

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://192.168.0.68/admin
```

## Solution :



