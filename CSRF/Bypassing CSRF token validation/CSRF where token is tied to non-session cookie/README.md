## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9b0fa0fc-3538-47a6-9c76-28553367992f)



## Overview :

**CSRF token is tied to a non-session cookie -**

In a variation on the preceding vulnerability, some applications do tie the CSRF token to a cookie, but not to the same cookie that is used to track sessions. This can easily occur when an application employs two different frameworks, one for session handling and one for CSRF protection, which are not integrated together:
```http
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 68
Cookie: session=pSJYSScWKpmC60LpFOAHKixuFuM4uXWF; csrfKey=rZHCnSzEp8dbI6atzagGoSYyqJqTz5dv

csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY&email=wiener@normal-user.com
```

This situation is harder to exploit but is still vulnerable. If the web site contains any behavior that allows an attacker to set a cookie in a victim's browser, then an attack is possible. The attacker can log in to the application using their own account, obtain a valid token and associated cookie, leverage the cookie-setting behavior to place their cookie into the victim's browser, and feed their token to the victim in their CSRF attack. 

> The cookie-setting behavior does not even need to exist within the same web application as the CSRF vulnerability. Any other application within the same overall DNS domain can potentially be
leveraged to set cookies in the application that is being targeted, if the cookie that is controlled has suitable scope. For example, a cookie-setting function on staging.demo.normal-website.com
could be leveraged to place a cookie that is submitted to secure.normal-website.com. 

**Some times csrf token is tied to a cookie but this cookie is not related to the session of the user. This happens because the application uses 2 different frameworks one for session handling and one for csrf protection and both aren't integrated together.**


## Solution :

**First let's login as wiener -**

- On viewing the page source, we can fint he csrf token of wiener - `ZvJ76rK5LcD1LqPUkIRLAjmEptJMIFon`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/021d2c3a-ea2f-4120-964c-20bb29300dfb)

- Click on inspect element & we can see that in the cookies we have the csrfkey - `wa8IFRWwagWFenyNJRLtl342K9U5uikc`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1c63b2c5-e8ad-48d2-98ac-87a274abeaed)


**Log in as carlos -**

Open a incognito tab and login as carlos.

Update carlos's email & capture the request,

```http
POST /my-account/change-email HTTP/2
Host: 0ae700c80376e89582c41b7200b8004a.web-security-academy.net
Cookie: session=RXZrYLgsaGTzXBCU1EKoEmhCU1scqX6W; csrfKey=nKlGR3jzyFUMsIPM7RJ8ZxDE6yHSP2k7
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 59
Origin: https://0ae700c80376e89582c41b7200b8004a.web-security-academy.net
Dnt: 1
Referer: https://0ae700c80376e89582c41b7200b8004a.web-security-academy.net/my-account?id=carlos
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

email=test%40test.com&csrf=LcvnVlPGvy3TnwCkfhhKPe60IEu1JcAD
```

- Carlos's csrfkey - `nKlGR3jzyFUMsIPM7RJ8ZxDE6yHSP2k7`
- Carlos's csrf token - `LcvnVlPGvy3TnwCkfhhKPe60IEu1JcAD`

Let's try to replace the csrf token and csrf key of carlos with that of wiener's to see if we can bypass the csrf protection by providing another pair/set of valid tokens.


