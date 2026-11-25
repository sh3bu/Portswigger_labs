## Overview :

Prototype pollution gadgets may occur in third-party libraries that are imported by the application.

## Lab :

<img width="903" height="527" alt="image" src="https://github.com/user-attachments/assets/5a7ed184-7982-4b8f-999d-43ab2084cadd" />


## Solution :

### Identifying Source 

<img width="1917" height="647" alt="image" src="https://github.com/user-attachments/assets/8c7e771b-ac73-4198-bd14-58c07ad2d977" />

DOM Invader indicates that there is a **potential sink in the URL in the Hash property which is the URL fragment**

<img width="1422" height="388" alt="image" src="https://github.com/user-attachments/assets/25da77c0-5ae3-4b68-a955-9f44df8a957d" />

We can see from above that indeed the prototype is polluted.

> Note that we used bracket notation since DOM invader mentioned it. Thats why Dot notation was not used here.

### Identifying Gadget & Sink

DOM Invader has identified the Gadget as `hitcallback` and Sink as `setTimeout`.

<img width="1919" height="496" alt="image" src="https://github.com/user-attachments/assets/530e2b04-9d98-4722-b084-340d9d538147" />

This JS is a **third party library belonging to Google Analytics** which is used in this site.

<img width="1065" height="882" alt="image" src="https://github.com/user-attachments/assets/a1ead5b7-6a89-4d8d-ae2c-93f6a121af78" />

<img width="685" height="137" alt="image" src="https://github.com/user-attachments/assets/cfb1ab8e-fb0b-4d6b-8b91-e4c27b58908f" />

Clicking on Exploit button triggers an XSS.

<img width="1571" height="273" alt="image" src="https://github.com/user-attachments/assets/ac8afe9d-fad6-4b5b-b41e-fc5b1cc21376" />

But to solve the lab we need to deliver this URL to victim & get his cookies - `https://0a53006503b813f7800003c1004e005e.web-security-academy.net/#__proto__[hitCallback]=alert%281%29`

For that, we need to go to exploit server & enter the following payload

```js
<script>
location="https://0a53006503b813f7800003c1004e005e.web-security-academy.net/#__proto__[hitCallback]=alert%28document.cookie%29"
</script>
```

Upon clicking View exploit, we can test it for ourselves & it works!
<img width="1571" height="261" alt="image" src="https://github.com/user-attachments/assets/0f48f89a-82e2-4de6-8e33-152a1856b049" />

Click on **Deliver exploit to Victim** to solve the lab

<img width="1561" height="441" alt="image" src="https://github.com/user-attachments/assets/93f4e14f-2eb2-4346-abf6-2dc35985fedf" />

