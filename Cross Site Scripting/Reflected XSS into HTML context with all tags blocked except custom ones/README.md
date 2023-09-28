## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d01a2cb7-14e4-448a-8d23-3dfa0747a8fd)

## Solution :

The search term is reflected in the page response embedded within `<h1>` tags.

```js
<h1>5 search results for 'Hi'</h1>
```
When we enter `<script>`in the search field, it gets blocked.

### Use custom tags -

The search url is as follows - 

```
https://0a96000b03ae5d4b80e03a7000a2002d.web-security-academy.net/?search=hello
```

We can use the following payload to pop an alert with document.cookie.

```html
<shebu onfocus=alert(document.cookie)  tabindex=1 id=1';
```

- `shebu` - custom tag.
- `onfocus=alert(document.cookie)` - whenever the particular tabindex is focussed, it will trigger a alert.
- `tabindex=1` - when tab in pressed 1 time , it will get selected.
- `id=1` - this will help us to specify which tabindex to use.

  ### Final payload -

  
