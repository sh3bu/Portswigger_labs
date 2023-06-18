## Lab Description :


## Overview :

#### Bypassing SameSite Lax restrictions using GET requests

In practice, servers aren't always fussy about whether they receive a GET or POST request to a given endpoint, even those that are expecting a form submission. If they also use Lax restrictions for their session cookies, either explicitly or due to the browser default, you may still be able to perform a CSRF attack by eliciting a GET request from the victim's browser.

As long as the request involves a top-level navigation, the browser will still include the victim's session cookie. The following is one of the simplest approaches to launching such an attack:

```html
<script>
    document.location = 'https://vulnerable-website.com/account/transfer-payment?recipient=hacker&amount=1000000';
</script>
```

Even if an ordinary GET request isn't allowed, some frameworks provide ways of overriding the method specified in the request line. For example, Symfony supports the _method parameter in forms, which takes precedence over the normal method for routing purposes:

```html
<form action="https://vulnerable-website.com/account/transfer-payment" method="POST">
    <input type="hidden" name="_method" value="GET">
    <input type="hidden" name="recipient" value="hacker">
    <input type="hidden" name="amount" value="1000000">
</form>
```
Other frameworks support a variety of similar parameters. 

## Solution :
