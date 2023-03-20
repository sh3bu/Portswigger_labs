






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

## LAB 3 - Username enumeration via response timing

We try to fuzz the username and password like the previous lab  

![image](https://user-images.githubusercontent.com/67383098/225657172-9212db9f-770e-4770-b67e-660f54159238.png)

But this time there is `rate limiting/ip blocking` mechanism being enabled.

The hint provided mentions that doing some simple HTTP request header manipulation can bypass this brute force protection. A quick Google search leads to a (page with the correct answer)[https://medium.com/r3d-buck3t/bypass-ip-restrictions-with-burp-suite-fb4c72ec8e9c]: the `X-Forwarded-For` header.

So ,

- Add `X-Forwarded-For` header with a number
- Select attack type to `Pitchfork` since we need to bruteforce 2 parameters which is `X-Forwarded-For` and `username`
- Keep the password as a lenghty one.



![image](https://user-images.githubusercontent.com/67383098/226356933-b9985a32-0762-4de8-aad1-673eb0a5af3f.png)


![image](https://user-images.githubusercontent.com/67383098/226356359-3891ce36-22b3-44f5-823b-d8a966bd6e95.png)

Now after the attack has been finished, we add 2 columns `response received` and `response completed` from the `columns` option.
We can see that one POST resquest has a different response which is `alerts` which is the username. 

![image](https://user-images.githubusercontent.com/67383098/226356459-a1d32d3d-8851-4cf3-8f5f-b2ff3cfceb01.png)

Now remove all the added parameters and change it to `X-Forwarded-For` and `password`. Click `Start attack`

![image](https://user-images.githubusercontent.com/67383098/226357386-aa2570d4-3983-4257-8898-d150bac382f0.png)

We can see that one response has a `302 status code` - `**klaster**` which is the password.

![image](https://user-images.githubusercontent.com/67383098/226357574-4eb25461-ecf7-4dfa-a8b0-58747e4858de.png)

-------------------------------------------------------------------------------










