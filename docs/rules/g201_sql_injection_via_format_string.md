---
id: g201
title: G201: SQL query construction using format stringg
---

SQL injection is one of the top security issues develops made and the consequences of this can be enormous. 
Using the format string function in the fmt Golang package to dynamically create an SQL query can easily create a possibility for SQL injection. The reason is that the format string function doesn't escape special characters like ' and it's easy to add second SQL command in the format string.

## Example problematic code:

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

## Gosec command line output

```
[examples/main.go:14] - G201: SQL string formatting (Confidence: HIGH, Severity: MEDIUM)
  > fmt.Sprintf("SELECT * FROM foo where name = '%s'", os.Args[1])
```

## The right way

Using a static SQL query is always preferred. 

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

It is highly recommended to use the database/sql package in Golang instead of fmt package for SQL queries. 

## See also

* https://www.calhoun.io/what-is-sql-injection-and-how-do-i-avoid-it-in-go/
* https://astaxie.gitbooks.io/build-web-application-with-golang/en/09.4.html 
