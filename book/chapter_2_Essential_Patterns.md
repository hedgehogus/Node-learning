## The callback pattern

**Callbacks** - are functions that are invoked to propagate the result of an operation and this is exactly what we need when dealing with asynchronous operations. They do replace the use of the *return* instruction that always executes synchronously.

Another ideal construct for implementing callbacks is **closures**. With closures, we can in fact reference the environment in which a function was created; we can always maintain the context in which the asynchronous operation was requested, no matter when or where its
callback is invoked.

#### The continuation-passing style

In JavaScript, a **callback** is a function that is passed as an argument to another function and is invoked with the result when the operation completes. In functional programming, this way of propagating the result is called **continuation-passing style (CPS)**. 

**An unpredictable function** - one of the most dangerous situations is to have an API that behaves synchronously under certain conditions and asynchronously under others. 

Unleashing Zalgo (60)
