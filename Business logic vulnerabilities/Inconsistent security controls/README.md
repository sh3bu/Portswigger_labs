## Lab Descriprtion :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1168b2af-7120-4f1b-8c7e-059f56d7c6e6)

## Overview :

 Applications may appear to be secure because they implement seemingly robust measures to enforce the business rules. Unfortunately, some applications make the mistake of assuming that, having passed these strict controls initially, the user and their data can be trusted indefinitely. This can result in relatively lax enforcement of the same controls from that point on.

If business rules and security measures are not applied consistently throughout the application, this can lead to potentially dangerous loopholes that may be exploited by an attacker. 

## Solution :

The admin panel is at `/admin`.

It is only available to users logged in with **@dontwannacry.com**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/60115bca-bab3-4dce-93ee-c247948d29be)


Create a test account using the provided email & verify the registration by clicking on the link sent to our inbox.


Once loggedin , we are presented with this page, where we can update our email-id.

Thinking from an attacker's perspective, we can understand that the the site checks whether we are an employee of that company or not by either checking the email-id or some form of cookie or special parameters like *?isdontwanacry=true*.

On analysing the requests which is been sent, we don't see any suspicious paramters . So it is  definitly checking the email-d.

Since we have an option to update our email, **why not change itattacker@exploit-0a4c008d03485779833ef97d01750004.exploit-server.net to `attacker@dontwannacry.com`?**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/24e67979-1dc1-4536-a349-e9bf4351031d)


Now we are able to view the **/admin** page.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1ca25b61-05ab-4bdf-9fba-b9c3f963bd07)

Delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a718b2f1-fedc-4f9a-80b6-c4b2b757ca48)
