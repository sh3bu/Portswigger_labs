Primer on JS prototype & inheritance - https://portswigger.net/web-security/prototype-pollution/javascript-prototypes-and-inheritance


## Successful Exploitation of Prototype Pollution

Successful exploitation of prototype pollution requires the following key components:

1. **A Prototype Pollution Source**
   - This is any input that enables you to poison prototype objects with arbitrary properties.
   - Common sources include:
     - **URL:** Via either the query string or fragment string (hash).
     - **JSON-based input**
     - **Web messages**

2. **A Sink**
   - A JavaScript function or DOM element that can be used to trigger dangerous behavior, potentially leading to arbitrary code execution.

3. **An Exploitable Gadget**
   - A property that is passed into a sink without proper filtering or sanitization.
