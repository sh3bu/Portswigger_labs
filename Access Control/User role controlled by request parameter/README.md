## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5865cf0d-18fb-4c8f-8dea-f3bcf53c6537)


## Solution :

Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location, such as a **hidden field**, **cookie**, or preset query string parameter. The application makes subsequent access control decisions based on the submitted value. For example:

- https://insecure-website.com/login/home.jsp?admin=true
- https://insecure-website.com/login/home.jsp?role=1

This approach is fundamentally insecure because a user can simply modify the value and gain access to functionality to which they are not authorized, such as administrative functions.

### Steps -

We have our login page,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/90554f90-9edf-4fcf-9882-79457e4aa808)

Lets try logging in using the username `administrator` and a random password.

