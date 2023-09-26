## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/40c91ee1-7acf-4829-a698-28a92188f418)

## Overview :

- Servers may use several cryptographic keys for signing different kinds of data, not just JWTs. For this reason, the header of a JWT may contain a `kid` (Key ID) parameter, which helps the server identify which key to use when verifying the signature.
-  JWS specification **doesn't define a concrete structure for this ID** - it's just an arbitrary string of the developer's choosing. For example, they might use the **kid parameter to point to a particular entry in a database, or even the name of a file**
-   If this parameter is also vulnerable to directory traversal, an attacker could potentially force the server to use an arbitrary file from its filesystem as the verification key.

```json
{
    "kid": "../../path/to/file",
    "typ": "JWT",
    "alg": "HS256",
    "k": "asGsADas3421-dfh9DGN-AFDFDbasfd8-anfjkvc"
} 
```

-  This is especially dangerous if the server also supports JWTs signed using a **symmetric algorithm**. In this case, an attacker could potentially point the kid parameter to a predictable, static file, then sign the JWT using a secret that matches the contents of this file.

One of the simplest methods is to use `/dev/null`

> As this is an empty file, reading it returns an empty string. Therefore, signing the token with a empty string will result  in a valid signature. 

## Solution :

Login as wiener using the credentials provided.

The following requests have JWT in it.
![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9f6d7682-4897-4008-9ee9-9617c92b391b)

Send the GET request sent to */admin* to repeater tab.

The JWT token used in the request is ,

```
eyJraWQiOiIwMDc4ZjEzZi05YTM5LTQ0NTgtYjRhZi03OWRkYzhmZjdlYmQiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTcyOTU0Nn0.K6mkOhz-r8aU7CNjVz8S16wa7TXvfZMJk8ctUEN7Z4c
```

Decode it using [jwt.io](https://jwt.io)

**Header -**

```json
{
  "kid": "0078f13f-9a39-4458-b4af-79ddc8ff7ebd",
  "alg": "HS256"
}
```

**Payload -**

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1695729546
}
```

Go to Jason Web Token tab. Change the `kid` to `"kid": "../../../dev/null"` & `sub` to  `"sub": "administrator"`.

Since the content of kid ie `/dev/null` is null, we need to specify the secret key to be null before signing the JWT token.

But we can specify NULL in the secret value in this extension so we need to Base64-encode the NULL value - `\x00`

The Base64 encoded value of null is - `AA==`.

Now using the JWT Editor, Create a new symmetric key (Since HMACSHA256 is used).

Generate the key & replace the secret as `AA==`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b8255235-44cb-48b6-8853-93ac8154c240)

Click on Generate . The key is as follows,

```
{
    "kty": "oct",
    "kid": "e7758479-c636-410b-85a7-eaa566905f96",
    "k": "AA=="
}
```
**Now the file that the kid points is null & so we can now sign the token with a empty string & it is still valid !!**

We now have the following JWT.

```
eyJraWQiOiIuLi8uLi8uLi8uLi8uLi8uLi8uLi9kZXYvbnVsbCIsImFsZyI6IkhTMjU2In0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTU3Mjk1NDZ9.z7n4FC7_cH6RLq_Ra_2Z9884jrsN9ZYbMhW4KfVEi4M
```

Now replace the JWT with the one we just created & forward the request .

Now we have access to admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/067e75c2-7e8f-44f3-b5ea-e6cd673d399d)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/37135f34-cdb4-4eb6-8b88-4a3666eba069)
