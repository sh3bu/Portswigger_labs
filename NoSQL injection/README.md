### Testing for NoSQL injection -

We can test the web application for NoSQL injections by entering fuzz strings as input.
```sql
'"`{
;$Foo}
$Foo \xYZ

'\"`{\r;$Foo}\n$Foo \\xYZ\u0000
```
If there is a change in the application response, then we can  determine which characters are interpreted as syntax by the application by injecting individual characters. For example, submitting `'`, may result in the following MongoDB query - `this.category == '''`
