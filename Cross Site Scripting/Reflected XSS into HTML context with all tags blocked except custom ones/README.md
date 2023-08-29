## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d01a2cb7-14e4-448a-8d23-3dfa0747a8fd)

## Solution :

The search term is reflected in the page response embedded within <h1> tags.

```js
<h1>5 search results for 'Hi'</h1>
```
When we enter `<script>`in the search field, it gets blocked.

### Finding the tags that are allowed -

Send the request to repeater and add the payload position as follows.

```
GET /?search=%3C§§%3E HTTP/1.
``
Start the attack.

Once the attack is finished, 
