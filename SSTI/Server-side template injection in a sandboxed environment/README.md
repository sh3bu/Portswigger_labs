## Lab Description :

![image](https://github.com/sh3bu/Portswigger_labs/assets/67383098/5b3f8ce0-c871-4314-b5de-3fbc075cda05)

## Overview :

Constructing a custom exploit using an object chain

As described above, the first step is to identify objects and methods to which you have access. Some of the objects may immediately jump out as interesting. By combining your own knowledge and the information provided in the documentation, you should be able to put together a shortlist of objects that you want to investigate more thoroughly. 

For example, in the Java-based template engine Velocity, you have access to a ClassTool object called **$class**. Studying the documentation reveals that you can chain the **$class.inspect()** method and **$class.type** property to obtain references to arbitrary objects. In the past, this has been exploited to execute shell commands on the target system as follows:

```java
$class.inspect("java.lang.Runtime").type.getRuntime().exec("bad-stuff-here")
```
## Solution :

As per the lab description we know the follwoing,

- 
- sandbox environment. 

[click here](javascript:alert(1))
