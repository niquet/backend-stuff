# Concurrency in Go

- Goroutines: functions that are created and scheduled to be run independenlty by the Go scheduler
- Go scheduler: reponsible for the management and execution of goroutines
- We must always maintain an account of running goroutines and shutdown cleanly
- Concurrency is not parallelism
- Concurrency is about dealing with lots of things at once
- Parallelism is about doing lots of things at once
- 

---

## Go scheduler semantics

- The scheduler is a new layer of abstraction on top of the operating system that moves execution control to the application level
- Whenever a Go program starts, the runtime asks the (virtual or physical) machine how many threads can run in parallel (number of cores)
- The runtime creates an operating system thread (M) and attaches it to a data structure that represents a logical processor (P) inside the program (for each available core)
- P and M represent the compute power or execution context for running the Go program
- An initial Goroutine (G) is created to manage the execution of instructions on a selected M/P
- M manages the execution of instructions on the hardware
- G manages the execution of instructions on M

---

## Concepts and definitions

- Work: set of instructions to be executed for a running application (accomplished by threads, an application can have 1-N threads)
- Thread: path of execution that is scheduled and performed (threads are responsible for the execution of instructions on the hardware)
- Thread states: a thread can be in one of three states: Running, Runnable, or Waiting
  - Running: the thread is executing its assigned instructions on the hardware by having a goroutine placed on the operating system thread
  - Runnable: the thread wants time on the hardware to execute its assigned instructions and is sitting in a run queue
  - Waiting: the thread is waiting for something before it can resume its work
  - Waiting threads are not a concern of the scheduler
- Concurrency: undefined out of order execution: given a set of instructions that would be executed in the order provided, they are executed in a different undefined order, but all executed
  - Work can be done concurrently when the order the work is executed in doesn’t matter, as long as all the work is completed
  - The result of executing the full set of instructions in any undefined order produces the same result
- Parallelism: doing a lot of things at once (requires the ability to physically execute two or more operating system threads at the same time on the hardware)
- CPU bound work: work that does not cause the thread to naturally move into a waiting state
- I/O bound work: work that does cause the thread to naturally move into a waiting state
- Synchronization: two or more goroutines that need to access the same memory location potentially at the same time need to be synchronized and take turns in order to avoid data races
- Orchestration: two or more goroutines that need to signal each other, with or without data need an orchestration mechanic; orchestration provides guarantees about concurrent work being performed and completed

---

## Concurrency basics

Example of a concurrency problem that requires orchestration:

```go
package main

import (
    "fmt"
    "runtime"
    "sync"
)

func init() {
    // Allocate one logical processor for the scheduler to use.
    runtime.GOMAXPROCS(1)
}

func main() {
    // wg is used to manage concurrency.
    // Below, a WaitGroup is constructed to its zero value state.
    var wg sync.WaitGroup
    // The Add method is called to set the WaitGroup to 2.
    // This will match the number of goroutines to be created.
    wg.Add(2)

    fmt.Println("Starting goroutines")
    // Create a goroutine from the lowercase function.
    go func() {
         lowercase()
         wg.Done()
    }()

    // Create a goroutine from the uppercase function.
    go func() {
         uppercase()
         wg.Done()
    }()

    fmt.Println("Waiting to finish")
    // Wait holds the main Goroutine from causing the function to return.
    wg.Wait()

    fmt.Println("Terminating program")
}

// lowercase displays the set of lowercase letters three times.
func lowercase() {

  // Display the alphabet three times.
  for count := 0; count < 3; count++ {
    for r := 'a'; r <= 'z'; r++ {
      fmt.Printf("%c\n", r)
    }
  }
}

// uppercase displays the set of uppercase letters three times.
func uppercase() {

  // Display the alphabet three times.
  for count := 0; count < 3; count++ {
    for r := 'A'; r <= 'Z'; r++ {
      fmt.Printf("%c\n", r)
    }
  }
}

```

- Orchestration problem: the main goroutine can’t allow the main function to return until the two goroutines finish their work
- A WaitGroup is a good choice for orchestration problems that don’t require data to be passed between goroutines
- Signaling here is performed through an API that allows a goroutine to wait for other goroutines to signal they’re done
- When calling `sync.WaitGroup()`, a WaitGroup is constructed to its zero value state
- When you know upfront how many goroutines that will be created, you should call `wg.Add()` once with that number
- When you don’t know (like in a streaming service) then calling `wg.Add(1)` is acceptable
- `wg.Done()` decrements the WaitGroup by 1
- Calling `Wait()` blocks the main goroutine until the WaitGroup is set back to 0
- Once the WaitGroup will is set back to 0, the main goroutine will be allowed to be unblocked from the call to Wait
- When the main function returns, the Go program is shut down
- This is why managing the orchestration with the proper guarantees is important
- An important part of this orchestration pattern is keeping the `Add` and `Done` calls in the same line of sight
- Try not to pass the WaitGroup as a function parameter where the calls get lost (helps reduce bugs)

- Literal functions are declared and executed with the use of the keyword `go`
- Running a function with `go` tells the Go scheduler to execute these functions concurrently (in an undefined order)

- If you forget to call Done in one of the goroutines, the program would deadlock since the WaitGroup can’t get back down to 0
- The Wait call will block forever

**Note**:
- `GOMAXPROCS` is used to run the the Go program in a single thread
- Single threaded programs have a single P/M to execute all goroutines
- The function call `runtime.GOMAXPROCS(1)` will overwrite the underlying environment variable
- When passing `0` to `runtime.GOMAXPROCS()`, the number of threads the Go program will be using is reported: `g := runtime.GOMAXPROCS(0)`
- In containertized environments you need to make sure that number matches the number of operating system threads you have available, otherwise the Go program won’t run as well as it could

---

## Preemptive scheduler (context switching)

- The schedule of the application scoped Go scheduler is preemptive
- You can’t predict when a context switch will take place
- And this will change every time you run the program
- In the above example, 

Example of context switches:

```go
```

---

## Data races

- 

---

## Credits

- [Goroutines](https://tour.ardanlabs.com/tour/eng/goroutines/1) by Ardan Labs
- []() by
