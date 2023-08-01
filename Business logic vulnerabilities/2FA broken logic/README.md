## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f9865451-fb34-4630-b8aa-41538004f015)

## Solution :

First we'll understand the workflow by logging in as wiener and taking a look at the requests made .


![image](https://user-images.githubusercontent.com/67383098/226844353-d15f82fb-1b9a-440b-b4aa-9b1c59a98691.png)

- 1st request (**POST**)
 
 ![image](https://user-images.githubusercontent.com/67383098/226983000-fd93f052-15ac-4711-a661-83f33784bff5.png)
 
- In the 2nd request (**GET**) , we see a `Cookie : verify=wiener` being added which indicates that we are already

![image](https://user-images.githubusercontent.com/67383098/226983275-854c01ff-9929-48c6-bd79-36bf8cedddb5.png)

- In the 3rd request , we again see that cookie being added,

![image](https://user-images.githubusercontent.com/67383098/226983807-be5c2bf3-4cdb-42bc-a534-04272c1565f7.png)

After forwarding all the 3 packets, the webpage asks us for a `4 digit OTP` to login, which we can get via the `Email Client`.

We get the OTP as `0138`

![image](https://user-images.githubusercontent.com/67383098/226984439-e6420e86-44d8-487e-9ee6-9f1b9f68502d.png)

Click `Login` 

![image](https://user-images.githubusercontent.com/67383098/226985212-9259cb5c-5541-4c6a-86c4-df0c068dfe22.png)


We now have a `mfa-code` parameter which indicates the OTP along with the session cookie .Forward the packet , we are logged in as wiener.

![image](https://user-images.githubusercontent.com/67383098/226985502-e2ac90b1-9555-4cb5-aa9f-96aee8812af5.png)


**Now the catch is what happens if we logout repeat the same process but by changing the `verify=wiener` to `verify=carlos` in every request (both GET & POST)**


![image](https://user-images.githubusercontent.com/67383098/226985998-8bd21b44-77f9-4acb-8445-8a7d8df6272b.png)

![image](https://user-images.githubusercontent.com/67383098/226986131-4acbaec0-cc3c-47e6-9b01-96eeed652ada.png)

![image](https://user-images.githubusercontent.com/67383098/226986250-e3fbf938-4d60-4548-9b13-6cc4e75e76b0.png)

After forwarding all the packets, we don't see any error message which means OTP for carlos has been created .

To login as carlos we have to 

- Enter incorrect  OTP and click `login`
- Capture the request and change `verify=weiner` to `verify=carlos`.
- Atlast bruteforce the OTP , find which OTP has `302 status code`.

![image](https://user-images.githubusercontent.com/67383098/226987054-39231246-fb3d-4789-9a6a-ba6d4cb0736f.png)


Since I don't have burp pro , I used `ffuf` TO FUZZ THE otp faster.


```
ffuf -X POST -u "https://0a0e008f04325f93c50b2233001b0024.web-security-academy.net/login2" -H "Cookie: verify=carlos; session=NB5SwamM383GwnD0MbTv7BXB5WeA3fIv" -H "Content-Type: application/x-www-form-urlencoded" -d "mfa-code=FUZZ" -w s; session=NB5SwamM383GwnD0MbTv7BXB5WeA3fIv" -H "Content-Type: application/x-www-form-urlencoded" -d "mfa-code=FUZZ" -w /home/noah/numbers.txt -fc 400,401,200
```
![image](https://user-images.githubusercontent.com/67383098/226980304-877fd06a-6af2-4107-bdd5-40ad4d25bd05.png)

Enter the OTP of carlos & we have successfully solved the lab.

![image](https://user-images.githubusercontent.com/67383098/226980655-4eb0e0d2-af3c-4769-884b-8d48ad22c47b.png)


