# Account locking:
 
 Locking an account offers a certain amount of protection against targeted brute-forcing of a specific account. However, this approach fails to adequately prevent brute-force attacks in which the attacker is just trying to gain access to any random account they can.


 
 > Account locking also fails to protect against `credential stuffing attacks`. 
 
 **Credential Stuffing attack** -
 
This involves using a massive dictionary of username:password pairs, composed of genuine login credentials stolen in data breaches. Credential stuffing relies on 
the fact that many people reuse the same username and password on multiple websites and, therefore, there is a chance that some of the compromised credentials in
the dictionary are also valid on the target website. Account locking does not protect against credential stuffing because each username is only being attempted once.
Credential stuffing is particularly dangerous because it can sometimes result in the attacker compromising many different accounts with just a single automated attack.


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

![image](https://user-images.githubusercontent.com/67383098/227200015-0763773e-ee53-466c-ae44-207a16ed737e.png)



Click `Start attack` 

![image](https://user-images.githubusercontent.com/67383098/227199876-eabd1b99-9fcd-4408-acb2-0622ec4d4ffd.png)

Now we got the username as - `arlington`

Now we need to find the **password**

- Update the username parameter to `arlington`
- Add the password parameter for bruteforcing with given list of passwords.

We have the results but we can't figure out which was the password , so we have to use the `Grep - Extract` method. Follow the steps,

Attack Type - `sniper`

Payload 1 - password parameter  [given list of passwords]

Got to `Options` -> `Grep-Extract` -> check the `Extract the following items from responses` -> Click `Add` button to open a dialog  which provides us a response . Highlight the line `Invalid username or password`
 

![image](https://user-images.githubusercontent.com/67383098/227201759-9222f5a1-514f-45be-9262-75839c73ead8.png)




We have one password from the list which didn't have the warning of `Invalid username or password`, it means we have succesfullt logged in.


![image](https://user-images.githubusercontent.com/67383098/227204607-1dc6e37e-0309-447a-8bff-2c152f235e8f.png)



![image](https://user-images.githubusercontent.com/67383098/227203837-a45ec2db-baf6-4c4c-80a3-3db25bc7f5dc.png)



