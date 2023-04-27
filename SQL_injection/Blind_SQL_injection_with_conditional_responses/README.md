## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234766625-5e8e25d3-387c-40d5-88b6-2c348a1ecb6b.png)

## Solution :


Request looks like 

![image](https://user-images.githubusercontent.com/67383098/234805007-50a73456-c90f-4288-a3bc-2b559e87e890.png)


Query made looks something like -

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>'
```
where trackingID - `g2w1yAddN9LlERl3` Cookievalue - `xdpiZNiyUyxtsAfDKlsfxYJldM3xW576`

TrackingID parameter is vulnerable. This is a blind SQL injection since response is not diplayed back. 

We can perform a SQL  injection on trackingID parameter , there maybe 2 results -

- 

- 
