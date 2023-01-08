# Learning Go

> Credits: Large portions of text are from *The Go Programming Language* by Donovan and Kernighan

## Chapter 1

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

A *map* is a reference to the data structure created by make. When a map is passed to a function, 
the function receives a copy of the reference.