## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/fe4268c0-9213-4fe3-9ac4-924358e0a73a)

## Solution :

When we select a category, the URL looks as follows.

```
https://0afa00c303a959438002801500a90058.web-security-academy.net/filter?category=Pets
```

The backend MongoDB query might look something like - 
```mongodb
this.category == 'fizzy' && this.released == 1
```

Injecting an single quote (`'`) , makes the application to throw an verbose error, which says that it uses mongodb in the backend.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4599afe2-d810-4fa3-905b-95b29087c343)


So we can use operator injection to evaluate the query to true & display all the products(both released & not released). - `' || '1' == '1`

The query will now be executed as follows
```mongodb
this.category == 'fizzy' || '1'=='1'  && this.released == 1
```

Now we can see all the items & the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a02ad56b-22cb-44d0-a9ff-1e523ec88ee3)
