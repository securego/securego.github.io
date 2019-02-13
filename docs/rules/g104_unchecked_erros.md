---
id: g104
title: G104: Audit errors not checked
---

Really useful feature of Golang is the ability to return a tuple of a result and an error value from a function. There is an unspoken rule in Golang that the result of a function is unsafe until you make check the error value. Many security exploits can be performed when the error value is not checked.

## Example code:

```
package main
import "fmt"
func test() (int,error) {
	return 0, nil
}
func main() {
	v, _ := test()
	fmt.Println(v)
}
```

other example:

```
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func a() error {
	return fmt.Errorf("This is an error")
}

func b() {
	fmt.Println("b")
	ioutil.WriteFile("foo.txt", []byte("bar"), os.ModeExclusive)
}

func c() string {
	return fmt.Sprintf("This isn't anything")
}

func main() {
	_ = a()
	a()
	b()
	c()
}
```

## Gosec command line output

The Gosec output from the first example: 

```
[examples/main.go:9] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > v, _ := test()
```

The output from the second example:

```
[examples/main.go:14] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > ioutil.WriteFile("foo.txt", []byte("bar"), os.ModeExclusive)

[examples/main.go:20] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > _ = a()

[examples/main.go:21] - G104: Errors unhandled. (Confidence: HIGH, Severity: LOW)
  > a()
```

## See also:

* https://blog.golang.org/error-handling-and-go
* https://blog.golang.org/errors-are-values