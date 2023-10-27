### Testing for NoSQL injection -

We can test the web application for NoSQL injections by entering fuzz strings as input.
```sql
'"`{
;$Foo}
$Foo \xYZ

'\"`{\r;$Foo}\n$Foo \\xYZ\u0000
``

