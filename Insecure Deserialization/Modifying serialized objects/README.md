## Lab Description :
![image](https://github.com/user-attachments/assets/5014ea2e-dea8-47c7-b222-59b71a32014c)

## Solution :

Login with the credentials provided - `wiener:peter`.

Upon login we see that the user's cookie is a PHP serialize object.  

![image](https://github.com/user-attachments/assets/ab994178-e0c0-4d4a-a08c-ce7c7c708b4c)

The cookie contains a attribute called `admin:0` which means false.

Change the boolean attribute to `1` & convert it back to proper encopding as it was before.
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```

Upon replacing the old cookie in dev tools with the cookie that we have created , we can now see the hyperlink to access the admin panel.
![image](https://github.com/user-attachments/assets/1ae134ef-cf40-4c50-82e3-4982e7cf513a)

We now have privilege to delete user accounts.

![image](https://github.com/user-attachments/assets/84468199-6e31-4afe-9ea2-7038f3b0f2ed)

Delete the user carlos to solve the lab.

![image](https://github.com/user-attachments/assets/e6bcb63f-edbb-439d-90b5-072068337231)
