# Go Routines

Go routines are a lightweight thread in Go. Go has it's own scheduler that deals with go routines and sends the information for the OS Scheduler to execute the work load.

| Feature                    | **Goroutine**                                    | **OS Thread**                          |
| -------------------------- | ------------------------------------------------ | -------------------------------------- |
| **Managed by**             | Go runtime scheduler                             | Operating System                       |
| **Creation cost**          | Very low (\~2 KB stack)                          | High (MBs of stack + syscalls)         |
| **Scalability**            | Can handle **millions** of goroutines            | Limited to **thousands** of threads    |
| **Switching**              | **User-space scheduling** (fast, no kernel trap) | **Context switch via kernel** (slower) |
| **Blocking call behavior** | Typically non-blocking or handled by runtime     | Blocks the whole thread                |
| **Scheduling**             | Go scheduler maps goroutines to OS threads       | OS scheduler assigns threads to cores  |
