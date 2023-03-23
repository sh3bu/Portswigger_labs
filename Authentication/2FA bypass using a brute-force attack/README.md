# Brute-forcing 2FA verification codes :

As with passwords, websites need to take steps to prevent brute-forcing of the 2FA verification code. This is especially important because the code is often a simple 4 or 6-digit number. Without adequate brute-force protection, cracking such a code is trivial.

Some websites attempt to prevent this by automatically logging a user out if they enter a certain number of incorrect verification codes. This is ineffective in practice because an advanced attacker can even automate this multi-step process by creating macros for Burp Intruder. The Turbo Intruder extension can also be used for this purpose.

## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/226912579-65ca8c90-ae63-4f00-9f23-0be333516c2c.png)


## Solution :

> Blog on **Burp Macros**  - https://akshita-infosec.medium.com/burp-macros-what-why-how-151df8901641

![image](https://user-images.githubusercontent.com/67383098/227289657-069c4e20-8278-4f88-ab26-a31c499e30f3.png)

![image](https://user-images.githubusercontent.com/67383098/227290265-87208e48-0d4f-4a09-87eb-c4ea1bb6c9d3.png)
![image](https://user-images.githubusercontent.com/67383098/227290418-fd6d59c6-70a1-47d5-8b48-02f63a96da40.png)

![image](https://user-images.githubusercontent.com/67383098/227290735-a2670c6b-a59e-40a1-8a28-b5b35e4c4437.png)

HTTP history 

![image](https://user-images.githubusercontent.com/67383098/227296503-f4c06128-a6cb-4929-9817-8444d08d596a.png)

`Project options` -> `Sessions` tab 

![image](https://user-images.githubusercontent.com/67383098/227297430-7c0dd916-99b1-4fb6-a996-76be7c214c82.png)

Under `Session Handling Rules` -> `Add`

![image](https://user-images.githubusercontent.com/67383098/227301370-1bec248b-f2c3-4587-8429-69af89b519dc.png)

Click `scope`

![image](https://user-images.githubusercontent.com/67383098/227303628-ab9f1120-1d70-43cf-be96-199f7352c9c3.png)

Go back to details tab

![image](https://user-images.githubusercontent.com/67383098/227304626-5f1ce1e0-839f-43e3-9763-16517abc4f18.png)


![image](https://user-images.githubusercontent.com/67383098/227304247-5e258403-2d55-47ed-b1d9-601d99f09a9c.png)


