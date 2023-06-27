## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/803a1935-d268-471a-9df8-3f7457f91eae)

## Overview :

### DOM-based cookie manipulation -

DOM-based cookie-manipulation vulnerabilities arise when a script writes attacker-controllable data into the value of a cookie. 


If JavaScript writes data from a source into document.cookie without sanitizing it first, an attacker can manipulate the value of a single cookie to inject arbitrary values: `document.cookie = 'cookieName='+location.hash.slice(1);`

If the website unsafely reflects values from cookies without HTML-encoding them, an attacker can use cookie-manipulation techniques to exploit this behavior. 

## Solution :

