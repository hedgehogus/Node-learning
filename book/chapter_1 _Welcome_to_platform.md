
## The Node.js philosophy

creator - *Ryan Dahl*

+ **Small core** - the smallest set of functionalities, leaving the rest to the so-called userland (or userspace),
the ecosystem of modules living outside the core
+ **Small modules** - one of the most evangelized principles is to design small modules, not only in terms of code 
size, but most importantly in terms of scope. “Make each program do one thing well.”
+ **Small surface area** - modules usually also have the characteristic of exposing a minimal set of functionalities.
Very common pattern for defining modules is to expose only one piece of functionality, such as a function or a constructor
Created to be used rather than extended.
+ **Simplicity and pragmatism** -  less and simpler functionality is a good design choice for software (Keep It Simple, Stupid (KISS)).
Designing simple, as opposed to perfect, fully-featured software.  We would probably have more success in trying to get
something working sooner and with reasonable complexity, instead of trying to create nearperfect software with huge effort 
and tons of code to maintain

## Introduction to Node.js 6 and ES2015

Some of these features will work correctly only when strict mode is enabled. Strict mode can be easily enabled by adding a "use strict"
statement at the very beginning of your script.

### The let and const keywords

```javascript
if (false) {
 let x = "hello"; 
}
console.log(x);
``` 
----- This code will raise a ReferenceError: x is not defined 

```javascript
const x = 'This will never change';
x = '...';
```
-------- This code will raise a TypeError: Assignment to constant variable

Const does not indicate that the assigned value will be constant, but that the binding with the value is constant

*It is becoming best practice to use const when requiring a module in a script, so that the variable holding the module cannot be accidentally reassigned:*

`const path = require('path');`

**If you want to create an immutable object, const is not enough, so you
should use ES5's method Object.freeze()** or the deep-freeze module (https://www.npmjs.com/package/deep-freeze).

### The arrow function 

 **Arrow functions are bound to their lexical scope.** This means that inside an arrow function the value of this is
the same as in the parent block.
Before Node.js introduced support for arrow functions, to fix this we needed to change the
*greet* function using *bind*, as follows:

```javascript
DelayedGreeter.prototype.greet = function() {
 setTimeout( (function cb() {
 console.log('Hello' + this.name);
 }).bind(this), 500);
};
```

### Class syntax
 The real killer feature of the new syntax is the possibility of extending the Person prototype using the extend and
super keywords

```javascript
class PersonWithMiddlename extends Person {
 constructor (name, middlename, surname, age) {
 super(name, surname, age);
 this.middlename = middlename;
 }
}
```

### Enhanced object literals

+ assign variables and functions as members of the object
+ computed property names
+ new setter and getter syntax
```javascript
const x = 22;
const y = 17;
const obj = { x, y,
   square (x) {
    return x * x;
   },
   set fullname (fullname) {
     let parts = fullname.split('');
     this.name = parts[0];
     this.surname = parts[1];
   }
};
```
### Map and Set collections

The **Map** prototype offers several handy methods, such as *set, get, has, and delete, and the size* attribute (notice how the latter differs from arrays where we use the attribute length). We can also iterate through all the entries using the for...of syntax.
Every entry in the loop will be an array containing the key as first element and the value as second element.

**But what makes maps really interesting is the possibility of using functions and objects as keys of the map**

Along with Map, ES2015 also introduces the Set prototype. This prototype allows us to easily construct sets, which means lists with unique values:

`const s = new Set([0, 1, 2, 3]);`
`s.add(3); // will not be added`

 We have the methods add (instead of set), has, and delete and the property size
 
 ### WeakMap and WeakSet collections
 
 WeakMap is quite similar to Map in terms of interface; however, there are two main differences you should be aware of: **there is no way to iterate all over the entries, and it only allows having objects as keys**. While this might seem like a limitation, there is a good reason behind it. In fact, the distinctive feature of WeakMap is that it allows objects used as keys to be garbage collected when the only reference left is inside WeakMap. This is extremely useful when we are storing some metadata associated with an object that might get deleted during the regular lifetime of the application. 
 
 Similar to WeakMap, WeakSet is the weak version of Set: it exposes the same interface of Set but it only **allows storing objects and cannot be iterated.**
 
 ### Template literals 
```javascript
 const text = `${name} was an Italian polymath`
```
### Other ES2015 features

+ Default function parameters
+ Rest parameters
+ Spread operator
+ Destructuring
+ new.target
+ Proxy 
+ Reflect
+ Symbols
