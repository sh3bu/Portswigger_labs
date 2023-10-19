## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6899ea11-1c92-4caf-9819-8f61fb385738)

## Overview :

- For performance reasons, **many websites reuse connections for multiple request/response cycles with the same client**. Poorly implemented HTTP servers sometimes work on the dangerous assumption that certain properties, such as the H**ost header, are identical for all HTTP/1.1 requests sent over the same connection**. This may be true of requests sent by a browser, but isn't necessarily the case for a sequence of requests sent from Burp Repeater. This can lead to a number of potential issues.

- For example, you may occasionally encounter servers that only perform thorough validation on the first request they receive over a new connection. In this case, you can potentially bypass this validation by sending an innocent-looking initial request then following up with your malicious one down the same connection. 

- Many reverse proxies use the Host header to route requests to the correct back-end. *If they assume that all requests on the connection are intended for the same host as the initial request, this can provide a useful vector for a number of Host header attacks, including routing-based SSRF, password reset poisoning, and cache poisoning*.
  
## Solution :

