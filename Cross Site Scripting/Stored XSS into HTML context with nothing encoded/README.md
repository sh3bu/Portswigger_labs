## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/63fd29c0-9435-42a5-a9f8-df61a54b58bd)

## Solution :

This time we have a blog website. We have a functionality to post comments.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/324d79a3-c72a-4bdc-8a31-5a6dccc0ba5b)

We can use the following XSS payload in the comment field to perform a stored Xss attack.

```js
<script>alert(1)</script>
```

Now each time when the page containing the comment is reloaded, it triggers an alert.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7f662334-2078-4bff-af81-3704cc0ccd18)

Thus the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d8fcf171-156a-4283-be84-9cd4765aaa0d)

