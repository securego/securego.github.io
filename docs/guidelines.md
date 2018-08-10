---
id: guidelines 
title: Secure programming guidelines 
---

# Secure programming guidelines


## Introduction

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce accumsan porta mauris tristique vulputate. Etiam ac elit eget orci tincidunt scelerisque ac vitae magna. Suspendisse sit amet augue eget metus mollis pretium a sit amet ligula. Nulla interdum suscipit imperdiet. Phasellus consequat nibh in placerat sollicitudin. Etiam felis sapien, facilisis nec elit eget, vestibulum aliquet tortor. Nulla in nibh faucibus, congue ligula vitae, aliquam augue.

Praesent a lacus et turpis lacinia fringilla sed ac dui. Praesent sed enim lorem. Integer id rutrum leo, et cursus turpis. Pellentesque non tortor imperdiet, consequat ante eget, venenatis lacus. Cras lobortis eros ac risus lacinia euismod ut sed ante. Ut rutrum nisl eget risus malesuada dapibus. Curabitur vel molestie felis. Proin ut velit id nibh vehicula tristique. Quisque auctor aliquam nulla at tempus.

Sed et accumsan augue. Donec sagittis non arcu quis iaculis. Proin quam augue, fermentum quis nibh id, vulputate imperdiet lacus. Duis dictum, lectus eu pulvinar fermentum, sem quam gravida sem, ac viverra diam erat vel augue. Curabitur pretium sodales imperdiet. Morbi sodales ut est vitae rutrum. Duis molestie sit amet nisi non accumsan. Etiam ultricies malesuada orci. Proin dictum non nisi eget sagittis. Nunc sit amet elementum urna, nec euismod diam. Donec ullamcorper cursus nisi, nec tristique metus. Mauris ullamcorper malesuada ultrices. In a fringilla est. Praesent efficitur feugiat ipsum, eget luctus leo pharetra quis.


## General guidence

### G101: Hardcoded credentials
### G102: Network interfaces
### G103: Use of unsafe block
### G104: Unchecked errors
### G105: Pitfalls of math/big.Int.Exp

## Avoiding injection vulnerabilities

### G201: SQL query construction using format string
### G202: SQL query construction using string concatenation
### G203: Use of unescaped data in HTML templates
### G204: Command execution

## Using the file system securely

### G301: Poor file permissions used when creating a directory
### G302: Poor file permisions used with chmod
### G303: Creating tempfile using a predictable path
### G304: File path provided as taint input
### G305: File traversal when extracting zip archive

## Cryptography

### G401 - Cryptography primitives
### G402 - TLS connection settings
### G403 - RSA Key Length 
### G404 - Random number sources

## Web topics 

Look at providing recommendations around common web frameworks for Go (some
may translate into rules).

Some example topics: 
* xss
* xsrf
* ssrf
* session uniqueness / encryption vs signatures
* sessions fixation
* security headers
* unvalidated redirects,
* net/cgi
* jwt pitfalls
* rate limiting
* password strength and lockouts
