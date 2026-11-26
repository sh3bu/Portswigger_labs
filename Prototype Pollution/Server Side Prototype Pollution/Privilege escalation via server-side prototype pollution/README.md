## Overview :

`POST` or `PUT` requests that submit JSON data to an application or API are prime candidates for this kind of behavior as it's common for servers to respond with a JSON representation of the new or updated object. In this case, you could attempt to pollute the global Object.prototype with an arbitrary property as follows:

```http
POST /user/update HTTP/1.1
Host: vulnerable-website.com
...
{
    "user":"wiener",
    "firstName":"Peter",
    "lastName":"Wiener",
    "__proto__":{
        "foo":"bar"
    }
}
```

If the website is vulnerable, your injected property would then appear in the updated object in the response:

```htp
HTTP/1.1 200 OK
...
{
    "username":"wiener",
    "firstName":"Peter",
    "lastName":"Wiener",
    "foo":"bar"
}
```
In rare cases, the website may even use these properties to dynamically generate HTML, resulting in the injected property being rendered in your browser.


## Lab :

<img width="923" height="743" alt="image" src="https://github.com/user-attachments/assets/9363a878-b2a2-45dd-ba2a-035836bf2a44" />


## Solution :

Upon login as wiener we get this page where we can update billing and delivery address.

<img width="1470" height="886" alt="image" src="https://github.com/user-attachments/assets/d6cca403-44f8-4675-8f3a-b9f616d659c9" />


The POST request to `my-account/change-address` looks interesting. It contains interesting JSON response parameter called `"isAdmin":false`.

<img width="1919" height="749" alt="image" src="https://github.com/user-attachments/assets/4f4bf72d-c7e5-432d-b2a3-3b13a2e60cee" />

### Identifying Source -

When we try to pollute the JSON in request body, the same property injected gets reflected in response.

<img width="1915" height="695" alt="image" src="https://github.com/user-attachments/assets/7feb3c83-509b-49c0-a731-1a85b0fa93ef" />

This indicates a potential source.

### Identifying gadget -

To gain administrative privileges for solving the lab, the JSON response parameter `isAdmin` seems like potential gadget to target.

Upon setting the same parameter in JSON request, we observe that the isAdmin property for wiener user has been inherited from `Object.prototype.isAdmin=true`

<img width="1884" height="697" alt="image" src="https://github.com/user-attachments/assets/b1a48efd-a11c-40f9-b9b8-3bfee4dab736" />

### Exploit :

Now when we refresh the page, we see the link to Admin panel.

<img width="1557" height="563" alt="image" src="https://github.com/user-attachments/assets/eb822133-2778-4f7c-ac93-1bee560fa5ec" />

Delete user Carlos to solve the lab.

<img width="1561" height="479" alt="image" src="https://github.com/user-attachments/assets/fb578e52-8c43-4509-bae7-b4a154a4f5d6" />


