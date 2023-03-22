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

**Concept**
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
