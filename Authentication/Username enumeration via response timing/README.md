## LAB 3 - Username enumeration via response timing

## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/83f4c5c6-e909-4787-9926-6daac8822d37)


## Solution :

We try to fuzz the username and password like the previous lab  

![image](https://user-images.githubusercontent.com/67383098/225657172-9212db9f-770e-4770-b67e-660f54159238.png)

But this time there is `rate limiting/ip blocking` mechanism being enabled.

The hint provided mentions that doing some simple HTTP request header manipulation can bypass this brute force protection. A quick Google search leads to a (page with the correct answer)[https://medium.com/r3d-buck3t/bypass-ip-restrictions-with-burp-suite-fb4c72ec8e9c]: the `X-Forwarded-For` header.

So ,

- Add `X-Forwarded-For` header with a number
- Select attack type to `Pitchfork` since we need to bruteforce 2 parameters which is `X-Forwarded-For` and `username`
- Keep the password as a lenghty one.



![image](https://user-images.githubusercontent.com/67383098/226356933-b9985a32-0762-4de8-aad1-673eb0a5af3f.png)


![image](https://user-images.githubusercontent.com/67383098/226356359-3891ce36-22b3-44f5-823b-d8a966bd6e95.png)

Now after the attack has been finished, we add 2 columns `response received` and `response completed` from the `columns` option.
We can see that one POST resquest has a different response which is `alerts` which is the username. 

![image](https://user-images.githubusercontent.com/67383098/226356459-a1d32d3d-8851-4cf3-8f5f-b2ff3cfceb01.png)

Now remove all the added parameters and change it to `X-Forwarded-For` and `password`. Click `Start attack`

![image](https://user-images.githubusercontent.com/67383098/226357386-aa2570d4-3983-4257-8898-d150bac382f0.png)

We can see that one response has a `302 status code` - `klaster` which is the password.

![image](https://user-images.githubusercontent.com/67383098/226357574-4eb25461-ecf7-4dfa-a8b0-58747e4858de.png)
