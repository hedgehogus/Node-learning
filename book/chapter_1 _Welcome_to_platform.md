
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

`if (false) {
 let x = "hello"; 
}
console.log(x);` ----- This code will raise a ReferenceError: x is not defined 

`const x = 'This will never change';
x = '...';`  -------- This code will raise a TypeError: Assignment to constant variable

Const does not indicate that the assigned value will be constant, but that the binding with the value is constant

*It is becoming best practice to use const when requiring a module in a script, so that the variable holding the module cannot be accidentally reassigned:*

`const path = require('path');`

**If you want to create an immutable object, const is not enough, so you
should use ES5's method Object.freeze()** or the deep-freeze module (https://www.npmjs.com/package/deep-freeze).

### The arrow function (36)


