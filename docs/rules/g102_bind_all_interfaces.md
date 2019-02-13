---
id: g102
title: G102: Bind to all interfaces
---

# G102: Bind to all interfaces

Binding to all network interfaces can potentially open up a service to traffic on unintended interfaces, that may not be properly documented or secured. This plugin test looks for a string pattern “0.0.0.0” that may indicate a hardcoded binding to all network interfaces.

## Example code:

```
package main
import (
	"log"
   	"net"
)
func main() {
	l, err := net.Listen("tcp", "0.0.0.0:2000")
	if err != nil {
		log.Fatal(err)
	}
	defer l.Close()
}
```

## Gosec command line output

```
[examples/main.go:9] - G102: Binds to all network interfaces (Confidence: HIGH, Severity: MEDIUM)
  > net.Listen("tcp", "0.0.0.0:2000")
```

## See also

* https://nvd.nist.gov/vuln/detail/CVE-2018-1281 
