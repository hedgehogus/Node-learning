## The callback pattern

**Callbacks** - are functions that are invoked to propagate the result of an operation and this is exactly what we need when dealing with asynchronous operations. They do replace the use of the *return* instruction that always executes synchronously.

Another ideal construct for implementing callbacks is **closures**. With closures, we can in fact reference the environment in which a function was created; we can always maintain the context in which the asynchronous operation was requested, no matter when or where its
callback is invoked.

#### The continuation-passing style (CPS)

In JavaScript, a **callback** is a function that is passed as an argument to another function and is invoked with the result when the operation completes. In functional programming, this way of propagating the result is called **continuation-passing style (CPS)**. 

**An unpredictable function** - one of the most dangerous situations is to have an API that behaves synchronously under certain conditions and asynchronously under others. 

*During first call function behaves **asynchronously**. Therefore, we have all the time in the world to register our listener, as it will be invoked later in another cycle of the event loop. The secon call will be **synchronous**. So, its callback will be invoked immediately, which means that all the listeners of reader2 will be invoked synchronously as well. However, we are registering the listeners after the creation of reader2, so they will never be invoked.*

Isaac Z. Schlueter, creator of npm and former Node.js project lead, in one of his blog posts compared the use of this type of unpredictable functions to unleashing Zalgo. Zalgo is an Internet legend about an ominous entity believed to cause insanity, death, and destruction of the world.

### Using synchronous APIs
**It is imperative for an API to clearly define its nature: either synchronous or asynchronous.**

For example, we can use the fs.readFileSync() function in place of its asynchronous counterpart.

> Pattern
> Prefer the direct style for purely synchronous functions.
> Bear in mind that changing an API from CPS to a direct style, or from asynchronous to synchronous or vice versa might also require a change to the style of all the code using it.

Also, using a synchronous API instead of an asynchronous one has some caveats:
+ A synchronous API for a specific functionality might not always be available.
+ A synchronous API will block the event loop and put the concurrent requests on hold. It does break the JavaScript concurrency model, slowing down the whole application

Using synchronous I/O in Node.js is strongly discouraged in many circumstances; however, in some situations, this might be the easiest and most efficient solution. Always evaluate your specific use case in order to choose the right alternative. Just to make a real use case example: **it makes perfect sense to use a synchronous blocking API to load a configuration file while bootstrapping an application.**

Use blocking API only when they don't affect the ability of the application to serve concurrent requests.

### Deferred execution 

Node.js, this is possible using **process.nextTick()**, which defers the execution of a function until the next pass of the event loop. Its functioning is very simple; it takes a callback as an argument and pushes it to the top of the event queue, in front of any pending
I/O event, and returns immediately. 

```java script
process.nextTick(() => callback(cache[filename]));
```
Another API for deferring the execution of code is **setImmediate()**. While their purposes are very similar, their semantics are quite different. Callbacks deferred with **process.nextTick()** run before any other I/O event is fired, while with **setImmediate()**, the execution is queued behind any I/O event that is already in the queue. 

### Node.js callback conventions
+ **Callbacks come last**. When a function accepts a callback in input, this has to be passed as the last argument
+ **Error comes first**. Another important convention to take into account is that the error must always be of type Error. This means that simple strings or numbers should never be passed as error objects.
```java script
fs.readFile('foo.txt', 'utf8', (err, data) => {
 if(err)
 handleError(err);
 else
 processData(data);
});
```
+ **Propagating errors**.  Proper error propagation is done by simply passing the error to the next callback in the chain. Also notice that when we are propagating an error we use the return statement. We do so to exit from the function as soon as the callback function is invoked and to avoid executing the next lines.
+ **Uncaught exceptions**. Wrapping the invocation of readJSONThrows() with a **try...catch block** will not work, because the stack in which the block operates is different from the one in which our callback is invoked. It's important to understand that an uncaught exception leaves the application in a state that is not guaranteed to be consistent, which can lead to unforeseeable problems. That's why it is always advised, especially in production, to exit from the application after an uncaught exception is received anyway.

### The module system and its patterns (68)
Modules are the bricks for structuring non-trivial applications, but also the main
mechanism to enforce information hiding by keeping private all the functions and variables
that are not explicitly marked to be exported. 

#### The revealing module pattern

One of the major problems with JavaScript is the absence of namespacing. Programs that
run in the global scope polluting it with data that comes from both internal application code
and dependencies. A popular technique to solve this problem is called the **revealing module
pattern** , and it looks like the following:
```java script
 const module = (() => {
 const privateFoo = () => {...};
 const privateBar = [];
 const exported = {
 publicFoo: () => {...},
 publicBar: () => {...}
 };
 return exported;
})();
console.log(module);
```
This pattern leverages a self-invoking function to create a private scope, exporting only the parts that are meant to be public

### Node.js modules explained (69)
