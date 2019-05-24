---
id: g201-g202
title: G201/G202: SQL query construction using format string/string concatenation
---

[SQL injection](https://en.wikipedia.org/wiki/SQL_injection) is one of the top security issues developers make and the consequences of this can be severe. 
Using the format string function in the fmt Golang package to dynamically create an SQL query can easily create a possibility for SQL injection. The reason is that the format string function doesn't escape special characters like ' and it's easy to add second SQL command in the format string.

## Examples of problematic code:

### G201 - SQL query construction using format string

```
package main
import (
	"database/sql"
	"fmt"
	"os"
)
func main(){
	db, err := sql.Open("sqlite3", ":memory:")
	if err != nil {
		panic(err)
	}
	q := fmt.Sprintf("SELECT * FROM foo where name = '%s'", os.Args[1])
	rows, err := db.Query(q)
	if err != nil {
		panic(err)
	}
	defer rows.Close()
}
```

The Gosec command line output:

```
[examples/main.go:14] - G201: SQL string formatting (Confidence: HIGH, Severity: MEDIUM)
  > fmt.Sprintf("SELECT * FROM foo where name = '%s'", os.Args[1])
```

### G202 - SQL query construction using string concatenation

```package main

import (
	"database/sql"
)

var staticQuery = "SELECT * FROM foo WHERE age < "

func main() {
	db, err := sql.Open("sqlite3", ":memory:")
	if err != nil {
		panic(err)
	}
	var gender string = "M"
	rows, err := db.Query("SELECT * FROM foo WHERE gender = " + gender)
	if err != nil {
		panic(err)
	}
	defer rows.Close()
}
```

The Gosec command line output:

```
[/Users/mvrachev/Martins/go/src/github.com/securego/securego.github.io/main.go:15] - G202: SQL string concatenation (Confidence: HIGH, Severity: MEDIUM)
  > "SELECT * FROM foo WHERE gender = " + gender
```

## The right way

Two of the ways to escape SQL injection when using Golang are:

1) use static queries

```
package main
import (
		"database/sql"
)
const staticQuery = "SELECT * FROM foo WHERE age < 32"
func main(){
		db, err := sql.Open("sqlite3", ":memory:")
		if err != nil {
			panic(err)
		}
		rows, err := db.Query(staticQuery)
		if err != nil {
			panic(err)
		}
		defer rows.Close()
}
```

2) use the database/sql
By using the database/sql package along with argument placeholders you are able to construct SQL statements that are automatically escaped properly. 
The key distinction here is that you aren’t trying to construct the SQL statement yourself, but instead you are providing arguments that can be easily escaped. The underlying driver for database/sql will ultimately be aware of what special characters it needs to handle and will escape them for you, preventing any nefarious SQL from running.


```
package main
import (
		"database/sql"
		"bufio"

)
func main(){
		db, err := sql.Open("sqlite3", ":memory:")
		if err != nil {
			panic(err)
		}
		in := bufio.NewReader(os.Stdin)
		name, err := in.ReadString('\n')
		if err != nil {
			panic(err)
		}
		rows, err := db.Query("SELECT * FROM foo WHERE name = ?", name)
		if err != nil {
			panic(err)
		}
		defer rows.Close()
}
```

It is highly recommended to use the database/sql package in Golang instead of fmt package for SQL queries. 

## See also

* https://www.calhoun.io/what-is-sql-injection-and-how-do-i-avoid-it-in-go/
* https://astaxie.gitbooks.io/build-web-application-with-golang/en/09.4.html 
