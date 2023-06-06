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
