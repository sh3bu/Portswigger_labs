## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/bc0be0ef-8729-4e9f-b6a4-49fdc5aa63a9)

## Solution :

Log in as wiener.

When we scroll down all the products in the home page, we can see that there is a signup feature. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/662897c8-2ee1-4607-8657-685192068c44)

When we signup, then site gives us a coupon code.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4bfdfcfe-b328-48a4-83a7-bedff38486ee)

In the home page, we have a product called gift card. Add it to cart

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/39bc42fb-ec1c-4684-b7ef-1419eb728be3)

Now in the cart section add the coupon *SIGNUP30* to avail discount.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/919efd5e-c9a3-47e7-acce-60252d411e48)

Once we place the order, we get a code to redeem.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/22aeb161-ca11-4fde-9bbf-34da2524d8c7)

Paste the coupon in the redeem section available at **My-Account** page to redeem some credits .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/975a5137-95e7-4106-99c9-0957e0bd6cc5)

Now we got extra **$3 dollars** to our store credit.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e0f6950c-258a-4b3e-9c5a-e908d7fe905a)

### Summary of the steps -

1. Login as wiener .
2. Signup for newsletter to get a discount coupon - **SIGNUP30**.
3. Add **Gift card** product to cart .
4. Apply **SIGNUP30** coupon .
5. Place the order to get a Gift coupon.
6. Redeem the coupon in *My-Account* page to get $3 dollars.

**On repeating this process again and again, we can gain more and more credits which will help us to buy the leet leather jacket.** In order to automate this multi step process, we use *macros* feature in burp.

### Automate the multi step process using macros -

Go to `Project options > "Sessions`. In the `Session handling rules` panel, click `Add`. The `Session handling rule editor` dialog opens.
In the dialog, go to the `Scope` tab. Under `URL Scope`, select `Include all URLs`.
Go back to the `Details` tab. Under `Rule actions`, click `Add` > `Run a macro`. Under `Select macro`, click `Add` again to open the Macro Recorder.

Select the following sequence of requests: 

```
POST /cart
POST /cart/coupon
POST /cart/checkout
GET /cart/order-confirmation?order-confirmed=true
POST /gift-card
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d68f037e-b4e7-499f-a28b-ba715606f27f)

In the list of requests, select `GET /cart/order-confirmation?order-confirmed=true`. Click `Configure item`. In the dialog that opens, click `Add` to create a custom parameter. Name the parameter `gift-card` and highlight the gift card code at the bottom of the response.

- Select the `POST /gift-card` request and click `Configure item` again. In the `Parameter handling` section, use the drop-down menus to specify that the gift-card parameter should be derived from the prior response (response 4). Click `OK`.
- In the Macro Editor, click `Test macro`. Look at the response to `GET /cart/order-confirmation?order-confirmation=true` and note the gift card code that was generated. Look at the `POST /gift-card request`. Make sure that the gift-card parameter matches and confirm that it received a 302 response. Keep clicking "OK" until you get back to the main Burp window.
- Send the `GET /my-account` request to Burp Intruder. Use the `Sniper` attack type.
- On the "Payloads" tab, select the payload type `Null payloads`. Under "Payload settings", choose to generate **412 payloads**. 
