```
This lab contains an OS command injection `vulnerability in the product stock checker`.

The application executes a shell command containing user-supplied product and store IDs, and returns the raw output from the command in its response.

To solve the lab, execute the `whoami` command to determine the name of the current user.
```

### Solution -

Vulnerability is in check stock filter 

![image](https://user-images.githubusercontent.com/67383098/225234017-13d55e1d-074c-40e9-ab3e-afea6521d066.png)

On clicking the link, the request & response looks like

![image](https://user-images.githubusercontent.com/67383098/225266361-4cb3dc03-7fbf-48ea-8a39-d816ad99f0a7.png)


We need to check which parameter is vulnerable [`productID` or `storeID`]

> productID

Request -

![image](https://user-images.githubusercontent.com/67383098/225265218-d45cc7a6-d412-4874-b5e9-9095f6bf9980.png)


Response -

![image](https://user-images.githubusercontent.com/67383098/225235472-03edc2dd-d895-4e42-9720-46ab3a2445fc.png)

It's not showing the o/p of whoami, so this parameter is not vulnerable.



> storeID

Request -


![image](https://user-images.githubusercontent.com/67383098/225265788-cc413d8d-329e-4ff8-8f95-cd99dba9ea5c.png)

Response - 


![image](https://user-images.githubusercontent.com/67383098/225265936-9d361244-2009-45a8-9dd5-1a4e9af7a24b.png)
