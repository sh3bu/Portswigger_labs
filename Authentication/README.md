






# Vulnerabilities in password-based login

## LAB 1 - Username enumeration via different responses

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

----------------------------------------------------------------------
## LAB 2 - Username enumeration via subtly different responses

```
This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

Candidate usernames
Candidate passwords
To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

```
Captured request -

 ![image](https://user-images.githubusercontent.com/67383098/225623669-69cd9be5-fa15-4341-87a8-8eb5afd8e4eb.png)

All the responses look then same.
![image](https://user-images.githubusercontent.com/67383098/225628858-0b29d51a-f1ce-46b9-bedc-da9b8d7ae3e1.png)

From this `right click any one of the request` -> `Define extract grep from response` option.
![image](https://user-images.githubusercontent.com/67383098/225631913-7a32cd56-eb4d-4d02-a24e-42c7cbdf357a.png)

A new dialog box appears, 
![image](https://user-images.githubusercontent.com/67383098/225632233-bf54550e-1869-4a16-a694-faedf246b70f.png)

Scroll down & highlight `Invalid username or password` , the regex fields automatically gets updated.

Now start the attack,

We can see that there is a `typo in only one response`
![image](https://user-images.githubusercontent.com/67383098/225631474-41d736d2-a8ad-4389-8622-033f1fdf6a81.png)

Here one thing stands out, this response has `Invalid username or password ` rather than `Invalid username or password` ie.trailing fullstop & space is missing.
![image](https://user-images.githubusercontent.com/67383098/225633230-5939c304-982d-4d17-9db6-3914912566de.png)

Now update the username to `arlington` , add the password field as target , add payload of passwords and start the attack.

We get a `302` status code which indicates that it is the password

![image](https://user-images.githubusercontent.com/67383098/225638137-53e9b4a6-0b35-4ba2-bc40-cef310eed777.png)

-----------------------------------------------------------



