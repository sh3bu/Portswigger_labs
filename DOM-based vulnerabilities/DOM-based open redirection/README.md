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
> If an attacker is able to control the start of the string that is passed to the redirection API, then it may be possible to escalate this vulnerability into a JavaScript
> injection attack. An attacker could construct a URL with the `javascript:` pseudo-protocol to execute arbitrary code when the URL is processed by the browser.

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause a redirection to an arbitrary external domain. 

## Which sinks can lead to DOM-based open-redirection vulnerabilities?

The following are some of the main sinks can lead to DOM-based open-redirection vulnerabilities:

```javascript
location
location.host
location.hostname
location.href
location.pathname
location.search
location.protocol
location.assign()
location.replace()
open()
element.srcdoc
XMLHttpRequest.open()
XMLHttpRequest.send()
jQuery.ajax()
$.ajax()
```

> Reference - https://portswigger.net/web-security/dom-based/open-redirection

## Solution :

If we click on any post, we can see at the bottom of the page there is a link to home page of the blog - `<- Back to blog`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1f69c655-cc3e-486a-831a-2ffa2488fa8b)


Clicking on it redirects us to the home page.

In HTTP  history , we can see this request being made.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d08afc28-f8b0-494c-ba6d-f99e3457e40e)

