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

---

### 🧠 What is `sync.RWMutex`?

It's a **reader/writer mutex**:

- It allows **multiple readers at the same time** (`RLock`).
- But **only one writer** (`Lock`), and **no readers can run while writing**.

---

### 🔍 What happens when you call `RLock()`?

- If **no `Lock()` is active or waiting**, it proceeds immediately.
- If a **`Lock()` is currently active or pending**, then `RLock()` **blocks** until the writer is done.

> 💡 Multiple goroutines can hold `RLock()` concurrently — that's the main advantage over a regular `Mutex`.

---

### ✍️ What happens when you call `Lock()`?

- If there are **active `RLock()` holders**, `Lock()` **waits** until all `RUnlock()` calls have been made.
- Once `Lock()` succeeds, **no other `RLock()` or `Lock()` calls can proceed** until the `Unlock()` is called.

---

### ⏳ Priority

> **Writers have priority over new readers.**

This means:

- If readers are active and a writer is waiting, **new `RLock()` calls will block** until the writer gets the lock.

This prevents **starvation** of writers in read-heavy systems.

---

### 🧪 Example with timing

```go
var mu sync.RWMutex

go func() {
 mu.RLock()
 fmt.Println("Goroutine 1: Read lock acquired")
 time.Sleep(2 * time.Second)
 mu.RUnlock()
 fmt.Println("Goroutine 1: Read lock released")
}()

go func() {
 time.Sleep(500 * time.Millisecond)
 mu.Lock()
 fmt.Println("Goroutine 2: Write lock acquired")
 mu.Unlock()
 fmt.Println("Goroutine 2: Write lock released")
}()

go func() {
 time.Sleep(1 * time.Second)
 mu.RLock()
 fmt.Println("Goroutine 3: Read lock acquired")
 mu.RUnlock()
 fmt.Println("Goroutine 3: Read lock released")
}()
```

### Output (approximately)

```
Goroutine 1: Read lock acquired
// (Goroutine 2 waits for Goroutine 1 to finish)
Goroutine 1: Read lock released
Goroutine 2: Write lock acquired
Goroutine 2: Write lock released
// (Goroutine 3 was blocked until writer finishes)
Goroutine 3: Read lock acquired
Goroutine 3: Read lock released
```
