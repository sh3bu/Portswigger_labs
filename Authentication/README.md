






# Vulnerabilities in password-based login :

- Bruteforcing usernames
- Bruteforcing passwords
- Username enumeration ( Take note of `status code` `error message` & `response times`)

## LAB 1 - Username enumeration via different responses

```
This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

Candidate usernames
Candidate passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.
```

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

```
This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Your credentials: wiener:peter
Candidate usernames
Candidate passwords

HINT - To add to the challenge, the lab also implements a form of IP-based brute-force protection. However, this can be easily bypassed by manipulating HTTP request headers.
```

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


# Flawed Bruteforce protection :

The two most common ways of preventing brute-force attacks are:

- **Locking the account that the remote user is trying to access** if they make too many failed login attempts
- **Blocking the remote user's IP address** if they make too many login attempts in quick succession

**For example, you might sometimes find that your IP is blocked if you fail to log in too many times. In some implementations, the counter for the number of failed attempts resets if the IP owner logs in successfully. This means an attacker would simply have to log in to their own account every few attempts to prevent this limit from ever being reached.**

## LAB 4 - Broken brute-force protection, IP block

```
This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.

Your credentials: wiener:peter
Victim's username: carlos

- Candidate passwords

Advanced users may want to solve this lab by using a macro or the Turbo Intruder extension. However, it is possible to solve the lab without using these advanced features.
```
![image](https://user-images.githubusercontent.com/67383098/226528347-f0566be7-28e0-4b47-9a55-29e1728747c7.png)


![image](https://user-images.githubusercontent.com/67383098/226527797-8688214b-2681-41df-863c-ee7c8847c126.png)


> The concept behind this lab is that after 2 failed login attempts, our IP gets blocked for 1 minute. To evade this while bruteforcing carlos's password , we need to > bruteforce 2 incorrect passwords then login with the correct username & password of wiener, then again 2 incorrect attempts and so on.

> 1st req -` wiener` & `peter`
> 2nd req - `carlos` & <one of pass from the list given>
> 3rd req - `carlos` & <one of pass from the list given>
> 4th req - `wiener` & `peter`
> 5th req - `carlos` & <one of pass from the list given>
> 6th req - `carlos` & <one of pass from the list given>

Thus we can avoid IP blocking by passing the right credentials once in 2 incorrect attempts while bruteforcing .

So we create 2 files `usernames` & `passwords`

![image](https://user-images.githubusercontent.com/67383098/226531018-a608f917-a969-46da-8a62-ce94145857e2.png)

Select the type of attack to `Pitchfork`

Add username and password as the parameters

![image](https://user-images.githubusercontent.com/67383098/226531198-cd3a816a-5592-445e-b0ff-c4ac8bf98dd5.png)

Now after the attack has been finished, we see that one request with `carlos username` has a 302 status code, so `tigger` is the password

SOLUTION - username: `carlos` password: `tigger`

![image](https://user-images.githubusercontent.com/67383098/226532002-7a91fd21-c7be-44c4-88ec-e8a3c4bfc2a5.png)

----------------------------------------------------------------------
 
# Account locking:
 
 Locking an account offers a certain amount of protection against targeted brute-forcing of a specific account. However, this approach fails to adequately prevent brute-force attacks in which the attacker is just trying to gain access to any random account they can.


 
 > Account locking also fails to protect against `credential stuffing attacks`. 
 
 **Credential Stuffing attack** -
 
This involves using a massive dictionary of username:password pairs, composed of genuine login credentials stolen in data breaches. Credential stuffing relies on 
the fact that many people reuse the same username and password on multiple websites and, therefore, there is a chance that some of the compromised credentials in
the dictionary are also valid on the target website. Account locking does not protect against credential stuffing because each username is only being attempted once.
Credential stuffing is particularly dangerous because it can sometimes result in the attacker compromising many different accounts with just a single automated attack.

## LAB 5 - Username enumeration via account lock
 
 ```
 This lab is vulnerable to username enumeration. It uses account locking, but this contains a logic flaw. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

Candidate usernames
Candidate passwords
 ```
 
 
 




