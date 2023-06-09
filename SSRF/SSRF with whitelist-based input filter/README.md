## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/dcbc7842-14d6-42d5-a581-a8e99f78d030)


## Overview :

Some applications only allow *input that matches, begins with, or contains, a whitelist of permitted values*. In this situation, you can sometimes circumvent the filter by *exploiting inconsistencies in URL parsing*.

-  You can **embed credentials in a URL before the hostname**, using the `@` character. For example:

```http
   https://expected-host:fakepassword@evil-host
```

- You can use the `#` character to indicate a **URL fragment**. For example:
   
```http
https://evil-host#expected-host
```

-  You can **leverage the DNS naming hierarchy** to **place required input into a fully-qualified DNS name that you control**. For example:
    
```http
https://expected-host.evil-host
```
    
- You can **URL-encode characters to confuse the URL-parsing code**. This is particularly useful if the code that implements the filter handles URL-encoded characters differently than the code that performs the back-end HTTP request. Note that you can also try double-encoding characters; some servers recursively URL-decode the input they receive, which can lead to further discrepancies.
    

## Solution :

In this lab, there is  **whitelisting** done at the backend. So only the whilelisted inputs will be accepted as i/p. So our normal way to access the admin panel - `http://localhost/admin` fails.

```http
HTTP/2 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 58

"External stock check host must be stock.weliketoshop.net"
```

This means that the application accepts the request only if it contains `stock.weliketoshop.net`.

Lets try giving `username@stock.weliketoshop.net`. Here we can also give **localhost** as the username portion in the url.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/33026e91-a221-4e99-b0bf-b903ed50d075)


The request is accepted & we get a 400 response back.This time it doesn't show the error `External stock check host must be stock.weliketoshop.net` ,which means the username part is accepted by the application.

Changing it to localhost also gives a 200 OK response.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/97252e52-427c-4c6c-9352-2fd1038e6d3d)

Next is to use the `# - URL fragment` to make the server not to interpret  the part after the # as  a part of the request.

Now when we give the i/p url as `http://localhost#@stock.weliketoshop.net`, we get this error - `External stock check host must be stock.weliketoshop.net`

> This time it doesn't show `missing parameter` error. This means it accepts localhost as the domain and everything after the `#` as a URL fragment.So the localhost word gets whitelisted here.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0b0824ee-3e41-41f7-8e35-283cab5f5652)


Next on , we try to **double encode the #** ie (`http://localhost%2523@stock.weliketoshop.net/`) to bypass this.

Now we're able to accesss the localhost website.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/87a2da31-d3a5-4068-bd44-3535cc980c79)

**/admin panel -**

Access the url `http://localhost%2523@stock.weliketoshop.net/admin`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e7aeb56d-2691-453b-b58a-686dac1b13e1)

We also got the uri to delete the user carlos.

Now make a request to `http://localhost%2523@stock.weliketoshop.net/admin/delete?username=carlos`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/477bee59-ab04-4af7-bdea-588e618e4b56)

Thus we deleted the carlos usre & solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c0ae4252-5eb7-436a-a308-58a6995cc7be)




