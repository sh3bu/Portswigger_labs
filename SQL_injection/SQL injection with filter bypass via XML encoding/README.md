## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4185a547-fb59-4522-8043-a5954cf42cc4)


## Solution :

you can perform SQL injection attacks using any controllable input that is processed as a SQL query by the application. For example, some websites take input in **JSON or XML** format and use this to query the database.

These different formats may even provide alternative ways for you to obfuscate attacks that are otherwise blocked due to WAFs and other defense mechanisms. Weak implementations often just look for common SQL injection keywords within the request, so you may be able to bypass these filters by simply encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses an XML escape sequence to encode the S character in SELECT: 

```xml
<stockCheck>
    <productId>
        123
    </productId>
    <storeId>
        999 &#x53;ELECT * FROM information_schema.tables
    </storeId>
</stockCheck>
```

This will be decoded server-side before being passed to the SQL interpreter. 

