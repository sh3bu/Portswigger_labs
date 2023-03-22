# User rate limiting

Another way websites try to prevent brute-force attacks is through user rate limiting. In this case, making too many login requests within a short period of time causes your IP address to be blocked. Typically, the IP can only be unblocked in one of the following ways:

- Automatically after a certain period of time has elapsed
- Manually by an administrator
- Manually by the user after successfully completing a CAPTCHA

**As the limit is based on the rate of HTTP requests sent from the user's IP address, it is sometimes also possible to bypass this defense if you can work out how to guess multiple passwords with a single request.**

## LAB DESCRIPTION :

![image](https://user-images.githubusercontent.com/67383098/226794213-7d7ec96b-4af3-4b21-9d9d-2a634f2ea048.png)


## SOLUTION :

Capture the request -> send to repeater

Here after sending the POST request to login page 3 times, it shows `You have too many incorrect login attempts. Please try again after 1 minute` , which means rate limiting is enabled.

![image](https://user-images.githubusercontent.com/67383098/226795410-935e153a-9ee8-48a3-a2cb-1ae329682156.png)

Now we could see that the credentials are sent in `JSON format`

![image](https://user-images.githubusercontent.com/67383098/226797254-eaa1bab7-3072-442e-998c-3b9c85ef4f30.png)

We can supply the given password list as `array of passwords` in JSON,

**SCRIPT** 
```
for pass in $(cat password.txt); do echo \"$pass\",; done
```

![image](https://user-images.githubusercontent.com/67383098/226796130-eabfea71-63b4-4ccc-8bba-6777117c87e1.png)
![image](https://user-images.githubusercontent.com/67383098/226796171-c7f6d0b9-12e8-40f9-953e-37b1c94cd7a8.png)

In the aray of passwords , one of them is the valid password for carlos.

We get a `302 status code`, which means we have logged in as carlos.

![image](https://user-images.githubusercontent.com/67383098/226797589-5c432b96-6631-4f38-b99e-ff4173b98949.png)


