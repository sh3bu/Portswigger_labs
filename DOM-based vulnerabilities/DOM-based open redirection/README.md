## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a414eb12-e357-45ec-80d4-094748cd7eb8)

## Overview :

DOM-based open-redirection vulnerabilities arise when **a script writes attacker-controllable data into a sink that can trigger cross-domain navigation**. For example, the following code is vulnerable due to the unsafe way it handles the location.hash property:

```javascript
let url = /https?:\/\/.+/.exec(location.hash);
if (url) {
  location = url[0];
}
```

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause a redirection to an arbitrary external domain. 

Reference - https://portswigger.net/web-security/dom-based/open-redirection

## Solution :

