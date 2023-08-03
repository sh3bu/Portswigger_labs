## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ebfe3a1e-bad9-4cbf-9b6d-bbe5c92c3802)

## Solution :

### Finding  admin panel -

When we try to browse to `/admin`, it says onnly persons belonging to `DontWannaCry` company can access this page.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4f39aa52-0b24-48de-b792-0878bdd498ab)

### Account registration -

Go to registration page to create an account with the email id given to us - `attacker@exploit-0ab1009903bcf40b8008753f01b300db.exploit-server.net`

In the registration page, it is mentioned that we can use `@dontwannacry.com` if we are an employee of that company.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e2520013-e1d1-4bcf-8303-72731ab8c472)

Create an account with very long email address ending with **@exploit-0ab1009903bcf40b8008753f01b300db.exploit-server.net**. 

Example - `very-long-string@YOUR-EMAIL-ID.web-security-academy.net`

> NOTE - The very-long-string should be at least 200 characters long.

So I created an account with a very long string - `aafafasighfiyuqabryuawgfibasifnkajsdbgiuafgijaeboifnaijfaokjfiuahfjiahgr90ipqjro89sygfduygsfdtuvdtGUYEDFGWYTDVUYVDuyGDUYucvuavcuyvauyvdfuaycvuayvcuyasvcuyavcuyavcuucauvcuayvclu2o3i1hri97uuc89273r89hiug987niuch89qn397yr89hiu7ay89qah45uiyr0q@exploit-0ab1009903bcf40b8008753f01b300db.exploit-server.net`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bab76475-7f54-4776-b17c-1b87d622420f)

Once registered, we get an email .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c9d1cd8b-3687-4de4-8f2b-9de778d0ced9)

Click on the link  & our registration is sucessful.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/fa41e806-cfb4-40e9-a396-1260742c105b)

Now login as that user using the creds that we've created - `test1:1234`

Once logged in we get this message but notice that **the email address was truncated to 255 characters**- 

```
aafafasighfiyuqabryuawgfibasifnkajsdbgiuafgijaeboifnaijfaokjfiuahfjiahgr90ipqjro89sygfduygsfdtuvdtGUYEDFGWYTDVUYVDuyGDUYucvuavcuyvauyvdfuaycvuayvcuyasvcuyavcuyavcuucauvcuayvclu2o3i1hri97uuc89273r89hiug987niuch89qn397yr89hiu7ay89qah45uiyr0q@exploit-0ab1009
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/03942139-045a-4c05-996d-a5c430050bf3)

#### KEY INFORMATION TO NOTE - 

- Total characters in original email used to register an account - `299 characters`
- Total characters in email after signing in - `255 characters`

### Register an account with @dontwannacry.com -

Now taking note of this behaviour, we can create a new account with **more than 255 characters long but  when the application truncates it, the last cahracters should be @dontwannacry.com**

The email id to be used now is 

```
aaafasighfiyuqabryuawgfibasifnkajsdbgiuafgijaeboifnaijfaokjfiuahfjiahgr90ipqjro89sygfduygsfdtuvdtGUYEDFGWYTDVUYVDuyGDUYucvuavcuyvauyvdfuaycvuayvcuyasvcuyavcuyavcuucauvcuayvclu2o3i1hri97uuc89273r89hiug987niuch89qn397yr89hiu7ay89qah45uiyr0q@dontwannacry.com.exploit-0ab1009903bcf40b8008753f01b300db.exploit-server.net
```
Create an account with the above email-id

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/33315eed-576d-4eb1-a18c-a265d1275752)

In the email inbox, click on the link to confirm registration.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f6f8ab11-435a-461e-88f4-28373b1984b4)


Now finally when we login, we can see that after it truncated , our  rmail address ends wiht **@dontwannacry.com**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c3f1bcc3-0420-413a-810f-3171463fc3a6)


We can now access the admin panel at */admin*.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/40313e59-9f73-4a1c-b868-aeba7d54883e)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1f137b0e-2b4e-471f-8e2a-105329266d44)
