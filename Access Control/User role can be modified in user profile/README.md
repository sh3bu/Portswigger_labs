## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bddb3540-e08c-4e1a-81d8-e2e5d2278405)


## Solution :

So as per the lab description, we can view the admin panel only if our `roleID=2`.

#### GET request to /myaccount - 

Once we click my account link, the browser sends a req as follows.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0cf88a20-ed0e-427a-82c8-d463635f5ecc)

#### GET request o /login - 

This request retreives the login page to enter the username and password.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c9cbb4dc-e266-4348-bef3-2ea6789209bf)

Login page -

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/35fc29d1-5d2f-4544-a7cc-98ce5fb2e27b)

Enter the credentials which are given for testing which is `wiener:peter`& click *LOGIN* button, the captured request is as follows

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6e659f9b-9efc-457d-b6fd-d987a603e8ed)

Now we have logged in as wiener,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/04910894-9e36-4e42-94fd-ec7690d3f960)


*Till now we didn't see any suspicious cookie values being sent from our client*

Lets move on & try to update our email.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7fe3f679-485c-48d6-be58-340ddd868aea)

**Request** :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6befb46b-e7ba-495e-8cfc-ae28cda3c316)

Send the request to repeater (I tried sending all the previous requests to repeater, it didn't have the *roleid* cookie either in request or response)

**Response** :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d26eb954-3b04-4bb6-a58b-4b57e8675acc)

Observe that we have a **roleid=1** in the JSON response.

So add the value `roleid=2` in the JSON request & see what is the response in browser

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/617b56ee-0624-4bc1-92c8-1bb8a6dcf868)

**We have a 302 redirect**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7afa9d88-9bf4-4276-baf6-17cba40a2458)

Click `Follow redirect`,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4fb027eb-3aae-451d-bcd7-60fa1763682f)

Now we can see the link to **Admin Panel** in the browser.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/107c0c37-e2a1-40e2-b18a-67c6dafaf3c9)


Now click on admin panel & delete the carlos user to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/448da80e-2713-41e1-bba1-3e7864f50bed)

















