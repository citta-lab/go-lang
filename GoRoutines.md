Go routines in Go






Lets imagine we have a program which has two tasks, `taskOne` is to validate the user & `taskTwo` is to fetch the data from database for validated user. So we would go ahead and write a program in synchronization fashion like this,
```go
package main

import (
	"fmt"
	"time"
)

func taskOne(){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task one completed, user validated")
}

func taskTwo(){
   time.Sleep(4 * time.Second) 
   fmt.Println("Task two completed, user data fetched")
}

func main() {
    taskOne() // happens first
	taskTwo() // happends second
	fmt.Println("~Main thread ended~")
}
```
In above example we have only one go routine by `main` and `main` will only be finished once the `taskOne` and `taskTwo` are completed. Hence this is more of synchronization fashion. 

Result of running the above code:
```go 
Task one completed, user validated
Task two completed, user data fetched
~Main thread ended~
```

## Concurrent Go routines :
Lets convert these tasks to go routine so they can be spinned off of main thread by it's own, more of asynchronous fashion.
```go
package main

import (
	"fmt"
	"time"
)

func taskOne(){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task one completed, user validated")
}

func taskTwo(){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task two completed, user data fetched")
}

func main() {
    go taskOne()
	go taskTwo()
	fmt.Println("~Main thread ended~")
}
```
In this example, taskOne and taskTwo are span its own and main go routine doesn't have any control over them. All these go routines will exit when the code it suppose to execute completes and if the main go routine execution completes. So `main` doesn't necesessarly wait for other go routines in our example and hence we will only see `~Main thread ended~` as output. This is the perfect example of `early exit`.

We can make the main wait for wait by adding a timer in main thread to wait for some pre-calculated / imaginary time so rest of the go routines can finish executing by then. THIS IS A BAD PRACTICE, as we would never know the time required to finish the `taskOne` and `taskTwo` go routines in real scenario due to `interleaving`. But to prove our theroy we can add timer as mentioned below,
```go
package main

import (
	"fmt"
	"time"
)

func taskOne(){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task one completed, user validated")
}

func taskTwo(){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task two completed, user data fetched")
}

func main() {
    go taskOne()
	go taskTwo()
	time.Sleep(4 * time.Second) // bad practice though
	fmt.Println("~Main thread ended~")
}
``` 
When we execute the program we still get undeterministic result due to interleaving, the only problem we kind of solved is not letting the `main` go routine finish before other two go routines ended.
```go
// RESULT : #1
Task two completed, user data fetched
Task one completed, user validated
~Main thread ended~

// RESULT : #2
Task one completed, user validated
Task two completed, user data fetched
~Main thread ended~
```
But there is a better way to achieve the same without adding timer in the main go routine and praying that it wont end until all other go routines completed executing. We will talk about that in synchronization.

## Synchronization using WaitGroup :
Program should always produce deterministic result upon execution. Lets fix this by using a synchronization package available from go and all go routines would register to this GLOBAL EVENT to work in synchronization.
```go
package main

import (
	"fmt"
	"time"
	"sync"
)

func taskOne(wg *sync.WaitGroup){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task one completed, user validated")
   wg.Done()            // removing the counter for first go routine
}

func taskTwo(wg *sync.WaitGroup){
   time.Sleep(2 * time.Second) 
   fmt.Println("Task two completed, user data fetched")
   wg.Done()           // removing the counter for second go routine
}

func main() {

    var wg sync.WaitGroup

    wg.Add(1)          // registering for first go routine
    go taskOne(&wg)
 
	wg.Add(1)          // registering for second go routine
	go taskTwo(&wg)
	
	wg.Wait()          // allowing for waitGroup to be finished before ending
	fmt.Println("~Main thread ended~")
}
```
But in this case we should be cautious to Add, Done and Wait methods to make use of the `WaitGroup` synchronization. Hence `Wait` is asscoiated with the group it only gurantees all the go routines are finished before calling the main but order of execution might still differ due to interleaving.

## Synchronization using Channels :
Go also provides `Channels` to communicate between the go routines safely and these communications are synchronous in fashion, perhaps we can make use of this synchronization dependecy to make the `main` go routine to wait for other go routines.
```go
package main

import (
	"fmt"
	"time"
)

func taskOne(c chan<- int){
   time.Sleep(4 * time.Second) 
   fmt.Println("Task one completed")
   c <- 1            // adding value to channel c
}

func taskTwo(d chan<- int){
   time.Sleep(8 * time.Second) 
   fmt.Println("Task two completed")
   d <- 2           // adding value to channel d
}

func main() {
    fmt.Println("Main thread started...")
    c := make(chan int)
	d := make(chan int)

    go taskOne(c)

	go taskTwo(d)
	
	first := <- c         // receiving from channel c and dumpting the value 
	second := <- d	     // receiving from channel d and dumpting the value 
    
    fmt.Println(first)
    fmt.Println(second)
	fmt.Println("~Main thread ended~")
}
```
In this case order of taskOne and taskTwo execution might differ but the value of first, second from the channel will always be in Order. i.e there is a synchronization between sending data to channel like here `c <- 1` and receiving data like `first := <- c`.
