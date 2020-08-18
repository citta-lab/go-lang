1. What is printed when the following program is executed?
```go
func main() {
  s := make([]int, 0, 3)
  s = append(s, 100)
  fmt.Println(len(s), cap(s))
}
```

2. What is printed when the following program is executed?
```go
type P struct {
    x string
y int
}
func main() {
  b := P{"x", -1}
  a := [...]P{P{"a", 10},
        P{"b", 2},
        P{"c", 3}}
  for _, z := range a {
    if z.y > b.y {
      b = z
    }
  }
  fmt.Println(b.x)
}
```
ANS: a

3. What is printed when the following program is executed?
```go
func main() {
  x := map[string]int {
    "ian": 1, "harris": 2}
  for i, j := range x {
    if i == "harris" {
      fmt.Print(i, j)
    }
  }
}
```
ANS: harris2

4. What is printed when the following program is executed?
```go
func main() {
  x := [...]int {1, 2, 3, 4, 5}
  y := x[0:2]
  z := x[1:4]
  fmt.Print(len(y), cap(y), len(z), cap(z))
}
```
ANS: 2534

5. What is printed when the following program is executed?
```go
func main() {
  x := [...]int {4, 8, 5}
  y := x[0:2]
  z := x[1:3]
  y[0] = 1
  z[1] = 3
  fmt.Print(x)
}
```
ANS: [1,8,3]

6. What is printed when the following program is executed?
```go
func main() {
  x := []int {4, 8, 5}
  y := -1
  for _, elt := range x {
    if elt > y {
      y = elt
    }
  }
  fmt.Print(y)
}
```
ANS: 8

7. What is printed when the following program is executed?s
```go
func main() {
  s := make([]int, 0, 3)
  s = append(s, 100)
  fmt.Println(len(s), cap(s))
}
```
ANS: 1, 3

8. Whats the output ?
```go

func fA() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}
func main() {
   fB := fA()
   fmt.Print(fB())
   fmt.Print(fB())
}
```
ANS: 12
REASON: first print will print 1, due to closure i++ will become 2 when second fB() is called in println.

9. How this defer function makes this output ?
```go
func main() {
	i := 1
	fmt.Print(i)
	i++
	defer fmt.Print(i+1)
	fmt.Print(i)
}
```
ANS: 123
REASON: After 1 is printed, defer function will evaluate the i+1 but will not executed it until main function ends so i+1 = 2+1 will be deferred to the end so 123.

10. Is following code is legal ?
```go

func test(x string) int {
	return len(x)
}

func main() {
	f = test
	fmt.Println(f("bob"))
}
```
ANS: YES and it will print 3
