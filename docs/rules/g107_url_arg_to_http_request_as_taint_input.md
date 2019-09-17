---
id: g107
title: G107: Url provided to HTTP request as taint input
---

Getting an URL from an untrusted source like user input gives the ability of an attacker to redirect your application to bad websites and perform additional attacks.
One of the examples is as shown below the [http.Get()](https://golang.org/pkg/net/http/#Client.Get) function issues a GET to the specified URL and if the result is appropriate GET will follow the redirect after calling Client's CheckRedirect function. That means that the attacker can send your application to various places.

This problem can be used to achieve [SSRF](https://www.acunetix.com/blog/articles/server-side-request-forgery-vulnerability/) atttacks via http requests with variable url.

## Example problematic code:

```
package main
import (
	"net/http"
	"io/ioutil"
	"fmt"
	"os"
)
func main() {
	url := os.Getenv("tainted_url")
	resp, err := http.Get(url)
	if err != nil {
		panic(err)
	}
  	defer resp.Body.Close()
  	body, err := ioutil.ReadAll(resp.Body)
  	if err != nil {
    		panic(err)
  	}
  	fmt.Printf("%s", body)
}
```

```
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

var url string = "https://www.google.com"

func main() {

	resp, err := http.Get(url)
	if err != nil {
		panic(err)
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%s", body)
}
```

## Gosec command line output

```
[examples/main.go:12] - G107: Potential HTTP request made with variable url (Confidence: MEDIUM, Severity: MEDIUM)
  > http.Get(url)
```

```
[/Users/mvrachev/Martins/go/src/github.com/securego/examples/main.go:17] - G107: Potential HTTP request made with variable url (Confidence: MEDIUM, Severity: MEDIUM)
  > http.Get(url)
```

## The right way

```
package main

import (
	"fmt"
	"net/http"
)

const url = "http://127.0.0.1"

func main() {
	resp, err := http.Get(url)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(resp.Status)
}
```

## See also

* http://projects.webappsec.org/w/page/13246981/URL%20Redirector%20Abuse
* https://www.owasp.org/index.php/Top_10_2010-A10-Unvalidated_Redirects_and_Forwards
