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
- By default if an array is not initialized then it will be initialized to `0` in int type array and " " in string type array.
- `[...]` in go is used to declare the size of array literals. So this [...] will count the number of elements assigned and declares the size instead of us mentioning the size. Example: `var arr [3]int = [3]{ 23, 1, 7}` can be written as `var arr [3]int = [...]{ 23, 1, 7}`. So array literals are nothing but an array with predefined values.
- Key thing about Arrays in go is `Fixed length` so the compilers knows before hand.
- Slices can update the underlying array which it is windowing.
- RFC ( Requests for Comments ) is definition of protocols and format via internet. Example: HTML, HTTP, URI
- File access is linear access and go has package called `ioutil` as long as the file is not huge. Example: `ioutil.ReadFile("test.txt")` similarly writing would be `ioutil.WriteFile("text.txt", "my data", 0777)` where 0777 is the permission. But the main problem with `ioutil` will read/write complete file vs `os` package can only read certain bytes.
- Declaring types once instead of repeating when arguments are off same type. Example: `func test(x int, y int)` can be written as `func test(x, y int)`.
- Return type can be defined in the function signature so above example would look like `fun test(x,y int) int { return x+y }`.
- Functions are First Class citizen in go. Function can be declared as type example: `var funExample func(int) int`
- `defer` can be used to execute the function at the end of that parent function execution however defer function will be evaluated immediately but not executed until the end.
>> Common things between Go & Node are `Lexical` scoping, `Closure`, can write `anonymous function`, functions are `first class object` so can pass as argument, return a new function etc, function can have variable number of arguments by using `...`,

- no `Class` keyword in go.
- `type` should be defined in the same package it has been used.
- By default all the variables, types defined in the package are PRIVATE unless we expose them using a function ( controlled functions )
- While using `receiver type` it is good practice to stay consistent by using either `call by reference` aka pointer or `call by value`.
- Go lang doesn't have `Inheritance`.



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

### Composite Datatypes:
#### Arrays & Array Literals
Arrays are declared as `var ages [2]int`. So arrays must have defined length. Similary in short hand declaration we can do `ages := [2]int`. Array Literals are nothing but an array with initialized values. So now our example becomes,
```go
var ages [2]int = [2]{ 20, 23}
// alternatively
var ages [2]int = [...]{ 20, 23} // [...] validates the size on fly and assigns it
// alternatively shirt hand
ages := [...]{20, 23}
```
`range` can be used to loop through the array which returns two properties index and value. for example
```go
for i, age range ages {
	fmt.Println("index %d, age %d", i, age)
}

```

#### Slice & Slice Literals
Slcies are subset of an array or `window` of an array. Slice has three properties `pointer` to indicate the start of the slice, `length` number of elements in the slice and lastly `capacity` is maxium size it can hold ( which depends on the underlying array )
```go
func main() {
	names := [...]string { "bob", "jay", "matt", "drew" }
	s1 := names[1:3]
	fmt.Println(s1) // [ jay, matt ]
	fmt.Println(len(s1), cap(s1)) // 2 , 3 i.e size, capacity indicating how many more elements slice can have referring to names array
}
```
we can initialize the slice using slice literals similar to array literals instead we will not infer or define a size. Example:
```go
names := []{ "bob", "jay", "matt", "drew" } // is slice literal, notice empty []
names := [...]string { "bob", "jay", "matt", "drew" } // is array literal, notice [...]
```
so most often slice literals are used instead of using an array because of the flexibility. In our slice literal example, slice will be defined with length and capacity of same size and pointer will start at index 0.
We can make use of `make` to create new slice with size or size and capacity and use `append` to add the item to slice which automatically increases the capacity once it exceeds the underlying array length. Example:
```go
func main() {
	/** Diff ways to make slices using make */
	var exampleOne = make([]int, 10)
	fmt.Println(len(exampleOne), cap(exampleOne)) // 10, 10

	var exampleTwo = make([]int, 10, 15)
	fmt.Println(len(exampleTwo), cap(exampleTwo)) // 10, 15

	exampleTwo = append(exampleTwo, 25, 12, 2, 34, 45, 545)
	fmt.Println(len(exampleTwo), cap(exampleTwo)) // 16, 30
}
```

#### Hash Table & Map
Will have key/value pair where each key is `unique`. Undeneath the hood 'hash function' is used by go lang to look for value asscoaited with the unique key. This is always `O(1)` lookup. But one thing we need to be aware is "Collision". Example: If two hash keys reference to same slot then there will be a collision.

`Map` is a implementation of Hash Table in go. we can make use of `make` function to create a map. Also can make have two paramters as return so we can get value of the key and true/false on whether the key exist. Example: `value, present := exampleMap["bob"]`. More example below,
```go
func main() {

	/* map example */
	var mapExample map[string]int     // map[key_type] value_type : declaring map
	mapExample = make(map[string]int) // : making an map

	/* ,map literal example */
	var mapExmTwo map[string]int
	mapExmTwo = map[string]int{"bob": 12, "rob": 23, "joe": 10} // initalizing map with values

	fmt.Println(mapExample) // empty map
	fmt.Println(mapExmTwo)  // map with bob:12 joe:10 rob:23

	/* GET, ADD, UPDATE, DELETE */
	fmt.Println(mapExmTwo["rob"]) // GET
	mapExmTwo["sunny"] = 100      // ADD
	fmt.Println(mapExmTwo)
	mapExmTwo["sunny"] = 10       // UPDATE
	fmt.Println(mapExmTwo)
	delete(mapExmTwo, "sunny")    // DELETE
	fmt.Println(mapExmTwo)
}
```
#### Struct ( aka Structure )
Arbitrary data type to hold object of any type. Think of like javascript object which can hold anything and `Struct` is a concept taken from C.
```go
func main() {
	type Person struct {
		name    string // name field
		age     int    // age field
		address string // address field
	}

	var p1 Person                                           // p1 will have all the fields defined in Person struct
	p2 := new(Person)                                       // initializing with empty struct using NEW
	p3 := Person{name: "Bob", age: 32, address: "45 Cidar"} // initializing struct with values

	fmt.Println(p1) // " " 0 " "
	fmt.Println(p2) //  " " 0 " " aka { 0 }
}
```
we made use of `new` keyword to declare a empty struct and also initialized with values for struct Person.

### Data Transfer
#### JSON
Go has a package to convert go object such as `struct` to json using marshaling and `json` object to struct using `un marshaling`. Below is the such example, pay close attention to json mapping with the struct field like `json:"name"` and also casing of struct field such as Address instead of address etc.

```go
func main() {

	type Person struct {
		Name    string `json:"name"`
		Age     int    `json:"age"`
		Address string `json:"address"`
	}

	/* Example 1: marshaling */
	p := Person{Name: "Bob", Age: 32, Address: "45 Cidar"}

	marshalData, _ := json.Marshal(p)

	fmt.Println(p)           // {Bob 32 45 Cidar}
	fmt.Println(marshalData) // [123 125 ........]

	/* Example 2: Un marshaling to p2 */
	var p2 Person
	b := []byte(`
		{
			"name":"Joe",
			"age":6,
			"address":"23 texas"
		}
	`) // wrapping json in byte
	err := json.Unmarshal(b, &p2)

	if err != nil {
		fmt.Println(" Problem processing into struct ")
	}

	fmt.Println(p2) // {Joe 6 23 texas}
}
```

### Call By Value vs Reference :
In this example we will understand normal call by value scenario and then change it to call by Reference. Keep in mind the only advantage of call by Reference is there no data copying rather just the address so it will be quick however it loses the data encapsulation provided by call by value.

```go
func callByValue (y int){
	y = y + 1
}

func main(){
	x := 10
	callByValue(x)
	fmt.Println(" X value is : ",x) // still 10
}
```
Now the same function can be written doing call by Reference where `x` value will get mutated from function
```go
func callByRef(y *int) {
	*y = *y + 1
}

func main() {
	x := 10
	callByRef(&x)
	fmt.Println(" X value is : ", x) // updated to 11
}
```
The same concept applied while handling arrays however this is much messier and you should always use SLICE instead.
```go
func callByRef(y *[5]int) {
	(*y)[2] = (*y)[2] + 10
}

func main() {
	x := [5]int{10, 3, 4, 56, 45}
	callByRef(&x)
	fmt.Println(" X value is : ", x) // updated to [10 3 14 56 45]
}
```
As we learned earlier slice takes three arguments: pointer, length & capacity. So by passing slice we are copying the pointer by default ( we had to do this explicitly in array )
```go
// modifying the slice
func callByRef(y []int) {
	y[2] = y[2] + 10
}

func main() {
	x := []int{10, 3, 4, 56, 45} // is a slice
	callByRef(x)
	fmt.Println(" X value is : ", x) // updated to [10 3 14 56 45]
}
```

### Function in Go
#### function as an Variable :
```go
// variable exampleFun of function with int type and returns int value
var exampleFun func(int, int) int

func addNum(first int, second int) int {
	return first + second
}

func main() {
	exampleFun = addNum
	fmt.Println("adding 2 and 3 = ", exampleFun(2, 3))
}
```
#### function as argument
```go
func applyInt(aFunc func(int) int, val int) int {
	return aFunc(val)
}

func incFun(val int) int {
	return val + 10
}

func decfun(val int) int {
	return val - 23
}

func main() {
	fmt.Println(applyInt(incFun, 100))
	fmt.Println(applyInt(decfun, 100))
}
```

#### function in closure
In this example we will make use of closure similar to javascript to return a function which can be executed in later point of time.
```go
func checkGravity() func(string, float64) float64 {
	var SI float64 = 9.81
	var US float64 = 32.2

	// using map to pick the user choice
	unitMap := map[string]float64{"SI": SI, "US": US}

	// anonymous function uses closre to refer unit value when this anonymous func called
	fn := func(userChoice string, value float64) float64 {
		unit := unitMap[userChoice]
		return value * unit
	}

	return fn
}

func main() {
	fn := checkGravity()
	siResult := fn("SI", 3)
	usResult := fn("US", 3)
	fmt.Println(siResult, usResult)
}
```
In this example SI, US unit values along with unitMap are referred from return anonymous function when executed using closure.

### Supporting Class
Class usually wraps different data types and functions defined with in the class called `methods`. So when we instantiate we get the reference to exposed data types and/or methods. Let us see how we can achieve something similar in go.
#### 1. Understanding Types :
In the earlier example we might have seen ways to use types, here are some more examples to cover different scenario
##### 1.1 One Type:
```go
type Name string

func main() {
	name := Name("Bob")
	fmt.Println(name) // Bob
}
```

##### 1.2 Struct Type:
In earlier example we were restricted to use just one data type (i.e string ) but in classes we have had used different types of data and associated them with class functions called methods. Hence it is important make use of `struct` which help us wrap different kind of data types.
```go
type Person struct {
	name string
	age  int
}

func main() {
	p := Person{name: "Bob", age: 12}
	fmt.Println(p) // {Bob 12}
}
```

#### 2. Receiver Type
In go there is no key word like `class` but we can associate data types with functions ( like methods in classes ) using receiver type. These receiver type need to be defined in the same file where the functions are referencing & receiver type should be defined in-front of function name like `func (receiver_type_here) funName { }`.
##### 2.1 Call by Value
 Lets look at one of the example below where `Person` is defined as type and added to `bioData` function as `receiver type`. In main function, `p` variable is created of type `Person` and result is evaluate by passing p as `call by value` into `bioData` method.
```go
type Person struct {
	name string
	age  int
}

// receiver type person is defined before function name
func (p Person) bioData() string {
	p.version = 10 // this will not update in Person type
	dob := 2020 - p.age
	return p.name + " was born on " + strconv.Itoa(dob)
}

func main() {
	p := Person{name: "Bob", age: 12}
	result := p.bioData() // using method associated with data ( person type )
	fmt.Println(result) // Bob was born on 2008
	fmt.Println(p) // {Bob 12 0}
}
```
This is great however there are some limitations such as (i) Hence `p` is a copy of receiver type ( call by value ) consuming/using methods wouldn't be able to update the type field values. Example: `bioData` wouldn't be able to change anything in Person type like adding version i.e ( `p.version = 10` in bioData function ) (ii) Hence this is call by value, these type field values need to be copied over to `bioData` function which might take time in large data type.

##### 2.2 Call by Reference ( Pointer )
we could address this issue by using a pointer in receiver type which will give access to filed memory. So the only change would be adding * infront of Person.
```go
type Person struct {
	name string
	age  int
}

// receiver type person is defined before function name
func (p *Person) bioData() string {
	p.version = 10 // this will update the Person struct type so everyone referencing Person will get updated version
	dob := 2020 - p.age
	return p.name + " was born on " + strconv.Itoa(dob)
}

func main() {
	p := Person{name: "Bob", age: 12}
	result := p.bioData() // using method associated with data ( person type )
	fmt.Println(result) // Bob was born on 2008
	fmt.Println(p) // {Bob 12 10} <== updated
}
```

#### 3. Interface
Go lang doesn't have 'Inheritance', the idea of extending the properties & methods from super class and overriding them in child classes to accomplish Inheritance & overriding. This same process of having same method in one or many child classes is called `Polymorphism`. However go uses `Interface` to achieve `Polymorphism` without Inheritance and/or method overriding. There are few key things we need to remember
- Interface will only have method signature without implementation like other languages.
- Consuming method doesn't have to declare / link or use `Interface` keyword while defining. Go handles that Implicitly.
- Method needs to implement all of the method signature from Interface to satisfy.
- Method can implement multiple `Interface` without needing to mention ( implicit ).
- Interface can be treated like a type. So we could make `var pInter Personal`
- Interface has Dynamic Val and Dynamic Type.
- Interface can have Dynamic Type but no Dynamic Value ( nil value ) which is valid.
- If there is no Dynamic Type and Dynamic Value then it is called `Nil Interface` and we cannot call the methods using interface instance as go compiler cannot refer to any type
- If the function takes multiple type of data types then use Interface. Example: `fmt.Println()` uses empty interface to take any types.

##### 3.1 Interface Example :
Below is the example,
```go
type Personal interface {
	BioData() int
	JobDetails() string
}

type Person struct {
	name string
	age  int
}

// no mention of using Interface or mentioning Interface name Personal here ( linked Implicitly )
func (p Person) BioData() int {
	return p.age
}

func (p Person) JobDetails() string {
	return "Engineer"
}

func main() {
	p := Person{ name: "Bob Marley", age: 43}
	fmt.Println(p.BioData()) // 43
	fmt.Println(p.JobDetails()) // Engineer
}
```

##### 3.2 Dynamic Type & Value:
Example Below for Dynamic Type and Dynamic Value. ( refer 3.1 example for complete program and continue with this modification )
```go
func main() {
	var pInter Personal // assigning interface to variable
	p := Person{ name: "Bob Marley", age: 43}
	pInter = p // is Okay, because Person implemented all the Interface methods
	fmt.Println(pInter.JobDetails()) // Engineer
}
```
So in this case `Person` is a Dynamic type & Dynamic value would be `p` which contains value of dynamic type Person.

##### 3.3 Dynamic Type & Nil Value:
In this case we are modify the above `Person p` instance to have no value but changing it to pointer. So the method receiver type also changed to receive Person pointer instead. We can see it will error when we call BioData() as the instance doesn't have any value declared. ( refer 3.1 example for complete program and continue with this modification )
```go
func (p *Person) BioData() int {
	return p.age
}

func (p *Person) JobDetails() string {
	return "Engineer"
}

func main() {
	var p *Person // p has Type but no value ( using pointer )
	var bla Personal
	bla = p
	fmt.Println(bla.JobDetails()) // Engineer
	fmt.Println(bla.BioData())    // (Error): runtime error: invalid memory address or nil pointer dereference
}
```
##### 3.4 Interface with Multiple Types:
If we are interested in having the interface which takes different types then we use `interface type` as function `receiver type`. Lets take our 3.1 example where Person type implements Personal Interface, now we will add Robot Type which also implements Personal Interface and at last the Details function designed to take any Type ( Person or Robot ) hence passing `p Personal` interface type as argument.
```go
// STEP 1: Same Interface Personal
type Personal interface {
	BioData() int
	JobDetails() string
}

// STEP 2: Old Person type
type Person struct {
	name string
	age  int
}

// STEP 3: New Type
type Robot struct {
	name    string
	version int
	brand   string
}

// STEP 4: Person Type Implemeting Personal Interface Method 1
func (p Person) BioData() int {
	return p.age
}

// STEP 5: Person Type Implemeting Personal Interface Method 2
func (p Person) JobDetails() string {
	return "Engineer"
}

// STEP 6: Robot Type Implemeting Personal Interface Method 1
func (r Robot) BioData() int {
	return r.version
}

// STEP 7: Robot Type Implemeting Personal Interface Method 1
func (r Robot) JobDetails() string {
	return r.name + " is AI robot"
}

// STEP 8 : Details function takes any type ( Personl / Robot ) which satifies Interface Personal
func Details(p Personal) {
	fmt.Println(".......*.......")
	fmt.Println(p.BioData())
	fmt.Println(p.JobDetails())
}

func main() {

	// STEP 9: Creating instance of Person / Robot type
	p := Person{name: "Bob Marley", age: 43}
	r := Robot{name: "XXO", version: 12, brand: "Boston Dynamcis"}

	// STEP 10: Details function takes any type who pleases the interface Personal
	Details(p)  // takes person type
	Details(r)  // takes robot type 
}
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

4. Example of taking user input ?
We can make use of `Scan` function from `fmt` package. However keep in mind `Scan` will take only store the user entered value directly into the defined address.
```go
package main
import "fmt"
func main(){
	var value string
	_,err := fmt.Scan(&value) // puts the user input string into `value` variable address
	fmt.Println(" user entered ", value) // now value can be printed
}
```











# References:
1. FreecodeCamp 6 hour introduction : https://www.youtube.com/watch?v=YS4e4q9oBaU
2. Installing Go using homebrew : https://sourabhbajaj.com/mac-setup/Go/README.html
