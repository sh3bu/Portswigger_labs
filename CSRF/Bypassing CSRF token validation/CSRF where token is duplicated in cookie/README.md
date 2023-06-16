## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5e3c5679-cfe8-4604-88d1-27aff3ccef53)


## Overview :

Sites also commonly use a **double-submit cookie as a defense against CSRF**. In this technique, the state-changing request contains the same random token as both a cookie and a request parameter, and  the server checks whether the two values are equal. If the values match, the request is seen as legitimate. Otherwise, the application rejects.

If the application uses double-submit cookies as its CSRF defense mechanism, it’s probably not keeping records of the valid token server-side. If the server were keeping records of the CSRF token server-side, it could simply validate the token when it was sent over, and the application would not need  to use double-submit cookies in the first place. 

The **server has no way of knowing if any token it receives is actually legitimate; it’s merely checking that the token in the cookie and the token in the request body is the same**. 

_Bypass this restriction by providing same non legitimate token in both request & body -_

```http
POST /password_change
Host: email.example.com
Cookie: session_cookie=YOUR_SESSION_COOKIE; csrf_token=not_a_real_token
(POST request body)
new_password=abc123&csrf_token=not_a_real_token
```


## Solution :

