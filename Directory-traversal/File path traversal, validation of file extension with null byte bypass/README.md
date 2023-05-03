## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235926746-ef67574b-3448-4bf9-82aa-d97c2adde5f8.png)


## Solution :

If an application requires that the user-supplied filename must end with an expected file extension, such as .png, then it might be possible to use a **null byte** to effectively terminate the file path before the required extension. 

For example:

```bash
filename=../../../etc/passwd%00.png
```
> NOTE -`%00` is a null byte that is used to terminate a string in certain programming languages and can be used in URL input to trick the application into treating > the input as a different file type

The captured request which loads images of the website look like this,

![image](https://user-images.githubusercontent.com/67383098/235928287-ffdfcdc5-64d8-49fc-bbcf-de3088b9bf5b.png)

Here, like all the previous labs, it loads a *.jpg* image.

If we give  normal payload like `../../../etc/passwd` , it will give us a **400 Bad Request** in return . 

This is because there is a security implementation being imposed here. The server only accepts i/p's which end with a .jpg file extension.

So we craft a payload in such a way that the payload  we send must satisfy the condition of the server & also retreive the **/etc/passwd** also.

So the final payload will be `../../../etc/passwd%00.jpg`

![image](https://user-images.githubusercontent.com/67383098/235930240-400f3f8b-1ecc-4b12-ab7b-f86736232686.png)

![image](https://user-images.githubusercontent.com/67383098/235930723-e5dc1272-ff9a-4d35-ac4a-c1c8fb73c87b.png)

