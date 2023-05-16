## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/237015401-3b749128-3ed6-4746-ad47-e2f2ebf1e28e.png)


## Solution :

When the lab loads, we get the ususal shopping website.

Clicking on `My Account` page takes us to `/login` endpoint

![image](https://user-images.githubusercontent.com/67383098/237015644-13e4dfcc-b08a-4691-84c3-491de6d60b4a.png)

Now we need to bypass this login page and directly access the admin page.

For that we can look for one of the common web directories like the **robots.txt** and **sitemap.xml**
 
#### robots.txt
 
 
![image](https://user-images.githubusercontent.com/67383098/237017303-78d344f9-93b9-4bed-8a35-44869a1f8172.png)

We see a **disallowed login** entry here for the admin panel which is `/administrator-panel`.

On visiting the `/administrator-panel` directory, we get the admin panel.

![image](https://user-images.githubusercontent.com/67383098/237017610-cea4c988-9383-4056-95f3-d58ef216004e.png)

So we click the link to delete the user *carlos* to solve the lab. 

![image](https://user-images.githubusercontent.com/67383098/237018105-478dabcc-1ff7-4a38-8aec-0558f66ed084.png)
