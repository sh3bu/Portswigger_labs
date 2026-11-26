## Detecting server-side prototype pollution via polluted property reflection

An easy trap for developers to fall into is forgetting or overlooking the fact that a JavaScript for...in loop iterates over all of an object's enumerable properties, including ones that it has inherited via the prototype chain.

> Note: This doesn't include built-in properties set by JavaScript's native constructors as these are non-enumerable by default.

You can test this out for yourself as follows:
```js
const myObject = { a: 1, b: 2 };

// pollute the prototype with an arbitrary property
Object.prototype.foo = 'bar';

// confirm myObject doesn't have its own foo property
myObject.hasOwnProperty('foo'); // false

// list names of properties of myObject
for(const propertyKey in myObject){
    console.log(propertyKey);
}

// Output: a, b, foo
```
This also applies to arrays, where a for...in loop first iterates over each index, which is essentially just a numeric property key under the hood, before moving on to any inherited properties as well.
```js
const myArray = ['a','b'];
Object.prototype.foo = 'bar';

for(const arrayKey in myArray){
    console.log(arrayKey);
}

// Output: 0, 1, foo
```

In either case, if the application later includes the returned properties in a response, this can provide a simple way to probe for server-side prototype pollution. 
