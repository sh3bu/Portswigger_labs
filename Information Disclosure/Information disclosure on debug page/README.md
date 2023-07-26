## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/817836d0-143c-4acd-8ab7-4cd55ab2c069)


## Overview :

 Debug messages can sometimes contain vital information for developing an attack, including:

    - Values for key session variables that can be manipulated via user input
    - Hostnames and credentials for back-end components
    - File and directory names on the server
    - Keys used to encrypt data transmitted via the client

Debugging information may sometimes be logged in a separate file. If an attacker is able to gain access to this file, it can serve as a useful reference for understanding the application's runtime state.

## Solution :


After the lab website loads, we can see the products listed there. Now to see if there is any **Debug page** info that is leaked. 

1. Go to Burpsuite , `Target` -> `Sitemap` . Right click on the top level entry for tha lab website shon.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/82fddad5-53c1-4645-8ebb-96bf7818c4d4)

2. Click on `Engagement tools` - `Find comments`. The following dialog box appears.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a04d4c4d-870a-4a64-a84c-4623be6f1fd2)

From that , we can see that there is a href pointing to **/cgi-bin/phpinfo.php**.

Send the request to repeater(add the path /cgi-bin/phpinfo.php),

```
GET /cgi-bin/phpinfo.php HTTP/2
Host: 0acf009004ebd9b881c6ac5600a700e0.web-security-academy.net
Cookie: session=3Y9b24AoTm8tC85I8NxLp0pXbmPiOYIi
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers
```

In the response we can see that lot of information is revealed including the value of `SECRET_KEY` - `pmgzklwq00nifvg9px4drvkmy40249p5`

```html
<tr>
  <td class="e">SECRET_KEY </td>
  <td class="v">pmgzklwq00nifvg9px4drvkmy40249p5 </td>
</tr>
```

Submit the key to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/72965427-cb3c-4c2a-a208-4019f42378c5)



