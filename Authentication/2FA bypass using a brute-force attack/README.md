# Brute-forcing 2FA verification codes :

As with passwords, websites need to take steps to prevent brute-forcing of the 2FA verification code. This is especially important because the code is often a simple 4 or 6-digit number. Without adequate brute-force protection, cracking such a code is trivial.

Some websites attempt to prevent this by automatically logging a user out if they enter a certain number of incorrect verification codes. This is ineffective in practice because an advanced attacker can even automate this multi-step process by creating macros for Burp Intruder. The Turbo Intruder extension can also be used for this purpose.

## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/226912579-65ca8c90-ae63-4f00-9f23-0be333516c2c.png)


## Solution :

