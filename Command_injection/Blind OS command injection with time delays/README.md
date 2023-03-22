# Lab 2 - Blind OS command injection with time delays  [PRACTITIONER]

```
This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response.

To solve the lab, exploit the blind OS command injection vulnerability to cause a 10 second delay.
```

### Solution -

The feedback form is as follows
![image](https://user-images.githubusercontent.com/67383098/225272191-aeed0d6c-ebdd-4165-b5fe-9c032128f014.png)

Captured request -> ![image](https://user-images.githubusercontent.com/67383098/225272577-7e45b33f-7dfd-4f79-ac41-3e1a1577477e.png)

By adding the  ` & sleep # `  (Note the space before & and after #) / ` & sleep 10 & `(URL-encode before sending) , we can see that the `email` parameter is vulnerable to command injection vulnerability, since there was time delay of 10 seconds.
> By '#' we comment out the rest of the query. Since it is a bash script that is running in the background 
REQUEST

![image](https://user-images.githubusercontent.com/67383098/225275759-bc1e050b-ecad-45ce-bc96-37cb1bce9f38.png)

RESPONSE 

![image](https://user-images.githubusercontent.com/67383098/225275993-e1937b09-6697-45e9-8672-f2960367e8df.png)
