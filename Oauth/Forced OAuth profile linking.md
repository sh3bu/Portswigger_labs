## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b27eec88-6295-4475-b757-2150fd794978)

## Solution :

So as per the lab desc, we have these 2 information

- Blog website account: `wiener:peter`
- Social media profile: `peter.wiener:hotdog`
- The admin user will open anything we send from the exploit server and they always have an active session on the blog website.

### Recon 

The login page has an option to login via traditional username:password & also an option to login via social media !.

Once we login as wiener, we have this option to attach our social media handle.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/60e10e41-2c07-49c5-9c21-db3f9fa67bce)

On clikcing **Attach a social profile**, the client application sends a *Authoriation request* to the Oauth identity provider.

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
