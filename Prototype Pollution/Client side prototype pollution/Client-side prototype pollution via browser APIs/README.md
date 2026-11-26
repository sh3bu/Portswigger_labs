## Overview :

### Prototype pollution via fetch() - 

The Fetch API provides a simple way for developers to trigger HTTP requests using JavaScript. The fetch() method accepts two arguments:

    The URL to which you want to send the request.

    An options object that lets you to control parts of the request, such as the method, headers, body parameters, and so on.

The following is an example of how you might send a POST request using fetch():
fetch('https://normal-website.com/my-account/change-email', {
    method: 'POST',
    body: 'user=carlos&email=carlos%40ginandjuice.shop'
})

As you can see, we've explicitly defined method and body properties, but there are a number of other possible properties that we've left undefined. In this case, if an attacker can find a suitable source, they could potentially pollute Object.prototype with their own headers property. This may then be inherited by the options object passed into fetch() and subsequently used to generate the request.

This can lead to a number of issues. For example, the following code is potentially vulnerable to DOM XSS via prototype pollution:
```js
fetch('/my-products.json',{method:"GET"})
    .then((response) => response.json())
    .then((data) => {
        let username = data['x-username'];
        let message = document.querySelector('.message');
        if(username) {
            message.innerHTML = `My products. Logged in as <b>${username}</b>`;
        }
        let productList = document.querySelector('ul.products');
        for(let product of data) {
            let product = document.createElement('li');
            product.append(product.name);
            productList.append(product);
        }
    })
    .catch(console.error);
```

To exploit this, an attacker could pollute Object.prototype with a headers property containing a malicious x-username header as follows:
```js
?__proto__[headers][x-username]=<img/src/onerror=alert(1)>
```

Let's assume that server-side, this header is used to set the value of the x-username property in the returned JSON file. In the vulnerable client-side code above, this is then assigned to the username variable, which is later passed into the innerHTML sink, resulting in DOM XSS.

> Note - You can use this technique to control any undefined properties of the options object passed to `fetch()`. This may enable you to add a malicious body to the request, for example.

### Prototype pollution via Object.defineProperty() - 

Developers with some knowledge of prototype pollution may attempt to block potential gadgets by using the Object.defineProperty() method. This enables you to set a non-configurable, non-writable property directly on the affected object as follows:
Object.defineProperty(vulnerableObject, 'gadgetProperty', {
    configurable: false,
    writable: false
})

This may initially seem like a reasonable mitigation attempt as this prevents the vulnerable object from inheriting a malicious version of the gadget property via the prototype chain. However, this approach is inherently flawed.

Just like the fetch() method we looked at earlier, Object.defineProperty() accepts an options object, known as a "descriptor". You can see this in the example above. Among other things, developers can use this descriptor object to set an initial value for the property that's being defined. However, if the only reason that they're defining this property is to protect against prototype pollution, they might not bother setting a value at all.

In this case, an attacker may be able to bypass this defense by polluting Object.prototype with a malicious value property. If this is inherited by the descriptor object passed to Object.defineProperty(), the attacker-controlled value may be assigned to the gadget property after all. 

**Example code**


1. The Developer's Flawed Defense

To stop user from inheriting isAdmin: true if Object.prototype ever gets polluted, the developer locks down isAdmin on the user object using Object.defineProperty(), leaving out value so it defaults to undefined:
```javascript
function createSafeUser(name) {
  const user = { name };

  // Developer tries to lock down 'isAdmin' so it can't be inherited from Object.prototype
  Object.defineProperty(user, 'isAdmin', {
    configurable: false,
    writable: false
    // Notice: 'value' is NOT explicitly set here!
  });

  return user;
}
```
2. The Attacker Pollutes Object.prototype.value

Before createSafeUser() runs, an attacker finds a prototype pollution vulnerability elsewhere in the app (like an unsafe merge() or clone() function) and pollutes Object.prototype with a property literally named value:
javascript
// Attacker's payload via a separate prototype pollution bug:
// e.g., JSON payload: {"__proto__": {"value": true}}
Object.prototype.value = true;
Use code with caution.

3. The Defense Backfires

Now, when createSafeUser("Alice") executes:
1. JavaScript evaluates the descriptor object: { configurable: false, writable: false }.
2. Object.defineProperty() checks descriptor.value.
3. Since value isn't defined directly on the descriptor, JavaScript looks up the prototype chain and finds Object.prototype.value (true).
4. Object.defineProperty() permanently locks alice.isAdmin to true—and because writable: false and configurable: false were set, the application cannot even fix or overwrite it.

## Lab :

<img width="823" height="431" alt="image" src="https://github.com/user-attachments/assets/a80d3fa6-e1e1-40fd-955e-0aad9ed93d68" />

## Solution :

### Identifying Source -

Notice that DOM Invader has identified a potential source in the Search URL.

<img width="1915" height="912" alt="image" src="https://github.com/user-attachments/assets/ae5d6641-16e3-4564-b12b-b569f0a1a3d4" />

We can pollute the Object.prototype & can verofy te same via console tab.

<img width="1377" height="519" alt="image" src="https://github.com/user-attachments/assets/b73a65f7-dfbd-4511-9ef4-ba642209f877" />

### Identifying Gadget -

By scanning for gadgets, we get to know that `script.src` is the sink in `SearchLoggerConfigurable.js` & `value` is the gadget.

<img width="1917" height="700" alt="image" src="https://github.com/user-attachments/assets/60798380-9b42-44a0-927f-5780cebcf3ce" />

We see that `Object.defineProperty` is used on the config object to lock down the `transport_url` property, setting both **configurable and writable to false**

```js
Object.defineProperty(config, 'transport_url', {configurable: false, writable: false});
```

So we need to just set a property called `value` & provide a xss payload that will get triggered when passed to a sink.

<img width="1665" height="337" alt="image" src="https://github.com/user-attachments/assets/25acaa69-ce16-4e4d-bdf3-52cc619c31a2" />

