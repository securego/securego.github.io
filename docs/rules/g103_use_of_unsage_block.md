---
id: g103
title: G103: Use of unsafe block
---

# G103: Use of unsafe block

Using the unsafe package in Go gives you low-level memory management and many of the strength of the C language but also gives flexibility to the attacker of your application. The pointer arithmetic is one of the examples from the unsafe package which can be used for data leak, memory corruption or even execution of attackers own script.

Also, you should keep in mind that the "unsafe" package is not protected by [Go 1 compatibility guidelines](https://golang.org/doc/go1compat).

If you want to ignore this rule you can do it, as usual, using the "exclude" option in the command line interface.

## Example code:

```
package main
import (
	"fmt"
	"unsafe"
)
type Fake struct{}
func (Fake) Good() {}
func main() {
	unsafeM := Fake{}
   	unsafeM.Good()
   	intArray := [...]int{1, 2}
   	fmt.Printf("\nintArray: %v\n", intArray)
   	intPtr := &intArray[0]
   	fmt.Printf("\nintPtr=%p, *intPtr=%d.\n", intPtr, *intPtr)
   	addressHolder := uintptr(unsafe.Pointer(intPtr)) + unsafe.Sizeof(intArray[0])
   	intPtr = (*int)(unsafe.Pointer(addressHolder))
   	fmt.Printf("\nintPtr=%p, *intPtr=%d.\n\n", intPtr, *intPtr)
}
```

## Gosec command line output

```
[examples/main.go:18] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(intPtr)

[/Users/mvrachev/Martins/go/src/github.com/securego/examples/main.go:18] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Sizeof(intArray[0])

[examples/main.go:19] - G103: Use of unsafe calls should be audited (Confidence: HIGH, Severity: LOW)
  > unsafe.Pointer(addressHolder)
```

## See also:

* https://golang.org/pkg/unsafe/

