# Channels

Channels in go are a thread safe way of communication. It's a type `chan` in Go and we must use it to be able to safely communicate between go routines. It works as a safe `queue` for values.

## Usage

As said a channels is used between go routines that need to use/receive an specific data.
To send a message into and get an message from we use the `<- operand`.

A receiver will be blocked until a message is in the queue and a sender will be blocked until it has an receiver to receive the value from the channel. Both situations not being satisfied it will cause a `deadlock`. The code on the Go routine will not continue if the channel value isn't used, and the code outside of it won't run until a message is in the channel.

Example

```Go
func main(){
  // Channel that will handle one int variable
  ch := make(chan int)

  go func(ch chan int){
    // This will be blocked until the channel has a value
    x := <-ch
    fmt.Println(x)
  }(ch) // Calling the anonymous function with the channel as parameter

  // Populating the channel
  ch<-10
}
```

We can set sizes for the channels queue, changing a little bit of it's usage. The described behavior of locks, in a sender perspective, now will change to not being blocked until the size of the channel is not full. For a receiver it continues the same behavior of blocking just when it is empty.

```Go
func main(){
  // Won't block until the queue is full
  ch := make(chan int, 3)

  go func(ch chan int){
    for i := range 10 {
      ch<-i
    }
  }(ch)

}
```

## Closing Channels

We can close a channel using the `close()` method. This must only be done by the sender perspective, never the receiver since it's go routine doesn't know when the channels is not going to be used anymore.

We can track if a channel is closed or not by a second return from the channel.

```Go
v, ok := <-channel
// Ok will be a boolean if the channel is open or not
```
