# Vulnerabilities in password-based login :

- Bruteforcing usernames
- Bruteforcing passwords
- Username enumeration ( Take note of `status code` `error message` & `response times`)

## LAB 1 - Username enumeration via different responses

## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8534cefc-0f1c-438e-b880-b1aeac3871e4)


## Solution :

The login looks like this,

![image](https://user-images.githubusercontent.com/67383098/225512065-f8791108-7c7f-4a2b-a8ed-a81e338bd145.png)

Captured request is as follows , it has two parameters - `username` and `password`

![image](https://user-images.githubusercontent.com/67383098/225512182-e55c9fd6-2e79-42d9-9204-2591fe61d837.png)

Now send the intercepted request to Intruder, clear all the selection & add `username` field.
In the payload section, copy paste all the usernames in the given list.Click `Start Attack`.

Now we can see there is a entry which has different response & length. So `ads` is the username.

![image](https://user-images.githubusercontent.com/67383098/225512567-b4ead3d6-ae9c-48e8-a496-703e4082a8c0.png)

Now change the username to `ads` and add the password parameter .

![image](https://user-images.githubusercontent.com/67383098/225512865-f10f65b3-aeda-40cb-92aa-73ed09df4313.png)

Add the given payload in the payload section, Click **Start Attack**.

We get an entry which has different status code which is `302` redirection which indicates  that login is successful and is redireting us to home page

![image](https://user-images.githubusercontent.com/67383098/225513040-c9ca155a-77a9-4538-8aed-84fa80327266.png)

**username** - `ads` **password** - `zxcvbn`
