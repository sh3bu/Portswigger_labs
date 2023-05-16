## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/79c1f760-c0c0-45e4-85b6-20f8dcf0264e)


## Solution :

The account page that contains the current user's existing password, prefilled in a masked input. So lets login as wiener to see what the account page has.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7a204dc2-b5c9-48eb-aba9-1b3471346572)

We can see there is a **pre filled password** in the password field. We can't see the password in plaintext but we can try reloading the page / view source-code to see what the password is.


![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1ad475a7-3da7-4f89-82e6-985755f6eba7)

So the password is prefilled with the password of the user winer which we logged in.

Now if we click on the `My account` link again after logging in , the apge reloads where it sends a request as below.

```http
GET /my-account?id=wiener HTTP/2
Host: 0a33005d047134a3807cc18f00330037.web-security-academy.net
Cookie: session=jqKEHRYHaiBdzMZ9BLoiv8e1xTz24mLC
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/113.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a33005d047134a3807cc18f00330037.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
```

Change the parameter `?id=wiener` to `?id=administrator`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4f6a5a0d-4787-4149-a800-1f70d415bde2)


Thus we have sucessfully exploited it as it leaked the admin's password in the response.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/315af2cf-1bde-47b9-b37d-b2c0bda0f2ec)

Password - `nrmwf5x1228f0spbcvxt`


Now we can login as admin & delete user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/46943942-8896-422a-98f3-e750ad54f826)




























