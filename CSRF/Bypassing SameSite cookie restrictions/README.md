## Bypassing SameSite cookie restrictions

SameSite is a browser security mechanism that **determines when a website's cookies are included in requests when it is originating from other websites**. SameSite cookie restrictions provide partial protection against a variety of cross-site attacks, including CSRF, cross-site leaks, and some CORS exploits. 

## What is a site in the context of SameSite cookies?

When determining whether a request is same-site or not, the URL scheme is also taken into consideration. This means that **a link from `http://app.example.com` to `https://app.example.com` is treated as cross-site by most browsers. **

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/1b23abe2-68da-49c3-a5dd-737eca7432a9)

> You may come across the term "effective top-level domain" (eTLD). This is just a way of accounting for the reserved multipart suffixes that are treated as top-level domains in practice, such as
> `.co.uk`

## What's the difference between a site and an origin?

The difference between a site and an origin is their scope; **a site encompasses multiple domain names, whereas an origin only includes one**. Although they're closely related, it's important not to use the terms interchangeably as conflating the two can have serious security implications. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/12cd5d7f-0c1a-4f02-a91b-bf6109940cbf)

## How does SameSite work? 

SameSite works by enabling browsers and website owners to limit which cross-site requests, if any, should include specific cookies. This can help to reduce users' exposure to CSRF attacks,

 All major browsers currently support the following SameSite restriction levels:

- Strict
-  Lax
-  None

 Developers can manually configure a restriction level for each cookie they set, giving them more control over when these cookies are used. To do this, they just have to include the SameSite attribute in the Set-Cookie response header, along with their preferred value:
 
- `Set-Cookie: session=0F8tgdOhi9ynR1M9wa3ODa; SameSite=Strict`

> If the website issuing the cookie doesn't explicitly set a SameSite attribute, Chrome automatically applies Lax restrictions by default.
