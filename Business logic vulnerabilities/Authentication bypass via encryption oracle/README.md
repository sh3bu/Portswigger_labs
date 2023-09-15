## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/52590bf6-6f74-410d-8c80-3a35e9a05210)

## Overview ;

#### Providing an encryption oracle -

Dangerous scenarios can occur **when user-controllable input is encrypted and the resulting ciphertext is then made available to the user in some way**. This kind of input is sometimes known as an "**encryption oracle**". An attacker can use this input to encrypt arbitrary data using the correct algorithm and asymmetric key.

**This becomes dangerous when there are other user-controllable inputs in the application that expect data encrypted with the same algorithm**. In this case, an **attacker could potentially use the encryption oracle to generate valid, encrypted input and then pass it into other sensitive functions**.

This issue can be compounded if there is another user-controllable input on the site that provides the reverse function. This would enable the attacker to decrypt other data to identify the expected structure. This saves them some of the work involved in creating their malicious data but is not necessarily required to craft a successful exploit.

The severity of an encryption oracle depends on what functionality also uses the same algorithm as the oracle. 

## Solution :

The login page has an option to enable `Stay logged in` .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/710d1cac-22cb-49ce-a127-269125a34308)

From the response we can see that the stay-logged-in cookie is encrypted.

```http
HTTP/2 302 Found
Location: /my-account?id=wiener
Set-Cookie: stay-logged-in=jWSTZyORXvTn32zVPtkb9LIaQclhXV%2fbjXQaKZvK0ZY%3d; Expires=Wed, 01 Jan 3000 01:00:00 UTC
Set-Cookie: session=Im3gnYROce4zjKuv1Qb45L3uavF0eCkp; Secure; HttpOnly; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

When we upload a comment with an email id , the comment is sucessful. But when we upload a comment with incorrect email id, then the application tells that it is an **invalid email address**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9d8f4acf-3fd7-42a7-adb7-cec8b5844393)


The request is as follows:

```http
POST /post/comment HTTP/2
Host: 0a54003c03bc98a680aafd01007900a6.web-security-academy.net
Cookie: stay-logged-in=jWSTZyORXvTn32zVPtkb9LIaQclhXV%2fbjXQaKZvK0ZY%3d; session=Im3gnYROce4zjKuv1Qb45L3uavF0eCkp
Content-Length: 103
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="117", "Not;A=Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a54003c03bc98a680aafd01007900a6.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a54003c03bc98a680aafd01007900a6.web-security-academy.net/post?postId=8
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

csrf=XxUgTgS98oZNFuZqNeFWMCVf0uQVOLSo&postId=8&comment=testing&name=Shebu&email=shebu.test.com&website=
```

The response is as follows,

```http
HTTP/2 302 Found
Location: /post?postId=8
Set-Cookie: notification=IBfS1CCltAuDPqPfEQce3NAZIVNRpqeBe8iX7yn0XcGOX2Kbxq1qoXttIAg1qPkF; HttpOnly
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

There is a possibility that the `Set-Cookie: notification:<value>` is the value of the email that we provided in the request.

After this response we are then redirected back to the blog post with the error message.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ad47d49d-76ed-498b-99ae-ae701ad80c80)

Rename the `POST /post/comment` request to **Encrypt** since it encrypts the email and sets it in the `Set-cookie: notification: <value>`

Rename the `GET /post?postId=8` request to **Decrypt** since 
