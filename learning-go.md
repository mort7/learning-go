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

```
for initialization; condition; post {
    // zero or more statements
}
```
- *Initialization* statement is optional. Must be simple statment (short variable declaration, increment/decrement, assignment statement, or function call)
- *Condition* is a boolean statment evaluated at the beginnning of each iteration of the loop. If it evaluates to true, the statments controlled by the loop are executed. 
- *Post* statement is executed after the body of the loop, then the condition is evaluated again. 
- The loop ends when the condition becomes false

**Traditional "while" loop**
```
for condition {
    // ...
}
```

**Traditional infinite loop**
```
for {
    // ...
}
```
- Can be terminated by a break or return statement

**Iterate over a range of values from a string or a slice**
```
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