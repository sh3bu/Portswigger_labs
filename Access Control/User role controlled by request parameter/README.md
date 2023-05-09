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


In the lab description , it is given that the admin page is at `/admin`. Lets try visiting that page.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b17f5070-267e-4d27-b42c-70fb406cc9b7)

It says we can view only if we are admin!

We have a login credential `wiener:peter` .Lets try logging in and see what requests & responses being made.

Once we hit login after entering username and password, it sends a *POST* request like this,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/764de774-11f2-4190-8afd-dd5d53def796)

Then a *GET* request is made to retreive the `/myaccount` page .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cf2ec79f-a4f6-4338-b176-df63982855f6)

Here we have an interesting cookie - `Admin=false`.

The reponse looks like this ,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6dab0448-ed2a-4059-9c8f-75cee9250a17)

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3c891294-4036-4f00-b5f4-da0982fa05cc)

We are logged in as *wiener* but .

#### What happens if we change the cookie value to `Admin=true` ?

We modify all the requests where the cookie is set to `Admin=false` to Admin=true`

#### POST req to /login endpoint

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/65389e2c-e676-4c9e-8e97-66f896c92a08)

#### GET req to /myaccount endpoint

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/221f359b-9d25-4563-bcd1-dd6785912bf6)

#### GET req to /admin endpoint

After getting the myaccount page, we click the `Admin panel` link, the request sent is modified as follows

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c8976c16-6f51-4ef4-b853-720853017adb)

Atlast, we get the admin panel .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/af5fde08-3449-42eb-b3b3-534043100a67)

Now we can delete the suer carlos and solve the lab.

#### GET req to /admin/delete endpoint

Capture the request to delete the user carlos & change the cookie value here too!

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/220746d8-5a4f-46a5-8ea9-6531f5641de0)

We have sucessfully solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/acb7ae4a-a827-4f99-89ae-5895dd0f8da6)










