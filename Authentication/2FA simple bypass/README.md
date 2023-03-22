# Bypassing two-factor authentication

At times, the implementation of two-factor authentication is flawed to the point where it can be bypassed entirely.

If the user is first prompted to enter a password, and then prompted to enter a verification code on a separate page, **the user is effectively in a "logged in" state before they have entered the verification code**. In this case, it is worth **testing to see if you can directly skip to "logged-in only" pages after completing the first authentication step**. Occasionally, you will find that a website doesn't actually check whether or not you completed the second step before loading the page.


## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/226806534-8b20a600-8087-4fe6-b10c-3026849d9613.png)

## Solution :

First we login as `wiener:peter` and see how the functionality works

![image](https://user-images.githubusercontent.com/67383098/226813201-1f4d53c1-caa5-4e99-97df-b9b57f030c6d.png)

On successfull confirmation of username and password, it asks for `4 digit OTP`

![image](https://user-images.githubusercontent.com/67383098/226813423-10fa0a37-c044-49b8-bbf6-99f3e669d9e2.png)

Click `Email client` to open email and get the **4 digit OTP**

![image](https://user-images.githubusercontent.com/67383098/226813486-1890f753-f36c-4601-9560-c12180fe74ad.png)

Paste the OTP in the field box & click `Log in`

We have sucessfully logged in as `wiener` . 

> Note the url of the page - `/my-account`

![image](https://user-images.githubusercontent.com/67383098/226813609-865d9d45-2af9-455f-85d7-406c0beecb7b.png)

Now lets bypass the 2FA of `carlos` and log in.

After entering the username and password , the request sent is as below

![image](https://user-images.githubusercontent.com/67383098/226814517-77aef57f-260e-421b-9a31-e46576b7973b.png)

Next step, it asks for `4 digit OTP` .
![image](https://user-images.githubusercontent.com/67383098/226814613-b3450250-56c0-4c3b-b86f-c2ac54214477.png)

 Capture the request & modify the uri path from `/login` to `/my-account` & click forward button in burp.
 

![image](https://user-images.githubusercontent.com/67383098/226814706-46f9c671-806e-4643-aaa9-d0eb747e9302.png)


![image](https://user-images.githubusercontent.com/67383098/226815413-a4818a5e-5284-476f-8815-ee4f29cb61cc.png)


![image](https://user-images.githubusercontent.com/67383098/226814329-d8c91b35-2fb7-4716-b2f5-b3626b9c7fe4.png)

