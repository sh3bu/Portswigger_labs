## What is the DOM?

The Document Object Model (DOM) is a web browser's hierarchical representation of the elements on the page. 

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/a0e6feed-744b-4c63-89fc-5c55a0110041)

Websites can use JavaScript to manipulate the nodes and objects of the DOM, as well as their properties.JavaScript that handles data insecurely can enable various attacks. DOM-based vulnerabilities arise when a website contains JavaScript that takes an **attacker-controllable value, known as a `source`**, and **passes it into a dangerous function, known as a `sink`**. 

## Taint-flow vulnerabilities

Many DOM-based vulnerabilities can be traced back to problems with **the way client-side code manipulates attacker-controllable data**.

### Source

A **source** is a **JavaScript property that accepts data that is potentially attacker-controlled**. An example of a source is the `location.search` property because it reads input from the query string, which is relatively simple for an attacker to control. Ultimately, any property that can be controlled by the attacker is a potential source. This includes 

- Referring URL (exposed by the `document.referrer` string)
- User's cookies (exposed by the `document.cookie` string), and web messages.

### Sink 

A sink is a potentially **dangerous JavaScript function** or **DOM object **that can **cause undesirable effects if attacker-controlled data is passed to it**. For example, 

- `eval()` function is a sink because it **processes the argument that is passed** to it as JavaScript.

 An example of an HTML sink is
 
- `document.body.innerHTML` because it potentially allows an attacker to **inject malicious HTML and execute arbitrary JavaScript**.

> DOM-based vulnerabilities arise when a website passes data from a source to a sink, which then handles the data in an unsafe way in the context of the client's session.

## Which sinks can lead to DOM-based vulnerabilities?

The following list provides a quick overview of common DOM-based vulnerabilities and an example of a sink that can lead to each one.

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/4894c83e-8e67-4d33-be53-d3fef8f28c9a)
