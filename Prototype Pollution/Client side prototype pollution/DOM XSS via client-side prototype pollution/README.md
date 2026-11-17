## Lab Description :

<img width="831" height="319" alt="image" src="https://github.com/user-attachments/assets/7d39ed5f-0938-44fc-ade1-799698a4534c" />

## Solution :

Login to the site with the provided credentials

Enable prototype pollution option in DOM Invader: 

<img width="490" height="499" alt="image" src="https://github.com/user-attachments/assets/ed492a0b-72eb-4f09-91bd-2cb4388e8706" />

### Identifying source -

<img width="1916" height="1021" alt="image" src="https://github.com/user-attachments/assets/cd676a55-5160-4045-88bd-b848b9139c55" />

So the source is in the URL parameter.

### Identifying gadgets -

Click on `Scan for gadgets` button in DOM Invader console.

### Identifying sink - 

Once the scan is complete, we can see the `script.src` sink that is identified by DOM Invader.

<img width="1919" height="711" alt="image" src="https://github.com/user-attachments/assets/2e09b518-6fe9-4812-88aa-aecd152231a0" />

From the stack trace, we can identify the exact location of the vulnerable code in `searchLogger.js`. It shows the sequence of function calls that led to the script.src sink, helping us understand how attacker-controlled input reaches it.

<img width="1072" height="206" alt="image" src="https://github.com/user-attachments/assets/649f583e-4719-40c7-a2a0-fdd2479be368" />

<img width="969" height="617" alt="image" src="https://github.com/user-attachments/assets/ccc2121f-c449-4a28-8451-9c35828f2d51" />

### Exploit -

<img width="1637" height="622" alt="image" src="https://github.com/user-attachments/assets/b3ec1386-dcad-4228-b147-f014aeaff0d2" />

Upon clicking epxloit, we can see XSS pop up & thus the lab is solved

<img width="1278" height="508" alt="image" src="https://github.com/user-attachments/assets/8f97ca86-60e6-4c48-83df-924b06bf8050" />

