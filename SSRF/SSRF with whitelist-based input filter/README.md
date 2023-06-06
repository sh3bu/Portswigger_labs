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

