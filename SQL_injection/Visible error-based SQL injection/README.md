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

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/02df6309-e561-4b2f-94d9-fcdbeae2465b)


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









