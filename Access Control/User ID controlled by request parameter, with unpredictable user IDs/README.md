## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cf733927-09c7-4307-9126-a9701dbf70e9)


## Solution :

In some applications, the exploitable parameter does not have a predictable value. For example, instead of an incrementing number, an application might use **globally unique identifiers (GUIDs)** to identify users. Here, an attacker might be unable to guess or predict the identifier for another user. However, the **GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews.**

Click on `My Account` , it takes us to login page. Use the credentials `wiener:peter` to login.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/098833f3-3b57-4892-8f0f-90477e20dd92)

Once we login, we can see the API key of wiener.

Click on `My Account` & capture the request.

We have the **GUID** of wiener in the request. We need to somehow find the GUID of carlos to get his API key.

For that , click on the home page and browse through all the blog posts by carlos, Click on his username

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5038876d-9dda-407d-8a33-7e6a5f52fa3d)

Now we got the GUID of carlos - `0cae1b22-401e-46b4-b767-09e89441403a`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/06d9c643-2ba6-47b2-9ffe-163756bf0c4a)

GUID of wiener - `jrrAYctrKLzzPUvGA7eKyh9NmlO6SPzm`
GUID of carlos - `0cae1b22-401e-46b4-b767-09e89441403a`

Now in the `My Account` page , click on the link , capture the request and modify the GUID of wiener with carlos to get the API key.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/58768fb5-202f-442b-aacc-523571af90d7)


In the response  now we get the API key of carlos - `9FLbgOm1kRjJoisnIuYN51HHtQdtOwYZ`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/27a8d6c4-c551-431d-b9c3-afb26ce6404c)


Thus we solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f7b12418-e6f8-42e6-afcc-513d34b6c932)

