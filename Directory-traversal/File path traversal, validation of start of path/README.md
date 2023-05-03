## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235922757-b8539ee3-e54f-4fcf-b0a5-dc796102e454.png)

## Solution :

If an application requires that the user-supplied filename must start with the expected base folder, such as /var/www/images, then it might be possible to **include the required base folder followed by suitable traversal sequences.** 

For example:

```bash
filename=/var/www/images/../../../etc/passwd
```

In this lab, the website loads serveral images just like the previous labs, the request captured looks like this

![image](https://user-images.githubusercontent.com/67383098/235923407-f54e2b3b-f8b4-403c-ad14-2c5898bd9704.png)


When we just give `../../../etc/passwd` payload in the `filename=` parameter , we get a `400 Bad Reques` in return. 
This is because **the server expects the filename must start with an expected base folder which is `/var/www/images`**.

![image](https://user-images.githubusercontent.com/67383098/235924360-57073ce3-e226-45d0-bd59-dab254f315a3.png)

So , we need to send a payload to retreive the /etc/passwd file where the payload starts with /var/www/images.

When we give `/var/www/images/../../../etc/passwd` as payload to filename parameter we can retreive the contents of the file.

![image](https://user-images.githubusercontent.com/67383098/235925042-309c9218-9192-4ff1-b441-538e4ec84ac9.png)

![image](https://user-images.githubusercontent.com/67383098/235925119-a6f531de-a986-46ae-b143-5c155db52896.png)
