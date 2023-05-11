## LAB 2 - Username enumeration via subtly different responses

## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ac3d689d-7d75-4e9e-a850-adfe4163d583)

## Solution :

Captured request -

 ![image](https://user-images.githubusercontent.com/67383098/225623669-69cd9be5-fa15-4341-87a8-8eb5afd8e4eb.png)

All the responses look then same.
![image](https://user-images.githubusercontent.com/67383098/225628858-0b29d51a-f1ce-46b9-bedc-da9b8d7ae3e1.png)

From this `right click any one of the request` -> `Define extract grep from response` option.
![image](https://user-images.githubusercontent.com/67383098/225631913-7a32cd56-eb4d-4d02-a24e-42c7cbdf357a.png)

A new dialog box appears, 
![image](https://user-images.githubusercontent.com/67383098/225632233-bf54550e-1869-4a16-a694-faedf246b70f.png)

Scroll down & highlight `Invalid username or password` , the regex fields automatically gets updated.

Now start the attack,

We can see that there is a `typo in only one response`
![image](https://user-images.githubusercontent.com/67383098/225631474-41d736d2-a8ad-4389-8622-033f1fdf6a81.png)

Here one thing stands out, this response has `Invalid username or password ` rather than `Invalid username or password` ie.trailing fullstop & space is missing.
![image](https://user-images.githubusercontent.com/67383098/225633230-5939c304-982d-4d17-9db6-3914912566de.png)

Now update the username to `arlington` , add the password field as target , add payload of passwords and start the attack.

We get a `302` status code which indicates that it is the password

![image](https://user-images.githubusercontent.com/67383098/225638137-53e9b4a6-0b35-4ba2-bc40-cef310eed777.png)
