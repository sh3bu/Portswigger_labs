## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6b580e31-a010-42f7-b6cb-b54a0c852486)


## Solution :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/aa9efa73-2760-4c17-bc5b-daa4ed2b2660)


The lab page contains a `Live chat` link which allows us to send messages and also view transcripts,


![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4936661c-7b0f-4212-b9a4-61a72f8059ef)

For testing, first I tried to send a message.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/aa1046b4-1411-4934-9af7-f8eacda50b0e)

Now when I click `transcript`, it disconnects from the chat and downloads the chat transcript.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0ee07a40-764f-4ac3-a281-37cb44c61453)

*Opening the transcript*

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/19f7238c-db6d-489d-90be-3d131a74ecbe)

Notice that **The transcript downloaded was 2.txt and why not 1.txt?.  Maybe there was previous chats which contains  carlos's password**

Lets repeat the same process but capture the request when clicking on transcript button.

The browser sends 2 request,

**POST request to /download-transcript  **

```http
POST /download-transcript HTTP/2
Host: 0a93006303669dea816ad90100780044.web-security-academy.net
Cookie: session=3pDWJmTTJsAkBH4iRJC3adMvr27d6Cyq
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------20881831844699883562007729330
Content-Length: 421
Origin: https://0a93006303669dea816ad90100780044.web-security-academy.net
Referer: https://0a93006303669dea816ad90100780044.web-security-academy.net/chat
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers


-----------------------------20881831844699883562007729330

Content-Disposition: form-data; name="transcript"

CONNECTED: -- Now chatting with Hal Pline --<br/>You: Hi<br/>Hal Pline: What did your last slave machine die of?<br/>Hal Pline: You know I can hear you from further away, no need to be in my personal space<br/>DISCONNECTED: -- Chat has ended --

-----------------------------20881831844699883562007729330--
```


**GET request to /download-transcript which downloads the file to our system -**

```http
GET /download-transcript/3.txt HTTP/2
Host: 0a93006303669dea816ad90100780044.web-security-academy.net
Cookie: session=3pDWJmTTJsAkBH4iRJC3adMvr27d6Cyq
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a93006303669dea816ad90100780044.web-security-academy.net/chat
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
```

It retreives a static file `3.txt` , lets change it to `1.txt`.

As we guessed, the first chat **1.txt** contains the chat which has carlos's password.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6e6345ae-305e-4edc-b25c-3aacdcc8041a)

Pasword - `tlyc9lxwaf51hql4rjin`

Now login as carlos, to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3c2b57d6-bac6-4151-b515-f80fc3d132d8)


























