## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235580616-52a78d72-f3c0-45f6-921c-8e7f78491659.png)


## Solution :

When the webpage loads, it loads all the images. The captured request looks like this,

![image](https://user-images.githubusercontent.com/67383098/235581314-2a1c9c3b-4244-47f6-b7ac-6322c48f38fc.png)


When we give `../etc/passwd` , the web application strips the `../` so the user input is interpreted as **etc/passwd**.

To bypass this we need to give *nested traversal sequences* like *....//etc/passwd* so that even if the server strips *../* so the resulting query which will be processed by the server will be `../etc/passwd`.

First we give this i/p - `....//....//etc/passwd`

We get this response containing **400 Bad Request**.

![image](https://user-images.githubusercontent.com/67383098/235582032-d517dfef-936d-4377-9f07-7995991f5ff9.png)

Next when we try this payload - `....//....//....//etc/passwd` , we get the contents of the file.

![image](https://user-images.githubusercontent.com/67383098/235582172-6c8ed3fe-bd4f-4845-9d70-d9ef242e6deb.png)

![image](https://user-images.githubusercontent.com/67383098/235582515-320c656b-a001-4974-8ff2-4767e2e7f29b.png)


