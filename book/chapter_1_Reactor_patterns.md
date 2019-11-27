## The reactor pattern

I/O - input/output - *is the communication between an information processing system, such as a computer, and the outside world, possibly a human or another information processing system.*

**I/O is slow** - it adds a delay between the moment the request is sent and the moment the operation completes

In addition to **blocking I/O**, most modern operating systems support another mechanism to access resources called **non-blocking I/O**. In this operating mode, the system call always returns immediately without waiting for the data to be read or written. If no results are
available at the moment of the call, the function will simply return a predefined constant, indicating that there is no data available to return at that moment.

The most basic pattern for accessing this kind of non-blocking I/O is to actively poll the resource within a loop until some actual data is returned; this is called **busy-waiting**.

#### Event demultiplexing

Busy-waiting is definitely not an ideal technique for processing non-blocking resources, but luckily, most modern operating systems provide a native mechanism to handle concurrent, non-blocking resources in an efficient way; this mechanism is called synchronous event demultiplexer or event notification interface. This component collects and queues I/O events that come from a set of watched resources, and block until new events are available to process.

1. The resources are added to a data structure, associating each one of them with a specific operation, in our example, read.
2. The event notifier is set up with the group of resources to be watched. This call is synchronous and blocks until any of the watched resources are ready for read. When this occurs, the event demultiplexer returns from the call and a new set of events is available to be processed.
3. Each event returned by the event demultiplexer is processed. At this point, the resource associated with each event is guaranteed to be ready to read and to not block during the operation. When all the events are processed, the flow will block again on the event demultiplexer until new events are again available to be processed. This is called the **event loop**.

### Introducing the reactor pattern 

The main idea behind it is to have a handler (which in Node.js is represented by a **callback** function) associated with each I/O operation, which will be invoked as soon as an event is produced and processed by the event loop. 

**This is what happens in an application using the reactor pattern:**
1. The application generates a new I/O operation by submitting a request to the Event Demultiplexer. The application also specifies a handler, which will be invoked when the operation completes. Submitting a new request to the Event Demultiplexer is a non-blocking call and it immediately returns control to the application.
2. When a set of I/O operations completes, the Event Demultiplexer pushes the new events into the Event Queue.
3. At this point, the Event Loop iterates over the items of the Event Queue.
4. For each event, the associated handler is invoked.
5. The handler, which is part of the application code, will give back control to the Event Loop when its execution completes. However, new asynchronous operations might be requested during the execution of the handler, causing new operations to be inserted in the Event Demultiplexer, before control is given back to the Event Loop.
6. When all the items in the Event Queue are processed, the loop will block again on the Event Demultiplexer which will then trigger another cycle when a new event is available.


The Node.js core team created a C library called **libuv**, with the objective to **make Node.js compatible with all the major platforms and normalize the non-blocking behavior** of the different types of resource; libuv today represents the lowlevel I/O engine of Node.js.

### The recipe for Node.js

**The reactor pattern and libuv are the basic building blocks of Node.js, but we need the following three other components to build the full platform:**
+ A set of bindings responsible for wrapping and exposing libuv and other lowlevel functionality to JavaScript.
+ V8, the JavaScript engine originally developed by Google for the Chrome browser. This is one of the reasons why Node.js is so fast and efficient. V8 is acclaimed for its revolutionary design, its speed, and for its efficient memory management.
+ A core JavaScript library (called node-core) that implements the high-level Node.js API.
