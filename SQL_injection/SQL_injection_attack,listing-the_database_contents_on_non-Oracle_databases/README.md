## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234173772-d674eb97-c3c0-4f6d-99b2-3628123f4f5b.png)

## Solution :

Most database types (with the notable exception of Oracle) have a set of views called the information schema which provide information about the database.

You can query information_schema.tables to list the tables in the database:

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



























