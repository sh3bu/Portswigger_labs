## Lab Description :

<img width="1153" height="536" alt="image" src="https://github.com/user-attachments/assets/3d26edc0-b271-4bd4-99c6-9fa49b0822e7" />

## Solution :

Login to the site using the credentials provided - `wiener:peter`

The home page displays the promo code to avail 20% off that we can use at checkout.

![image](https://github.com/user-attachments/assets/23582f17-e1a2-4fab-8280-9bfe89ceca88)

### Identify possible collision  :

Add any cheap item to our cart . In this case I added **High-End Gift wrapping** to cart.

Now apply the coupon code , capture send the request to repeater.

Upon adding coupon code `PROMO20`, we see that we get the following response.

<img width="1463" height="778" alt="image" src="https://github.com/user-attachments/assets/9dec137c-cf1b-45ad-8787-91ce47e5064b" />

<img width="1257" height="894" alt="image" src="https://github.com/user-attachments/assets/8d599500-8562-40e2-b9f9-d986d49da792" />

But upon sending the same request again, we see that the server responds with **Coupon already applied**. 

<img width="1463" height="806" alt="image" src="https://github.com/user-attachments/assets/3f6fb328-d4d1-4f36-9036-15c318821ce4" />

### Exploiting race window :

Assuming that there might be a race window between the application checking if coupon code is applied already/not. We can exploit it by using the **single packet attack technique** to send more number of packets at the same time to the server & executing all of it at the same time to exploit the race window.

To exploit, 

1. Add the item to cart.
2. Send the request to repeater & **click on custom actions tab** at right side.
3. Click on **New->From Template** & select **Trigger Race condition** script.
4. Modify the number of requests to 1000.
<img width="1685" height="960" alt="image" src="https://github.com/user-attachments/assets/dc4a4943-4d75-43e2-8de0-dcc9d629d25e" />

5. Now run the script.

<img width="867" height="901" alt="image" src="https://github.com/user-attachments/assets/2e5ec64d-5d2b-4176-89b1-a7d8f7dc90e3" />

Observe that we can exploited the race window & now the price of the product is in negative. Similarly repeat the process using leather jacket & place the order to solve the lab

<img width="1262" height="879" alt="image" src="https://github.com/user-attachments/assets/bbec01fc-0c15-452f-bb36-c260e6a32ed6" />

<img width="1170" height="197" alt="image" src="https://github.com/user-attachments/assets/1175ab04-17c2-4e27-a1d5-9cee86f39fe5" />
