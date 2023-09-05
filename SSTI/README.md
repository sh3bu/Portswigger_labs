## Methodology -

![ssti-methodology-diagram](https://github.com/sh3bu/Portswigger_labs/assets/67383098/65d81bbe-01bb-4143-b482-6c094e303a6f)

#### Detect :

Fuzz the template by injecting a sequence of special characters commonly used in template expressions, such as `${{<%[%'"}}%\` to see if ti triggers any exceptions. The response  will indicate which template engine is being used in the backend.

#### Identify the template engine :

Using a process of elimination based on which syntax appears to be valid or invalid, you can narrow down the options quicker than you might think.

![template-decision-tree](https://github.com/sh3bu/Portswigger_labs/assets/67383098/233012d4-90ed-44c7-87aa-84f1ef8e1432)

#### Exploit :

After detecting that a potential vulnerability exists and successfully identifying the template engine, you can begin trying to find ways of exploiting it. 
