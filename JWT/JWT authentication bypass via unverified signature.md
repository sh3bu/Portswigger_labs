## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e12a4920-a21c-4d2b-965f-dffe3dea8c9f)

## Overview :

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the **Node.js** library **jsonwebtoken** has `verify()` and `decode()`.

Occasionally, developers confuse these two methods and only pass incoming tokens to the `decode()` method. This effectively means that the application doesn't verify the signature at all. 

## Solution :

Login to the website as wiener using the provided credentials.

In the HTTP history tab we can see 3 requests have been highlighted as it has JWT.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d37294ca-00d4-4e78-93ba-c892085b17a3)

Send the *GET* request to the uri `/my-account?id=wiener` to repeater. Since it is the request that identifies the user. 

```http
GET /my-account?id=wiener HTTP/2
Host: 0aef00060493cca0807926f7003e00e9.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aef00060493cca0807926f7003e00e9.web-security-academy.net/login
Connection: keep-alive
Cookie: session=eyJraWQiOiI3ZTUzNjY5MS00MGQ4LTQ3MGUtYTdkZC1kMWYxYTg5ZmYwZGUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTYyNzI4OH0.llpcQqdSn9hrRmaHEa-tkzGWfXBaiSTOZmaJSyx2_iRuWOXpLP3BdhL71DiqMvZJw9hYUhZKjAhRWhRS7glXTP6mFn6haB-9icqGC7uRcCrPfBMLlVX9bp9l63qnvgkLsRu6VuonPAtfa7zSC1IVW8kUnZ13nQIz90OCKybsznObTxNBCllTaE_FOJRFxkxaLd7L_zZmR_1EvFyLjHfE3D3T2uahWyEGizwmmgl8SA8A26gOQL9RmTZcQ3QCEgXbJnZkbKhKnj3ccNnQRgmYGnpmCBgc9gVk_RZfZEKjujUMukIXAjdOyEhYH7_qz716xbLZWWzzY7hhepeCCYa5EQ
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```

Select the header and payload section of the JWT in the request, we can see the decoded value in the inspector tab.

