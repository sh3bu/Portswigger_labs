## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4896d9c2-c649-466b-8934-76ce6e76a95d)

## Overview :

> Domain-specific flaws

In many cases, you will encounter logic flaws that are specific to the business domain or the purpose of the site.

The discounting functionality of online shops is a classic attack surface when hunting for logic flaws. This can be a potential gold mine for an attacker, with all kinds of basic logic flaws occurring in the way discounts are applied.

For example, consider an online shop that offers a 10% discount on orders over $1000. This could be vulnerable to abuse if the business logic fails to check whether the order was changed after the discount is applied. 

## Solution :

When the website loads, we see a message from the developers that we can avail discount if we are a new customer.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/35e76d94-b6d0-4911-bbf9-1f093dd07016)

Log in to the website as wiener & add the leather jacket to cart. 

Apply the promo code & we can see that there is $55 discount while purchasing it.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/8fab21a9-08e6-47cd-8638-a71061e27138)

At the bottom of the shopping page we have this signup feature .

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1b2355c6-ce79-4cd1-8c5b-6db2622503f8)

Signing up for newsletter gives us another $50 dollar discount.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e1a93dd8-029b-44a0-99e3-12d0de9ed49a)

Clicking on place-order button ,the order is placed sucessfully.

### Flawed logic -

At this point after analysing the features of the site, we think from an attacker's perspective on what can possibly go wrong?

1. Maybe we can try to apply  same coupon code again and again till the price decreases to certain limit.
2. Else we could use both the coupons we got and apply them again & again alternatively.

#### Case 1 - Apply  same coupon code again and again :

When we try to apply the same coupon again and again, we get this message stating that *Coupon already applied.*

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/2cf90998-47b1-4994-a4e0-453f58805f37)

#### Case 2 - Use both coupons again & again alternatively :

When we try to apply both the coupons again&again in an alternate way, we identify the flaw. The price has now come down to $0 dollars.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/915685cd-cca5-4711-b7ec-481f21cb295f)

Place the order to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/de4833a7-837a-426b-ab00-5cff61f61eb5)
