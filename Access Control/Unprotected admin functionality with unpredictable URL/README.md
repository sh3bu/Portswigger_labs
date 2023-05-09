## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0bf7bd0d-de32-4c77-b80c-e9d9ff8f55a9)


## Solution :

The lab webpage looks like this,

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b7a63b87-190f-4161-93d7-ebdcca9876cd)

Clicking on `My Account`, takes us to the *Login page* at `/login`.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/96bc2b66-d748-4703-8a2a-101a316363fa)

On seeing the source code, we see a javascript code which checks if the user is **admin**, then it redirects us to `/admin-59c6c7` endpoint which is the admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/cb626268-41b7-4241-ac59-0102a24389a6)

So we could directly browse to that directory to view the admin panel.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a48c0b51-e444-4484-8583-dc638bb23b9d)

Click on ther  *DELETE* link of carlos to delete the carlos user .Thus lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5f95734c-302b-4739-83cf-19bf4a4a59eb)
