## The callback pattern

**Callbacks** - are functions that are invoked to propagate the result of an operation and this is exactly what we need when dealing with asynchronous operations. They do replace the use of the *return* instruction that always executes synchronously.

Another ideal construct for implementing callbacks is **closures**. With closures, we can in fact reference the environment in which a function was created; we can always maintain the context in which the asynchronous operation was requested, no matter when or where its
callback is invoked.

#### The continuation-passing style

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

### Deferred execution (63)

