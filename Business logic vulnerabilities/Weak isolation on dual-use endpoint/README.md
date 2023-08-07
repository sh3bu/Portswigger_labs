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
