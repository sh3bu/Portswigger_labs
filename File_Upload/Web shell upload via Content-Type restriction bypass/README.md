## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0c98703e-48d9-42ad-af35-eb6b74343216)

## Overview :

- One way that websites may attempt to validate file uploads is to check that this input-specific **Content-Type header matches an expected MIME type**. If the server is only expecting image files, for example, it may only allow types like `image/jpeg` and `image/png`.
- Problems can arise when the value of this header is implicitly trusted by the server.,if no further validation is performed to check whether the contents of the file actually match the supposed MIME type.

## Solution :

