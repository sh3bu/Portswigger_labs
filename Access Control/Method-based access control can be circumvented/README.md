## Lab Decription :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d964f901-24f1-4c01-8336-90ce13bdd4ed)


## Solution :

In this lab we exploit the flaw of  **Method based access control**. Some websites may enforce access control to restrict access to specific URLs and HTTP methods based on the user's role like ,

```http
DENY: POST, /admin/deleteUser, managers
```

Here it blocks the *GET* requests to the  URL */admin/delete*.

Using method based access control, we can *bypass this by changing the HTTP request method*


### Steps -

We have two set of credentials given, first lets try logging in as admin and see the requests and responses being made.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/df9dfa56-4427-4419-9a14-457c7868367f)


![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1fc1357f-5c0e-4cbb-aedd-a9534644dfd8)


Nothing fishy.Now we have logged in as administrator.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bc2ae963-230b-4a67-a090-368599943182)

















![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b89d9c87-9794-47ff-8e4d-c2cccc6a13f7)

We try to login to the website with credentials given `wiener:peter`.

#### Request -

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7b2e6343-5793-432d-8ff2-df657e1dbcf5)

We don't see any suspicious headers.

Next we try to update the email

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/dda04f86-8bac-4b38-ada4-f5ea4c5b8936)


