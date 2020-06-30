# Go Lang

### Installation ( Mac ):

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
















# References:
1. FreecodeCamp 6 hour introduction : https://www.youtube.com/watch?v=YS4e4q9oBaU
2. Installing Go using homebrew : https://sourabhbajaj.com/mac-setup/Go/README.html
