# Node JS fundamentals

In this article, I will discuss some Node.js fundamentals that every
beginner needs to know to improve the code quality of a Node-based web
application.

## NodeJS main components

- **Core JavaScript API**: Responsible for the high-level implementation.
- **V8 Engine**: Responsible for interpreting JavaScript code and executing the program.
  It's written in C/C++ and implements the core functionality of JavaScript.
- **libuv**: A library written in C/C++ responsible for event manipulation, asynchronous
  processing with functions and timers, implementing the Event Loop, and managing
  thread pools. It also handles high-demand CPU code execution. It's a crucial
  module in Node.js as it provides access to the operating system.

## Threads and processes

A fundamental concept to properly understand how Node.js works is the distinction
between a thread and a process, as they are used by computers to execute
instructions.

### Process/Task

In simple terms, a process is a set of program instructions to be executed. A computer
executes many processes simultaneously, which can be considered tasks that the
operating system runs periodically, such as antivirus programs. Each process has its
own memory space and `does not share memory` with other processes.

### Thread

A thread is the `smallest processing unit of a program` and represents how the
instruction set is divided and scheduled for execution. A process is composed of
threads that `share memory` for efficient execution of different instructions.

## is Node single threaded?

Node.js is considered single-threaded because its main component for
execution, the Event Loop, relies on only one thread for execution, called
the `main thread`.
However, it's important to note that libuv has, by default, 4 threads that can be used
for high-demand CPU processing, allowing Node.js to act as a multi-threaded runtime in certain scenarios.

## Event Loop

Before introducing about the event loop we need to gather some extra concepts,
`Call Stack` and the `Task Queue`.

### Call Stack

This is a component present in many programming languages for keeping track of
the `program execution` and it's `function calls` in a Stack data structure. It's functioning
can be expressed as a LIFO (Last in first out).
It's important to know that `callbacks` in Node are just executed if the Call
Stack is `empty`, without executing a particular function at the time.

### Task Queue

The task queue is used for example by `timers functions` such as the setTimeout
that receives a `callback` and a `timer` for it to be executed. When call those
types of functions they are registered in the Node's API timer queue and when the time of
execution is reached the callback is inserted in the task queue for execution.
Since a queue is a `FIFO` (First in frist out) data structure they callbacks will
be executed in insertion order.

### Understanding the Event Loop

The `Event Loop` is fundamental for the node js runtime since it makes possible
for the non-blocking I/0 strcuture of the language work. It's responsible for
making node application really good with reading files, processing http requests
and querying databases.

### Event Loop Phases

We can devide the `Event Loop` in some phases of execution:

1. **Timers Phase**: Executes callbacks scheduled by `setTimeout()` and `setInterval()`.
2. **Pending Callbacks Phase**: Processes I/O callbacks that were deferred to the next loop iteration.
3. **Poll Phase**: Retrieves new I/O events and executes their callbacks. If no callbacks are
   scheduled, it waits for new events.
4. **Check Phase**: Executes callbacks scheduled by `setImmediate()`.
5. **Close Callbacks Phase**: Handles close events, like when a socket or handle is closed.

The loop repeats these phases as long as there are tasks in the `Call Stack`
to be executed.

## Non-blocking I/0

I/O represents any system interaction with the disk (file manipulation, database
querying) or with the network (HTTP requests). When writing JavaScript code, we
can have `blocking` and `non-blocking` calls. By understanding how the
`Event Loop` works, we can write code that won't block it.

### Blocking calls

We say we have a blocking call when a non JavaScript NodeJS code is being
executed and it will not allow any other process to be executed until it finishes.
This behavior pauses the execution of the `Event Loop` since it can't continue
to execute any process.

_Note_: High intense CPU performance process aren't considered blocking calls

#### Example from the Node JS Documentation

```JavaScript
const fs = require('node:fs');

const data = fs.readFileSync('/file.md'); // blocks here until file is read
console.log(data);
moreWork(); // will run after console.log
```

In this example we have a blocking operation that will prevent the Event Loop
from executing callbacks.

```JavaScript
const fs = require('node:fs');

fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
  console.log(data);
});
moreWork(); // will run before console.log
```

In this example the Event Loop will not be blocked and we will depend on the
resolution of the async operation to have the data displayed.

### Async/await is a blocking operation?

Async/await is a sintax sugar built on top of promisses and those operations do
not block the `Event Loop`. While waiting for the promise to be resolved the
`Event Loop` continues it's execution to execute callbacks and receive another
I/O operations.

#### Example of async/await

```JavaScript
async function fetchData() {
  console.log("Fetching data...");
  const response = await fetch("https://api.example.com/data"); // Pauses here
  const data = await response.json();
  console.log("Data received:", data);
}

console.log("Start");
fetchData(); // This runs asynchronously
console.log("End");
```

The execution of this program would return

```terminal
Start
Fetching data...
End
Data received: { ... }
```

This happens because when a Promise is returned by the fetch method it will stop
the execution of the function until it resolves the promise but because the
`Event Loop` isn't blocked it will continue to executed the code normally and
when the promise is resolved it will scheduled to continue the function
execution
Really understaing this behavior is key for developing JavaScript Node JS code
without any unexpected behavior.

## Conclusion

In this article, we covered many core concepts of Node.js that
beginners often overlook at the start of their journey. However, understanding
these concepts is crucial for writing efficient and scalable applications.
F
