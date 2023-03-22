## LAB DESCRIPTION :

![image](https://user-images.githubusercontent.com/67383098/226798530-f168966d-220f-47b8-96a5-ee5c9de046a1.png)

## Solution :

**Concept**
 
 > The concept in this is that 
 > bruteforce with a wrong username multiple times with any random password , it shows  `Invalid username or password`
 > bruteforce with correct username multiple times with any random password, it shows `You have made too many incorrect login attempts` indicating *it is a right password*  
 > Once correct username is found we can bruteforce the password , we can find it since it will have a `302` status code

The POST request to `/login` looks like this

![image](https://user-images.githubusercontent.com/67383098/226799235-5e0e7942-30e9-4b49-8ce8-833627c50ccf.png)

I noticed that when I bruteforce with invalid username and password, the account does not lock.
Only for valid usernames if we bruteforce, the account gets locked.

Send the request to intruder -

Attack Type - `Cluster Bomb`
Payload 1- $Username$ - `List of usernames`
Payload 2 - Password$$ - `Null payloads`

Click `Start attack` 

