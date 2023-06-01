## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6fba9ea2-8a18-4fc8-bd4c-ae3094bf98dc)

## Overview :

A common feature is the option to stay logged in even after closing a browser session. This is usually a simple checkbox labeled something like "Remember me" or "Keep me logged in".

This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cookie.

Some websites assume that if the cookie is encrypted in some way it will not be guessable even if it does use static values. While this may be true if done correctly, naively **encrypting the cookie** using a simple two-way encoding like Base64 offers no protection whatsoever. Even using proper encryption with a one-way hash function is not completely bulletproof.

> If the attacker is able to *easily identify the hashing algorithm*, and *no salt8 is used, they can potentially *brute-force the cookie* by simply 
> hashing their wordlists. This method can be used to bypass login attempt limits if a similar limit isn't applied to cookie guesses. 

## Solution :

