# Lab 3 - Blind OS command injection with output redirection  [PRACTITIONER]

```
This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:

/var/www/images/

The application serves the images for the product catalog from this location. You can redirect the output from the injected command to a file in this folder, and then use the image loading URL to retrieve the contents of the file.

To solve the lab, execute the whoami command and retrieve the output.
```


### Solution -

Submitting the feedback form , the request looks like this

![image](https://user-images.githubusercontent.com/67383098/225280879-8dac8703-7a8d-4a01-9fe6-f934eb241a58.png)

As we know, the `email` parameter is vulnerable , we give the payload ` & whoami > /var/www/images/op.txt #` (URL-encode before sending) 


![image](https://user-images.githubusercontent.com/67383098/225297477-42b0bafc-d034-4bb0-93e2-0fa579f1a0d9.png)


We get a successful response 

![image](https://user-images.githubusercontent.com/67383098/225297581-afe92884-5c81-46b3-b009-4ba75d57737a.png)

Now, after storing the result of  `whoami` command in `/var/www/images` directory, we can check the content .

To find where the filename parameter is , we can check the `HTTP history tab` , there we can see the http GET request being made to retreive all image files in home page

![image](https://user-images.githubusercontent.com/67383098/225297722-7e28a055-7933-45df-858d-11fa05a47ba2.png)

Transfer the request to Repeater tab. We modify the `filename = op.txt` & in the response we get the username !

![image](https://user-images.githubusercontent.com/67383098/225297876-b1d95b58-f28f-4935-beb5-0f09148738fa.png)
