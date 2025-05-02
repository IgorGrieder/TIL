# Mutexes

Mutexes (Mutual Exclusion) are another way of sharing data in a safe way between Go routines just like Channels in Go. They are a part of the sync standard library and use a lock and unlock strategy and it excludes the execution of all go routines that uses the mutex when one `.lock()` is called. The lock can just be "in the hand of one go routine"

## Maps

This can be useful when writing to a `map` because multiple Go routines changing it's value at the same time will cause a `panic` in the code, so we must use mutexes.
So if you know there's going to be a change on a map in a Go routine it's probably a good idea to have a mutex lock in it to avoid `race conditions`.

```Go
type SafeMap struct {
 Mux *sync.Mutex
 Sm  map[string]int
}

func main() {
 SafeMapInstance := SafeMap{Sm: map[string]int{"a": 1}}

 go func(sm SafeMap) {
  sm.Mux.Lock()
  defer sm.Mux.Unlock()

  sm.Sm["a"] = 2
 }(SafeMapInstance)
}
```

## RWMutex

Writing to a map for instance is a dangerous operations along different go routines, but not reading from it. That's why RW mutexes exist, to allow reading from different go routines to a resource.
