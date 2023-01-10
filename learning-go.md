# Learning Go

> Credits: Large portions of text are from *The Go Programming Language* by Donovan and Kernighan

## Chapter 1 - Tutorial

### 1.1 Hello, World
Go is a compiled language. It is organized into packages (similar to libraries or modules in other languages). A **package** consists of one or more .go source files in a single directory that defines what the program does. Each source file contains a package declaration that define what the package does. This is followed by **imports**.

Package **main** is special, it defines a standalone executable program, not a library. Within package main, the function main is also special- it's where execution of the program begin.

#### Running a Go program
```
go run <file_name>.go
```

#### Building a Go program
```
go build <file_name>.go
```

### 1.2 Command-Line Arguments

Use **os.Args** to access command line arguments. It is a slice (dynamically sized sequence *s* of array elements where individual elements can be accessed as s[i] and a contiguous subsequence s[m:n]) of strings. Go uses *half-open* intervals that include the first index but exclude the last.

#### Operators

**+** is the usual arithmetic operator and concatenates string values.

**=** is an assignment statement 

**+=** is an assignment operator

**:=** is a short variable declaration, it declares one or more variables and gives them the appropriate types based on the initializer values.

#### Loop Statements
The *for* loop is the only loop statement in Go.

```go
for initialization; condition; post {
    // zero or more statements
}
```
- *Initialization* statement is optional. Must be simple statment (short variable declaration, increment/decrement, assignment statement, or function call)
- *Condition* is a boolean statment evaluated at the beginnning of each iteration of the loop. If it evaluates to true, the statments controlled by the loop are executed. 
- *Post* statement is executed after the body of the loop, then the condition is evaluated again. 
- The loop ends when the condition becomes false

**Traditional "while" loop**
```go
for condition {
    // ...
}
```

**Traditional infinite loop**
```go
for {
    // ...
}
```
- Can be terminated by a break or return statement

**Iterate over a range of values from a string or a slice**
```go
for _, arg := range os.Args[1:] {
    // ...
}
```

**Blank Identifier (_)**

**Join function**
- Faster than concatenating strings

### 1.3 Finding Duplicate Lines

**Map** \
A *map* holds a set of key/value pairs and provides constant-time operations to store, retrieve, or test for an item in 
the set. The key many be of any type whose values can be compared with ==, the value can be anytype.

**bufio** \
Makes input and output efficient and convenient. Scanner is a useful feature that reads input and breaks it into lines or 
worlds.

**Printf** 
```
%d              # decimal integer
%x, %o, %b      # integer in hex, octal, binary
%f, %g, %e      # floating-point number: 3.141593 3.141592653589793 3.141593e+00
%t              # boolean: true or false
%c              # rune (Unicode code point)
%s              # string
%q              # quoated string "abc" or rune 'c'
%v              # any value in a natural format
%T              # type of any value
%%              # literal percent sign (no operand)  
```
- tab \t
- newline \n
- Printf does not write newline by default

#### File opening
**os.Open** returns two values:
1. An open file (*os.File) that is used in subsequent reads by the Scanner
2. Value of the built-in *error* type. If err equals the special built-in value *nil*, the file was opened successfully.

The file is read and when the end of the input is reached, use *Close* to close the file and releases any resources.
If *err* is not nil, something when wrong and the error value describes the problem.

> Functions and other package-level entities may be declared in any order

#### Readfile (from the io/ioutil package)
- Reads named files (not the standard input)

### 1.4 Animated GIFs
Mainly introduces Go functionality

### 1.5 Fetching a URL
- Use *net/http* and *io* packages

*http.Get* makes an HTTP requests and, if there is not an error, returns the result in the response
struct *resp*. The *Body* field of *resp* contains the server response as a readable stream. 
*io.ReadAll* reads the entire response and the result is stored in *b*. The *Body* stream is closed to avoid
leaking resources.

### 1.6 Fetching URLs Concurrently

A *goroutine* allows for concurrent function execution. A *channel* is a communication mechanism that allows
one goroutine to pass values of a specified type to another goroutine. The function main runs in a goroutine and
the *go* statement creates additional goroutines. When one goroutine attempts a send or receive on a channel, it 
blocks until another goroutine attempts the corresponding send or receive operation, at which point the value is 
transferred and both goroutines proceed. 

### 1.7 A Web Server

Create handler functions for request that match paths. Listens on localhost:8000

### 1.8 Loose Ends

*Control Flow*: Switch statement does not fall through like other C-like languages (there
is a fallthrough statement if needed)

*Named types*: A type declaration makes it possible to give a name to an existing type

*Pointers*: Go provides pointers- values that contain the address of a variable
Pointers are explicitly visible. The *&* operator yields the address of a variable and the * operator retrieves 
the variable 

*Methods and interfaces*: A method is a function associated with a named type. Interfaces are 
abstract types that let us treat different concrete types in the same way
based on what methods they have, not how they are represented or implemented.

*Packages*: There are many packages released that are extremely useful.

*Comments*: 
/* ... */ for multiple line comments
// for everything else

## Chapter 2: Program Structure

### 2.1 Names

The names of Go functions, variables, types, statement labels and packages follow
a simple rule: a name begins with a letter or an underscore and may have any number 
of additional letters, digits, and underscores. Case matters: heapSort and Heapsort are different names

Go *keywords*
- break
- case
- chan
- const
- continue
- default
- defer
- else
- fallthrough
- for
- func
- go
- goto
- if
- import
- interface
- map
- package
- range
- return
- select
- struct
- switch
- type
- var

*Constants*
- true
- false
- iota
- nil

*Types*
- int, int8, int16, int32, int64
- uint, uint8, uint16, uint32, uint64, uintptr
- float32, float64, complex64, complex128
- bool
- byte
- rune
- string
- error

*Functions*
- make
- len
- cap
- new
- append
- copy
- close
- delete
- complex
- real
- imag
- panic
- recover

> If an entity is declared within a function, it is *local* to that function. If it 
> is declared outside the function, it is visible to all files of the package 
> that it belongs to

>If the name begins with a **capital** letter, it is 
> exported and visible/accessible outside of its own package. Package names
> themselves are always lower case

### 2.2 Declarations

A *declaration* names a program entity and specifies some or all of its properties. 
There are four major kinds of declarations: *var*, *const*, *type*, and *func*

A *Go* program is stored in one or more file whose names end in .go. Each file begins with a package declaration that 
says what package the file is a part of. The *package* declaration is followed by any import declarations, then a sequence of
*package-level* declarations of types, variables, constants, and functions in any order.

A function declaration has a name, a list of parameters (the variables are provided by the function's callers), 
an optional list of results, and the function body, which contains the statements that define what the function does. 
The results list is omitted if the function does not return anything. 

### 2.3 Variables

A *var* declaration creates a variable of a particular type, attaches a name to it, and sets its initial value.
Each declaration has the general form
```go
var name type = expression
```

Either the type or the *= expression* part may be omitted but not both. If the type is omitted, it is determined by the 
initializer expression. If the expression is omitted, the initial value is the *zero value* for the type- 0 for numbers,
false for booleans, "" for strings, and nil for interfaces and reference types (slice, pointer, map, channel, function).
The zero value of an aggregate type like an array or struct has zero value of all its elements or fields.

> The zero-value mechanism ensures that a variable always holds a well-defined value of its type; in Go there is no
> such thing as an uninitialized variable. 

```go
var s string
fmt.Println(s)      // Prints ""
```

```go
var i, j, k int                     // int, int, int
var b, f, s = true, 2.3, "four"     // bool, float64, string
```

> Initializers may be literal values or arbitrary expressions. Package-level variables are initialized before *main* 
> begins and local variables are initialized as their declarations are encountered before function execution.

A set of variables can also be initialized by calling a function that returns multiple values:
```go
var f, err = os.Open(name)          // os.Open returns a file and an error
```

#### 2.3.1 Short Variable Declarations

> Within a function, an alternate form called a *short variable declaration* may be used to declare and initialize
> local variables. It takes the form *name := expression*, and the type of *name* is determined by the expression.

Because of their brevity and flexibility, short variable declarations are used to declare and initialize the majority of
local variables. A *var* declaration tends to be reserved for local variables that need an explicit type that differs
from that of the initializer expression, or for when the variable will be assigned a value later and its initial value
is unimportant.

```go
i := 100                        // an int
var boiling float64 = 100        // a float64

var names []string
var err error
var p Point
```

As with *var* declarations, multiple variables may be declared and initialized in the same short variable declaration:
```go
i, j := 0, 1
```

> Keep in mind that := is a declaration, whereas = is an assignment

Like ordinary *var* declarations, short variable declarations may be used for calls to functions like
os.Open that return two or more values.=

```go
f, err := os.Open(name)
if err != nil {
	return err
}
// ...use f
f.Close()
```

> One subtle but important point: a short variable declaration does not necessarily 
> *declare* all the variables on its left-hand side. If some were already declared in the 
> *same* lexical block, then the short variable declaration acts like an *assignment* to those
> variables

In the code below, the first statement declares both *in* and *err*. The second declares *out* but only assigns a value
to the existing *err* variable
```go
in, err := os.Open(infile)
// ...
out, err := os.Create(outfile)
```

A short variable declaration must declare at least one new variable, so this code
will not compile:
```go
f, err := os.Open(infile)
f, err := os.Create(outfile)  // compile error: no new variables
```

The fix is to use an ordinary assignment for the second statement.
```go
f, err := os.Open(infile)
f, err = os.Create(outfile)  // This works
```

#### 2.3.2 Pointers

A *variable* is a piece of storage containing a value. Variables created by declarations are identified by a name,
such as *x*.

A *pointer* value is the address of a variable. A pointer is the location at which a value is stored. With a pointer, 
we can read or update the value of a variable *indirectly*, without using or knowing the name of the variable.

If a variable is declared var x int, the expression &x ("address of x") yields a pointer to an integer variable, that 
is, a value of type *int, which is "pointer to int". If this value is called p, we say "p points to x". The variable to 
which p points is written *p. The expression *p yields the value of that variable, an int, but since *p
denotes a variable, it may also appear on the left-hand side of an assignment, in which case the assignment
updates the variable.

```go
x := 1
p := &x                     // p, of type *int, points to x
fmt.Println(*p)             // "1"
*p = 2                      // equivalent to x = 2
fmt.Println(x)              // "2"
```

Each component of a variable of aggregate type- a field of a struct or element of an array- is also a
variable and thus has an address too. Variables are sometimes described as *addressable* values. Expressions that 
denote variables are the only expressions to which the *address-of* operate & may be applied. 

> The zero value for a pointer of any type is *nil*. The test p != nil is true if *p* points to a variable. 
> Pointers are comparable, two pointers are equal if and only if they point to the same variable or both
> are nil.

```go
var x, y int
fmt.Println(&x == &x, &x == &y, &x == nil)      // "true false false"
```

x and y here are initialized to their zero value here. Remember that Go does not have uninitialized variables.
So x and y both have values, hence why &x != &y. If they both were nil, they would be equal but are not in this case.

It is perfectly safe for a function to return the address of a local variable. For instance, in the code below, the 
local variable *v* created by this particular call to *f* will remain in existence even after the call has returned,
and the pointer *p* will still refer to it:

```go
var p = f()
func f() *int {
	v := 1
	return &v
}
```

Each call of f returns a distinct value:
```go
fmt.Println(f() == f())     // "false"
```

Because a pointer contains the address of a variable, passing a pointer argument to a function
makes it possible for the function to update the variable that was indirectly passed.

```go
func incr(p *int) int {
	*p++                // increments what p points to; does not change p
	return *p
}
v := 1
incr(&v)                // side effect: v is now 2
fmt.Println(incr(&v))   // "3" (and v is 3)
```