# Understanding Node vs Go

### Return Multiple Values
1. In this example we would like to return two values.

```go
func Person (name string, age int) (string, int) {
  return name, age
}
name, age := Person("bob", 23)
```
in Node
```javascript
const [ name, age ] = Person (name, age ) => {
  return [name, age]
}
```
