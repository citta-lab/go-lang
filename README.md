# Go Lang

## Pointers:
- Similar to javascript lexical scoping go uses the concept of `shadowing` to refer to inner scope variables before reaching out to it's immediate outer scope variable.
- By default go uses `float64` when variable is defined with floating number. If we need precise floating pointer then it needs to be declared using `var`. Example: `var price float34`.
- By design if the variables are declared but not used then go will throw an error at compilation ( in javascript we had to use linter to detect ). i.e it is a compile time error.
- Upper case variable name is to allow the variable to be accessed outside of the package.
- Acronyms should always be upper case. Ex: `var theURL string = 'http://github.com'`. i.e URL

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

#### 5. Running Go
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

## Variables :

Three different ways to declare a variables

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

If we are interested in type conversion then in go we would be doing like
```go
var age int = 12
var ageFloat float
ageFloat = float64(age)

// In Java, it would be like
double myDouble = 12.3;
int age = (int) myDouble;
```













# References:
1. FreecodeCamp 6 hour introduction : https://www.youtube.com/watch?v=YS4e4q9oBaU
2. Installing Go using homebrew : https://sourabhbajaj.com/mac-setup/Go/README.html
