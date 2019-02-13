---
id: g101
title: G101: Hardcoded credentials
---

The use of hard-coded passwords increases the possibility of password guessing tremendously. This plugin test looks for all string literals and checks the following conditions:

Variables are considered to look like a password if they have match any one of:
* “password”
* “pass”
* “passwd”
* “pwd”
* “secret”
* “token”


Note: this can be noisy and may generate false positives.

## Example code:

```
package main

import "fmt"

func main() {
	username := "admin"
	var password = "f62e5bcda4fae4f82370da0c6f20697b8f8447ef"

	fmt.Println("Doing something with: ", username, password)
}
```

## Gosec command line output

```
[examples/main.go:7] - G101: Potential hardcoded credentials (Confidence: LOW, Severity: HIGH)
  > password = "f62e5bcda4fae4f82370da0c6f20697b8f8447ef"
```

## See also:

* https://www.owasp.org/index.php/Use_of_hard-coded_password
* http://gotowebsecurity.com/what-is-hardcoded-password-and-how-to-fix-it/ 
