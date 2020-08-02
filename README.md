# Go Lang

## Pointers:
- Go is a compiled language but garbabge collection built into it. Typically compile languages such as C, C++ doesn't have automatic garbage collection and deallocating memory is painful process.
- Similar to javascript lexical scoping go uses the concept of `shadowing` to refer to inner scope variables before reaching out to it's immediate outer scope variable.
- By default go uses `float64` when variable is defined with floating number. If we need precise floating pointer then it needs to be declared using `var`. Example: `var price float34`.
- By design if the variables are declared but not used then go will throw an error at compilation ( in javascript we had to use linter to detect ). i.e it is a compile time error.
- Upper case variable name is to allow the variable to be accessed outside of the package.
- Acronyms should always be upper case. Ex: `var theURL string = 'http://github.com'`. i.e URL
- In go we can declare a variable in 3 ways. Declare a variable and initialize later, declare and initialize together and lastly colon syntax to initialize without declaring.
- Go doesn't do implicit conversion and hence we need to use Type conversion.
- Cant add two different type integers. Example: In `var first int8 = 13` and `var second int32 = 10` we can't do `first + second`. To make it work we need to do type conversion like `first + int8(second)`.
- In bit shifting i.e ( 8 << 3 ) we would be multiplying 8 i.e ( 2^3 ) with (2^3). Similarly, if we are bit shifting 8 >> 2 then we would be diving by 2^2 so it would be 8 (i.e 2^3) / 2^2.
- While sending strings between web services in go we often will change it to byte rather than just strings. Example: `s := 'Bob was here'` would be converted to byte like `b := []byte(s)` so the data would be like `[66 111 98 32 119 97 115 32 104 101 114 101]`.
- Strings in go are `alias for bytes` so if we try to extract the index value of string we will get corresponding byte value. Also strings are immutable and need to use double quotes `"value"` instead of `'value'`.
- By adding * infront of the type we can decalre a pointer. Example `var x *int` declares x as an pointer.
- We cannot assign values from `int 16` with `int 32` without type conversion becuase the compiler will treat these as different types though they are of int. So we can do var x int32 = int32(y) where y is int16 type initially.



## Installation ( Mac ):

#### 1. Brew Install
In this process we will be using `homebrew` and then configure GOPATH in our profile.
```shell
brew update
brew install golang
```  

#### 2. Create Workspace
```shell
mkdir /Users/username/Documents/Go-Works/golib
mkdir /Users/username/Documents/Go-Works/code
```
then create `src`, `bin`, and `pkg` under code.

`src`: Single directory used by go path for searching source code
`bin`: Any binary created by our project will reside here
`pkg`: Intermediate binary mainly used while using third party library

#### 3. Update Profile
In root of the computer we would probably need to update `bash_profile, bashrc or .zshrc ` files. In my case `.zshrc`.
```shell
## 1. GO ROOTS:
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOROOT/bin

## 2. Compound GO PATHS:
export GOPATH=/Users/username/Documents/Go-Works/golib  #keeps all library code away from our actual code
export PATH=$PATH:$GOPATH/bin
export GOPATH=$GOPATH:/Users/username/Documents/Go-Works/code #actual code path
```
then `source ~/.zshrc` to load the latest profile.

#### 4. Install library
The main reason of having this compound path is to separate the downloaded bin,pkg,src files from that of our files.
```
go get github.com/nsf/gocode
```
will install `bin` and `src` in golib and keeps the `code` folder clean.

#### 5. Go Tools
When we install go, it provides `Go Tools` which helps in managing the go code. There are several commands such as, 

- `go build` : By default it will look for local directory and builds the executable ( i.e .exec).
- `go doc` : Pulls the documentation from the packages and print it.
- `go fmt` : Helps in formatting the go code as per the standards.
- `go get` : Downloads packages and installs them.
- `go list` : list all the installed packages.
- `go run`: compiles and runs the package if it's not already compiled.

#### 6. Running Go
As mentioned earlier, the GOPATH by default will look for src folder and we could have just had `first-app` as project inside it, it would work great however the idea of being consistency between source control and the work-space helps. So we will mimic the source control structure so it would be easy to manage. i.e under code > github.com > git_user_name > first-app.
```shell
# First Way: ( generates new executable in the parent src directory )
go build github.com/citta-lab/first-app  
./first-app

# Second Way:
go run github.com/citta-lab/first-app/main.go

# Third Way: ( generates new executable in the bin directory )
go install github.com/citta-lab/first-app
bin/first-app
```

#### 7. Main
There must be one package called `main` which will be convereted to executable. So main package should have `main()` function where the actual code execution starts. Keep in mind other packages ( other than main ) will not get converted to running executable.

```go
pacakge main
import "fmt"
func main(){
	fmt.Printf("hello, world\n")
}
```

## Variables :

Three different ways to declare a variables but in general varaible should have `KEYWORD NAME TYPE` format.

### Declaring:
Variable can be declared using the key word `var` by following this format `var name type`.
```go
var i int  // declare a variable i of type int
i = 38 // assigning value

//OR

var i int = 38 // initialize and assign in same line

// OR
i := 38 // this will make go figure out the type on fly and assigns a value and can only used with in the function
```
We could also make use of grouping while declaring variables in the package level, example
#### (i) Before :
```go
var name string = "Bob"
var age int = 12
var school string = "MIT"
```

#### (ii) After :
```go

var (
  name string = "Bob"
  age int = 12
  school string = "MIT"
)
```

### Scoping:
In go there are three layer of scoping.
1. Package Level
2. Block Level
3. Outside Package Level

```go
package main

var NAME string  // Can be accessed outside of this main package [ OUTSIDE PACKAGE SCOPE ]
var age int // Can only be accessed inside of this main package by all functions [ PACKAGE SCOPE ]

func main(){
  var school string // Can only be accessed with in the function [ BLOCK SCOPE ]
  var age int // Can only be accessed with in the function [ BLOCK SCOPE ]
}
```
Just by defining the variable with upper case we can let the variable to be accessed outside of the package.

### Type Conversion:

If we are interested in type conversion then in go we would be doing like `type (variable)`.
```go
var age int = 12
var ageFloat float
ageFloat = float64(age)

// In Java, it would be like
double myDouble = 12.3;
int age = (int) myDouble;
```
If we are interested in converting int to a string then we need to use the package `strconv` in go. Below is the example of showing the usecase,
```go
import (
	"fmt"
	"strconv"
)

func main() {
  var age int = 38
	fmt.Println(age) // prints 38

	var newAge string
  newAge = string(age)
	fmt.Println(newAge) // prints & as number 38 is reference to ASCII character &

	var newAgeCon string
	newAgeCon = strconv.Itoa(age) // pronounced as : string convert into a age
	fmt.Println(newAgeCon) // prints 38
}
```

## Primitives :
Integer in go by default will be at least 32 byte, However we can chose the size of the int from 8 to 62 byte. That being said if we want variable age of 32 byte integer then we would write `var age int32`. If we only want positive number rage from 0 then we can use unsigned integer like `var age uint32` but we don't have 64 byte unsigned integer.
- Complex type ( complex64, complex128)
- Int type ( int, int8, int16, int32, int64 )
- Float type ( float, float32, float64 )
- bool type
- Text type ( string, Rune )

## Data Types 
### Pointers : 

Pointers is an address to the data in memory. This comes with two operators, `&` which helps in retriving the "address" of the variable or a function meanwhile `*` operator returns the variable or a function when used with address. So they work exactly opposite to each other. 

Example:
```go
func main() {
	fmt.Println("Data Types Example")
	var x int = 10

	var ip *int = &x
	var y int = *ip

	fmt.Println("X variable address", ip)
	fmt.Println("X value from pointer adress of X", y)
}
```
By using `new()` we can also create an variable, however instead of returning an variable it returns the `pointer` to that variable. So in the first step we would create an pointer using new function then assign a value to that pointer address. Example: 

```go
func main() {
	fmt.Println("New : Example")
	var x = new(int) // creating a pointer to x
	*x = 12; // assigning value to that pointer

	var y int = *x // getting value from pointer
	fmt.Println("X variable address", x);
	fmt.Println("X value from pointer",y);
}
```

### Deallocating Memory:
Whenever the program done executing it `deallocates` the memory assigned to variable so it can free up the space. This helps in not eating up the memory. Stack and Heap are two such thing, 

In stack, memory will be deallocated once the function execution is complete. In this example `x` will be deallocated after execution 
```go
func main(){
	var x int = 10;
	fmt.Println("Stack Example")
}
```
In Heap, Generally memory will not be deallocated after the function execution as it sits in the outer scope of the function. Hence heap is persistent. In other launguages we explicitly `deallocate` the memory of x ( example: C ).
```go
var x int = 100;
func main(){
	fmt.Println("Heap Example")
}
```

### ASCII vs UNICODE
ASCII is 8-bit code so all the letters in english can be represented by just using ASCII, However languages like Chinese requires more bits to cover all the character. So Unicode comes into picture, which is of 32-bit character code.

UTF-8 is of variable length ( subset of unicode), but the first 8-bit of unicode is similar to ASCII but UTF-8 can hold much more character compared to ASCII if needed. Aka it can go upto 32-bit. UTF-8 is the DEFAULT character code used in Go. 

Code Points : is the term for Unicode Characters. i.e 2^32 code points.
Rune: Is the term for Code Point.

Example:
```go
fmt.Println("Hi")
```
In this case `H`, `i` is represnted as a Rune which is nothing but a UTF-8 code point.

### Strings, Constants
`String is Immutable`
Strings are nothing but array of Runes, remeber runes are nothing but a representation of UTF-8 character coding. There are few methods we can use on Strings provided by go,
`IsSpace()`, `IsDigit()`, `IsLetter()`, `IsLower()`,`IsPunct()` and manipulating functions such as 	`Compare(a, b)`, `Contains(s, substring)`, `HasPrefix(s, prefixt)`, `Index(s, substring)`.

In Go we have a package called `Iota` which can be used to create unique constats. This acts as enemuration in other languages. In `Iota` actual value is not important but the uniqueness is. Example:
```go
type Problems int; // defined Problems as int type
const (
	X Problems = iota // declared X as type int and assigned iota
	Y // compiler refers the topline and declared Y/ as type int and assigned iota
	Z // same as above
)
```


## Questions:
1. Hows Go diff from other languages ? 
- It is a compiled langauge with automatic grabage collection. So this sits inbetween pure compiled language (c, c++ ) vs interpretted langauge ( python ). This might slows down the performace of go compared to compiled langauge but it's not much.

2. Hows Go's `Structs` diff from OOP's `Class` ? 
- Typically Class has data types ( like variables ) and functions to operate on them and we instantiate these classes to create new object. However the idea of `Structs` is different type of data with methods to operate on them. So structs will not have below compared to class,
(i) No Inheritance 
(ii) No Constructor 
(iii) No Generics 

3. What is concurency in go ?
In general, ability of the machine to run the program in parallel but that doesn't mean it runs the program at the time but the core is alive at the same time to handle the task. Go is desgined to handle this concurent programming by means of `primitives`, 'go routines' ( aka threads ), `channels` to communicate between tasks and `select` to enable task synchronization.











# References:
1. FreecodeCamp 6 hour introduction : https://www.youtube.com/watch?v=YS4e4q9oBaU
2. Installing Go using homebrew : https://sourabhbajaj.com/mac-setup/Go/README.html
