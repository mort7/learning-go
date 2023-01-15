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

Each time we take the address of a variable or copy a pointer, we create new *aliases* or ways 
to identify the same variable. For example, *p is an alias for *v*. Pointer aliasing is useful because it 
allows us to access a variable without using its name, but this is a double-edged sword:
to find all the statements that access a variable, we have to know all of its aliases.

flag package is useful for command line arguments

#### The *new* Function

Another way to create a variable is to use the built-in function *new*. The expression *new*(T) creates an
*unnamed variable* of type T, initializes it to the zero value of T, and returns its address,
which is a value of type *T. A variable created with *new* is no different from an ordinary local
variable whose address is taken, except that there's no need to invent (and declare) a dummy name, and we can use new(T)
in an expression. Thus *new* is only syntactic convenience, not a fundamental notion: the two newInt functions
below have identical behaviors.

```go
func newInt() *int {
	return new(int)
}

func newInt() *int {
	var dummy int
	return &dummy
}
```
Each new call to a *new* returns a distinct variable with a unique address:
```go
p := new(int)
q := new(int)
fmt.Println(p == q)     // "false"
```

There is one exception to this rule: two variables whose type carries no information and is therefore of size
zero, such as struct{} or [0]int, may, depending on the implementation have the same address.

The *new* function is relatively rarely used because the most common unnamed variables
are of struct types, for which the struct literal syntax is more flexible.

Since *new* is a predeclared function, not a keyword, it's possible to redefine the name for something else within a 
function.

```go
func delta(old, new int) int { return new - old }
```

Of course, within *delta*, the built-in *new* function is unavailable.

#### 2.3.4 Lifetime of Variables

The *lifetime* of a variable is the interval of time it exists during program execution.
The lifetime of a package-level variable is the entire program. By contrast, local variables have dynamic lifetimes: 
a new instance is created each time the declaration statement is executed, and the variable lives on until it becomes
*unreachable*, at which point its storage may be recycled. Function parameters and results are 
local variables too; they are created each time their enclosing function is called. 

For example, in this excerpt from the Lissajous program of Section 1.4,

```go
for t := 0.0, t < cycles*2*math.Pi; t += res {
	x := math.Sin(t)
	y := math.Sin(t*freq + phase)
	img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5), 
		blackIndex)
}
```

The variable *t* is created each time the *for* loop beings, and new variables *x* and *y* are created on 
each iteration of the loop.

How does the garbage collector know that a variable's storage can be reclaimed?

The full answer is more detailed than we need here, but the basic idea is that every package-level variable
and every local variable of each currently active function, can potentially be the start or root
of a path to the variable in question (by following pointers and other kinds of references). If no
such path exists, the variable has become unreachable.

A compiler may choose to allocate local variables on the heap or on the stack, but surprisingly, this choice is not 
determined by whether *var* or *new* was used to declare the variable.

```go
var global *int

func f() {
	var x int
	x = 1
	global = &x
}

func g() {
	y := new(int)
	*y = 1
}
```

Here, *x* must be heap-allocated because it is still reachable from the variable *global* after *f* has returned,
despite being declared as a local variable; we say x *escapes from* f. Conversely, when *g* returns, the variable *y 
becomes unreachable and can be recycled. Because *y does not escape, it is safe for the compiler to allocated
*y on the stack, even though it was allocated with *new*. In any case, the notion of escaping is not something that you 
need to worry about in order to write correct code, though it's good to keep in mind during performance optimization, 
since each variable that escapes requires an extra memory allocation.

### 2.4 Assignments

The value held by a variable is updated by an assignment statement, which in its simplest form has a variable
on the left of the = sign and an expression on the right.

```go
x = 1                               // named variable
*p = true                           // indirect variable
person.name = "bob"                 // struct field
count[x] = count[x] * scale         // array or slice or map element
```

Each of the arithmetic and bitwise binary operators has a corresponding *assignment operator* allowing, for example, 
the last statement to be rewritten as

```go
count[x] *= scale
```

which saves us from having to repeat (and re-evaluate) the expression for the variable. Numeric variables can also be 
incremented and decremented by ++ and -- statements:

```go
v := 1
v++       // save as v = v + 1; v becomes 2
v--       // same as v = v - 1; v becomes 1 again
```

#### 2.4.1 Tuple Assignment

*Tuple assignment* allows several variables to be assigned at once.
```go
x, y = y, x
```

```go
func fib(n int) int {
    x, y := 0, 1
	for i := 0; i < n; i++ {
		x, y = y, x+y
	}
	return x
}
```

Tuple assignment can also make a sequence of trivial assignments more compact
```go
i, j, k = 2, 3, 5
```

Certain expressions, such as a call to a function with multiple results, produce several values.
```go
f, err = os.Open("foo.txt")     // function call returns two values
```

A map lookup, type assertion, and channel receive produce two results
```go
v, ok = m[key]          // map lookup
v, ok = x.(T)           // type assertion
v, ok = <- ch           // channel receive
```

#### 2.4.2 Assignability

Assignment statements are an explicit form of assignment, but there are places where things are
*implicitly* assigned:
```go
medals := []string{"gold", "silver", "bronze"}
```

is the same as
```go
medals[0] = "gold"
medals[1] = "silver"
medals[2] = "bronze"
```

In general, each type (left and right) must match. *nil* may be assigned to any variable,
of interface or reference type. Constants are more flexible.

### 2.5 Type Declarations

A type declaration defines a new *named type* that has the same *underlying type* as an existing 
type. The named type provides a way to separate different and perhaps incompatible uses of
the underlying type so they can't be mixed unintentionally.

```go
type name underlying-type
```

Type declarations most often appear at the package level, where the named type is visible
throughout the package, and if the name is exported (it starts with an upper-case letter), 
it's accessible from other packages as well.

### 2.6 Packages and Files
Packages in Go serve the same purposes as libraries or modules in other languages, supporting modularity, encapsulation,
separate compilation, and reuse. The source code for a package resides in one or more .go files, usually in a directory 
whose name ends with the import path.

Each package serves as a separate *name space* for its declarations. Within, the *image* package,
for example, the identifier *Decode* refers to a different function than does the same identifier
in the *unicode/utf16* package. To refer to a function outside its package, we must *qualify* the identifier
to make explicit whether we mean *image.Decode* or *utf16.Decode*

Packages also let us hide information by controlling which names are visible outside the package, or *export*. 

#### 2.6.1 Imports

The last segment of the import path is used as a short name for imported packages.

#### 2.6.2 Package Initialization

Package initialization begins by initializing package-level variables in the order in which they are declared,
except that dependencies are resolved first (if variables require other variables to be initialized).

If the package has multiple .go files, they are initialized in the order in which the files are given to the compiler,
the go tool sorts .go files by name before invoking the compiler.

Each variable declared at package level starts life with the value of its initializer expression, 
if any. But for some variables, like tables of data, an initializer expression may not be the simplest way 
to set its initial value. In that case, the *init* function mechanism may be simpler. Any file many contain any number 
of functions whose declaration is just func init() { /* ... */ } 

Such *init* functions can't be called or referenced, but otherwise they are normal functions. Within each file,
*init* functions are automatically executed when the program starts, in the other in which they are declared.

Packages are initialized bottom-up so *main* is initialized last.

### 2.7 Scope

A declaration associates a name with a program entity, such as a function or a variable. The *scope* of a declaration 
is the part of the source code where a use of the declared name refers to that declaration. The scope of a declaration
is a region of text, it is a compile-time property. The lifetime of a variable is the range of time
during execution when the variable can be referred to by other parts of the program; it is a run-time property.

A *syntactic block* is a sequence of statements enclosed in braces (function or loop), declarations inside are not
visible outside the syntactic block. A *lexical block* are other groupings of declarations that are not explicitly 
surrounded by braces. There is a lexical block for the entire source code, called the universe block, for each package,
for each file, for each `for`, `if`, and `switch`, for each case in a `switch` or `select` statement, and for each
syntactic block. 

Declarations outside a function are at the package level.

You may have multiple declarations of the same name as long as each is in a different lexical block.

> When the compiler encounters a reference to a name, it looks for a declaration, starting with the innermost 
> enclosing lexical block and working up to the universe block. If the compiler finds no declaration, it reports 
> an ‘‘undeclared name’’ error. If a name is declared in both an outer block and an inner block, the inner declaration 
> will be found first. In that case, the inner declaration is said to shadow or hide the outer one, making it 
> inaccessible


```go
func f() {}

var g = "g"

func main() {
	f := "f"
	fmt.Println(f)      // "f"; local var f shadows package-level func f
	fmt.Println(g)      // "g"; package-level var
	fmt.Println(h)      // compile error: undefined: h
}
```

## Chapter 3: Basic Data Types

Go's types fall into four categories: *basic types*, *aggregate types*, *reference types*, and *interface types*.

### 3.1 Integers

Four sizes- 8, 16, 32, 64 bits
Signed and unsigned 

The type *rune* is a synonym for int32 and conventionally indicates that a value is a Unicode code point.

The type *byte* is a synonym for uint8, and emphasizes that the value is a piece of raw data 
rather than a small numeric quantity. 

Finally, there is a type uintptr, whose width is not specified but is sufficient to hold all the bits of a pointer value.
The *uintptr* type is used only for low-level programming, such as at the boundary of a Go program with a C library or 
an operating system. 

int is not that same type as int32, even if the natural size of int is 32 bits

Signed numbers are represented in 2's-complement form

Signed ints range from -2<sup>n-1</sup> through 2<sup>n-1</sup>-1
Unsigned ints range from 0 to 2<sup>n</sup>-1

int8 is -128 to 127 while uint8 is 0 to 255

Go's binary operators for arithmetic, logic, and comparison- listed in order of decreasing
precedence:
```go
* / % << >> & &^ +-|^
== != < <= > >=
&&
||
```

There are only five levels of precedence, operators at the same level associate to the left, so parentheses may be 
required for clarity, or to make operators evaluate in the intended order in an expression like `mask & (1 << 28)`.

Each operator in the first two lines of the table above for instance +, has a corresponding assignment
operator like += that may be used to abbreviate an assignment.

> +, -, *, and / may be applied to integer, floating-point, and complex numbers, but the remainder operator % applies 
> only to integers. In Go, the sign of the remainder is always the same as the sign of the dividend. 

>The behavior of / depends on whether its operands are integers, so 5.0/4.0 is 1.25 but 5/4 is 1 because
> integer division truncates the result toward zero

*Overflow* can happen if the integers are too large to fit into it's resulting type

Binary comparison can be used with ints

Go provides the following bitwise binary operators, the first four treat their operands as bit patterns
with no concept of arithmetic carry or sign:

```go
&       // bitwise AND
|       // bitwise OR
^       // bitwise XOR
&^      // bit clear (AND NOT) << left shift
>>      // right shift
```

### 3.2 Floating-Point Numbers

float32 provides ~6 decimal digits of precision while float64 provides about 15 digits. Use float64 because computations
can accumulate error rapidly unless one is quite careful.

> Very small and large numbers are better written in scientific notation

### 3.3 Complex Numbers

Go provides two sizes of complex numbers, `complex64` and `complex128`, whose complements are
`float32` and `float64` respectfully.

```go
var x complex128 = complex(1, 2)        // 1+2i
var y complex128 = complex(3, 4)        // 3+4i
fmt.Println(x*y)                        // "(-5+10i)"
fmt.Println(real(x*y))                  // "-5"
fmt.Println(imag(x*y))                  // "10"
```

### 3.4 Booleans

type `bool` or *boolean* has only two possible values- `true` and `false`

`!` is the negation operator

`&&` (AND) and `||` (OR) operators have *short circuit* behavior, making it safe to write expressions like:
```go
s != "" && s[0] == 'x'
```

### 3.5 Strings

A string is an immutable sequence of bytes. String may contain arbitrary data, but usually contain human-readable text.
Text strings are conventionally interpreted as UTF-8 encoded sequences of Unicode code points (runes).

The built-in `len` returns the number of bytes (not runes), and the *index* operation returns the 
*i*-th byte of string s

Concatenate using `+`

#### 3.5.1 String Literals

A string value can be written as a *string literal*, a sequence of bytes enclosed in double quotes.

Go source files are always encoded in UTF-8 and Go text strings are conventionally interpreted as UTF-8, 
we can include Unicode code points in string literals.

Within a double-quoted string literal, *escape sequences* that begin with a backslash \ can be used to insert
arbitrary byte values into the string:
```go
\a ‘‘alert’’ or bell
\b backspace
\f form feed \n newline
\r carriage return
\t tab
\v vertical tab
\' single quote (only in the rune literal '\'') 
\" double quote (only within "..." literals) 
\\ backslash
```

Hex values can be written with a *hexidecimal escape* `\xhh` 

A raw string literal is written using back-quotes instead of double quotes. These are helpful for regular expressions.
They are also useful for HTML templates, JSON literals, etc.

#### 3.5.2 Unicode

Unicode version 8 defines code points for over 120,000 characters in over 100 languages.

#### 3.5.3 UFT-8

UTF-8 is a variable-length encoding of Unicode code points as bytes.

#### 3.5.4 Strings and Bytes Slices

Four standard packages are particularly important for manipulating strings:
- `bytes`
  - similar to string but of type `[]byte`
  - Useful to use a byte buffer while building strings and strings are immutable
- `strings`
  - searching, replacing, comparing, trimming, splitting, and joining
- `strconv`
  - converting boolean, integer, and floating-point values to and from their string representations
- `unicode`
  - functions such as `IsDigit`, `IsLetter`, `IsUpper`, and `IsLower` for classifying runes

A string contains an array of bytes that, once created, is *immutable*. Elements of a byte slice can be freely modified

Strings can be converted to byte slices and back again:
```go
s := "abc"
b := []byte(s)
s2 := string(b)
```

### 3.6 Constants

Constants are expressions whose value is known th the compiler and whose evaluation is 
guaranteed to occur at compile time, not at run time. The underlying type of every constant is:
boolean, string, or number.

A `const` declaration defines named values that look like variables but whose value is constant. This prevents
accidental changes during program execution.

Since their values are known to the compiler, constant expressions may appear in types, specifically as the length of 
an array type:

```go

const IPv4Len = 4
// parseIPv4 parses an IPv4 address (d.d.d.d).
func parseIPv4(s string) IP {
    var p [IPv4Len]byte
// ...
}
```

## Chapter 4: Composite Types

Arrays and structs are *aggregate* types; their values are concatenations of other values in memory.
Arrays are homogeneous while structs are heterogeneous. Both arrays and structs are fixed size. In contrast,
slices and maps are dynamic data structures that grow as values are added.

### 4.1 Arrays

An array is a fixed-length sequence of zero or more elements of a particular type. Because of their fixed length, 
arrays are rarely used directly in Go. Slices, which can grow and shrink, are much more versatile, but to understand 
slices we must understand arrays first.

Individual array elements are accessed with the conventional subscript notion, where subscripts run from zero to one 
less than the array length. 

```go
var a [3]int                    // array of 3 integers
fmt.Println(a[0])               // print the first element
fmt.Println(a[len(a)-1])        // print the last element, a[2]

// Print the indices and elements/
for i, v := range a {
	fmt.Printf("%d %d\n", i, v)
}
// Print the elements only.
for _, v := range a {
	fmt.Printf("%d\n", v)
}
```

An *array literal* can initialize an array with a list of values. Uninitialized values with be the zero value
```go
var q [3]int = [3]int{1, 2, 3}
var r [3]int = [3]int{1, 2}
fmt.Println(r[2])                       // "0"
```

In an array literal, if an ellipsis "..." appears in place of the length, the array length is determined by the number 
of initializers. The definition of q can be simplified to:

```go
q := [...]int{1, 2, 3}
fmt.Printf("%T\n", q)           // "[3]int"
```