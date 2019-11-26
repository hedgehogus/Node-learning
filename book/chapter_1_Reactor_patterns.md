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

### Introducing the reactor pattern (50)
