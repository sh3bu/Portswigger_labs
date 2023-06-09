## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9e09bf01-222e-4118-94bd-0ff203b6c425)


## Overview :

 Some applications block input containing hostnames like 127.0.0.1 and localhost, or sensitive URLs like /admin. In this situation, you can often circumvent the filter using various techniques:

   - Using an alternative IP representation of `127.0.0.1`, such as `2130706433`, `017700000001`, or `127.1.`
   - Registering your own domain name that resolves to 127.0.0.1. You can use `spoofed.burpcollaborator.net`` for this purpose.
   - Obfuscating blocked strings using **URL encoding or case variation**.
   - **Providing a URL that you control, which subsequently redirects to the target URL**.
   -  *Try using different redirect codes*, as well as different protocols for the target URL. For example, **switching from an http: to https:** URL during the redirect has been shown to bypass some anti-SSRF filters.


## Solution :

In this lab the word `localhost` is blacklisted. So we need to encode the url such that it **bypasses the blacklist filter** which is setup at the backend.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/706d8d45-cea5-4473-9eb2-d00087713262)

We need to find whether `localhost` / `admin` is blacklisted.

First we try giving `localhost` , our request is blocked.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e349d57a-a954-4a44-a82f-dff7ad68a621)

Next we try another way by giving `LoCaLHosT`, this time we are able to bypass ther restriction.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9c76e868-38da-48dd-84cc-0c0a588f4c24)

Now lets add `/admin` ie (LoCaLHosT/admin), again it is blocked.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b4160f8d-25d3-46ab-ae83-5d97d06baf66)

If we apply the capitalization technique to /admin , we again bypass the blacklist.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4376bbf1-52da-4a21-9514-f7b9bbbda03b)

> We can use several techniques like `short ip - 127.1 or 127.0.1`, `Long ip - 127.000.000.1` or encode the IP in several forms.
> Website to encode IP into different forms - https://ipaddress.standingtech.com/online-ip-address-converter

Since we have access to admin panel, we can now delete the user carlos ,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cb00130f-8522-4133-80a0-7183e9c0773c)

Thus we've solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ed8a231d-e75a-49b4-ab64-f019e560bc2d)



