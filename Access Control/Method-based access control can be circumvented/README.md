## Lab Decription :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d964f901-24f1-4c01-8336-90ce13bdd4ed)


## Solution :

In this lab we exploit the flaw of  **Method based access control**. Some websites may enforce access control to restrict access to specific URLs and HTTP methods based on the user's role like ,

```http
DENY: POST, /admin/deleteUser, managers
```

Here it blocks the *GET* requests to the  URL */admin/delete*.

Using method based access control, we can *bypass this by changing the HTTP request method*


### Steps -

The lab tells us to get familiarized with the admin page with the credentials `administrator:admin`.

Now we have logged in as administrator.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bc2ae963-230b-4a67-a090-368599943182)

We see that there is a link to *admin panel*. Clicking on admin panel takes us to this page where we can upgrade  or downgrade the privileges of a user.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a899bc23-32ad-4e54-8f5b-8d648f1f05b1)

Click on any action && capture the request and send it top repeater as it may come handy later.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a4fb6cb6-3695-48fd-8f43-f46bce773105)

We can see that in this *POST* request we have a parameter called **username=carlos** & **action=upgrade*

The above reqest upgrades the privileges of carlos user.


Now **Log out of admin user and try loggin in as wiener**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cfce931f-aad7-401c-a435-0fa6443f1c1c)

Reload the page & capture the request & send it to repeater.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/57fabd90-75e5-49af-a1b9-9b3f3bbf0f73)


**Our goal is to exploit the flawed access controls to promote yourself to become an administrator.**

So we can try to perform the role changing action as wiener by pasting the cookie value of wiener in the request made by admin to change the privileges !

Cookie of admin -   `6siUFNzqgvZGuGPXfxu7lFIiDXgNKdF5`
Cookie of wiener - `T0lAJAlHhdoNt8LZIK7VB45FxcSyawzE`

The response is **401 - Unauthorized**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cc3e41cd-b53f-4b51-89b9-a16747e5a157)

Since this lab is based on Method-Based-Access control bypass, we can try to change the request from **POST** to **GET**. We can do this by `Right click on request` -> Click `Change request method` option.

Now it changes to a *GET* request,

```http
GET /admin-roles?username=carlos&action=upgrade HTTP/2
Host: 0a6b00020427b34c8392bbf9005e002b.web-security-academy.net
Cookie: session=T0lAJAlHhdoNt8LZIK7VB45FxcSyawzE
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Origin: https://0a6b00020427b34c8392bbf9005e002b.web-security-academy.net
Referer: https://0a6b00020427b34c8392bbf9005e002b.web-security-academy.net/admin
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close
```






















