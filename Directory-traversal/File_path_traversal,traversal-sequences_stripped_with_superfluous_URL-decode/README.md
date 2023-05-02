## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235583370-5a684593-c018-4625-bb50-f1aaa0f38e45.png)


## Solution :

The request which loads images loooks like ,

![image](https://user-images.githubusercontent.com/67383098/235583693-1bb5be26-ea94-439a-aac9-a36a6907f28f.png)

In this lab , the server strips all the directory traversal sequences **../** & also **..%2F..%2F..%2F**(doule encoding), so to bypass this we can use **triple encoding**.


So to bypass this we use triple encoded version of **../../../etc/passwd** as `..%252F..%252F..%252Fetc%252Fpasswd` as payload.

Now we got the response back containing the response of the file.

![image](https://user-images.githubusercontent.com/67383098/235585322-60ce1b39-c8d7-4e4b-93d1-6c540d1d654c.png)

![image](https://user-images.githubusercontent.com/67383098/235585698-3e35e55b-e837-4386-a640-ba77accb596c.png)
