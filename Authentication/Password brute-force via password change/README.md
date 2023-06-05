## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e42366ed-c112-442f-b5cc-6e57398a6bc1)


## Overview :

**Changing user  passwords** -

 Typically, changing your password involves entering your current password and then the new password twice. These pages fundamentally rely on the same process for checking that usernames and current passwords match as a normal login page does. Therefore, these pages can be vulnerable to the same techniques.

Password change functionality can be particularly dangerous if it allows an attacker to access it directly without being logged in as the victim user. For example, if the username is provided in a hidden field, an attacker might be able to edit this value in the request to target arbitrary users. This can potentially be exploited to enumerate usernames and brute-force passwords. 

## Solution :

Go to  the login page. Enter the test credentials `wiener:peter` to login & test the functionality.

We are redirected to this page where we can change our  current password.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/67b3c142-6578-4376-af55-f1aeab9720c6)

1. When we enter correct current password & 2 different new passwords like  `peter:1234:123`, we get this response `New passwords do not match`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a960e46f-9ff5-405e-862b-d44983a8d485)

2. When we enter correct password & 2 same new passwords like `peter:1234:1234` ,  we get this response `Password changed successfully!`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/238ade73-cac6-4d96-8cab-e5bb4c8f5af2)

3. When we enter wrong current password & 2 different new passwords like `asdf:1234:123` , we get this response `current password is incorrect`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b733afe6-5681-49ee-be4f-b0f01379e801)
*
4. When we enter wrong current password  & 2 same new password `asdf:1234:1234` , our account gets locked.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e4226fd1-8dda-4224-bc92-1471dd2f3597)


By using the 2 & 4 use case, we can try to bruteforce carlos's password by giving 2 same new passwords and bruterforcing the current password field.

A *POST* request is sent when we hit change password button after entering the new password.

```http
POST /my-account/change-password HTTP/1.1
Host: 0a10000303be5187822a7e1a006f00d0.web-security-academy.net
Cookie: session=d4WuJiDqFIehBh9jjDBem7IEgLtgQqai
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 91
Origin: https://0a10000303be5187822a7e1a006f00d0.web-security-academy.net
Referer: https://0a10000303be5187822a7e1a006f00d0.web-security-academy.net/my-account
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

username=wiener&current-password=wiener&new-password-1=newpass123&new-password-2=newpass123
``
