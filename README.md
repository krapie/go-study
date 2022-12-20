# Go Study

Go study with [Go offical documentation](https://go.dev/doc/)

# Getting started

### [Tutorial: Get started with Go](https://go.dev/doc/tutorial/getting-started.html)

> A brief Hello, World tutorial to get started. Learn a bit about Go code, tools, packages, and modules.
> 
- `go mod init` : Enable dependency tracking
    - **Dependency Tracking**: When your code imports packages contained in other modules, you manage those dependencies through your code's own module. That module is defined by a go.mod file that tracks the modules that provide those packages. That go.mod file stays with your code, including in your source code repository.
- `go run .` : Run go code
- `go mod tidy` : Locate dependencies and download imported package and check sums + synchronize and add module
    - [pkg.go.dev](http://pkg.go.dev) site to find published modules whose packages have functions you can use in your own code

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) [Create a module](https://go.dev/doc/tutorial/create-module.html)

> A tutorial of short topics introducing functions, error handling, arrays, maps, unit testing, and compiling.
> 
- **Modules**
    - You collect one or more related packages for a discrete and useful set of functions.
    - Go code is grouped into packages, and packages are grouped into modules. Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.
    - If you publish a module, this *must* be a path from which your module can be downloaded by Go tools. That would be your code's repository.
- **Function**
    
    ![function-syntax.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fb591da-8c69-4316-b0e5-51431181d3cc/function-syntax.png)
    
    - In Go, a function whose name starts with a capital letter can be called by a function not in the same package. This is known in Go as an exported name.
- `:= Operator`
    
    ```go
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    ```
    
    - shortcut for declaring and initializing a variable in one line (Go uses the value on the right to determine the variable's type).
    - Long verison:
        
        ```go
        var message string
        message = fmt.Sprintf("Hi, %v. Welcome!", name)
        ```
        

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) [Call your code from another module](https://go.dev/doc/tutorial/call-module-code.html)

- `main` package: In Go, code executed as an application must be in a main package.
- `go mod edit -replace` : replace moudle location (ex: replace module location to internal file system)
    
    ```go
    $ go mod edit -replace example.com/greetings=../greetings
    ```
    
- `require` in go.mod: To reference a *published* module, a go.mod file would typically omit the `replace` directive and use a `require` directive with a tagged version number at the end.
    
    ```go
    require example.com/greetings v1.1.0
    ```
    

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) **[Return and handle an error](https://go.dev/doc/tutorial/handle-errors.html)**

- Common error handling in Go: Return an error as a value so the caller can check for it.
- `[errors.New](https://pkg.go.dev/errors/#example-New)()` : Import the Go standard library `errors` package to use `errors.New()` function
- `nil` : meaning no error. add in the successful return. That way, the caller can see that the function succeeded.
- `log.SetPrefix()` : Import the `[log` package](https://pkg.go.dev/log/) to print prefix at the start of its log messages, without a time stamp or source file information.

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) **[Return a random greeting](https://go.dev/doc/tutorial/random-greeting.html)**

- slice: A slice is like an array, except that its size changes dynamically as you add and remove items. The slice is one of Go's most useful types.
- `randomFormat()` : Starts with a lowercase letter, making it accessible only to code in its own package (in other words, it's not exported).
- `[]string` : Declaring a slice, you omit its size in the brackets. This tells Go that the size of the array underlying the slice can be dynamically changed.
- `math/rand` : • Use the [math/rand package](https://pkg.go.dev/math/rand/) to generate a random number for selecting an item from the slice.
- `init()` : Go executes `init` functions automatically at program startup, after global variables have been initialized.

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) **[Return greetings for multiple people](https://go.dev/doc/tutorial/greetings-multiple-people.html)**

- Backward compatibility: Changing the function's parameter from a single name to a set of names would change the function's signature. If you had already published the module and users had already written code calling that function, that change would break their programs. → In this situation, a better choice is to write a new function with a different name. The new function will take multiple parameters. That preserves the old function for backward compatibility.
- Calling existing function in overloaded function helps reduce duplication while also leaving both functions in place.
- `make(map[*key-type*]*value-type*)` : Initialize a map
- **`for _, name := range names { }`** : If you don't need the index, you use the Go blank identifier (an underscore) to ignore it.

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) **[Add a test](https://go.dev/doc/tutorial/add-a-test.html)**

- Implement test functions in the same package as the code you're testing.
- `Test{*Name}` :* Test function names have the form `Test*Name*`, where *Name* says something about the specific test.
- `t *testing.T` : Test functions take a pointer to the `testing`
 package's [testing.T type](https://pkg.go.dev/testing/#T) as a parameter. You use this parameter's methods for reporting and logging from your test.
- `t.FatalF()` : Use the `t` parameter's [Fatalf method](https://pkg.go.dev/testing/#T.Fatalf) to print a message to the console and end execution.
- `go test` : The `go test` command executes test functions (whose names begin with `Test`) in test files (whose names end with _test.go).
    - The output will include results for only the tests that failed, which can be useful when you have a lot of tests.
    - You can add the `-v` flag to get verbose output that lists all of the tests and their results.

### [Tutorial:](https://go.dev/doc/tutorial/getting-started.html) **[Compile and install the application](https://go.dev/doc/tutorial/compile-install.html)**

- `go build` : The [go build command](https://go.dev/cmd/go/#hdr-Compile_packages_and_dependencies) compiles the packages into an executable, along with their dependencies, but it doesn't install the results.
- `go install` : The [go install command](https://go.dev/ref/mod#go-install) compiles and installs the packages.

### **[Tutorial: Getting started with multi-module workspaces](https://go.dev/doc/tutorial/workspaces.html)**

> Introduces the basics of creating and using multi-module workspaces in Go. Multi-module workspaces are useful for making changes across multiple modules.
> 
- `go work init ./hello` : **Initialize the workspace:** The `go work init` command tells `go` to create a `go.work` file for a workspace containing the modules in the `./hello` directory.
    - The `go` directive tells Go which version of Go the file should be interpreted with. It’s similar to the `go` directive in the `go.mod` file.
    - The `use` directive tells Go that the module in the `hello` directory should be main modules when doing a build.
- `go work use ./example` : The `go work use` command adds a new module to the go.work file.
    - The Go command resolves the `golang.org/x/example` import using the `go.work` file.
- `go.work` can be used instead of adding [replace](https://go.dev/ref/mod#go-mod-file-replace) directives to work across multiple modules.
- `go get golang.org/x/example@v0.1.0` : Releasing these modules is done by tagging a commit on the module’s version control repository. That way, the `go` command can properly resolve the modules outside the workspace.
- The `go` command has a couple of subcommands
    - `go work use [-r] [dir]` adds a `use` directive to the `go.work` file for `dir`, if it exists, and removes the `use` directory if the argument directory doesn’t exist. The `r` flag examines subdirectories of `dir` recursively.
    - `go work edit` edits the `go.work` file similarly to `go mod edit`
    - `go work sync` syncs dependencies from the workspace’s build list into each of the workspace modules.

### **[Tutorial: Developing a RESTful API with Go and Gin](https://go.dev/doc/tutorial/web-service-gin.html)**

> Introduces the basics of writing a RESTful web service API with Go and the Gin Web Framework.
> 
- Basics of writing a RESTful web service API with Go and the [Gin Web Framework](https://gin-gonic.com/docs/) (Gin)
- Design API endpoints → Prepare Data/Code → Write a handler for response
- **Design API endpoints**
    - /albums
        - `GET` – Get a list of all albums, returned as JSON.
        - `POST` – Add a new album from request data sent as JSON.
    - /albums/:id
        - `GET` – Get an album by its ID, returning the album data as JSON.
- **Prepare Data/Code**
    - To keep things simple for the tutorial, you’ll store data in memory. A more typical API would interact with a database.
- **Write a handler to return all items**
    - When the client makes a request at `GET /albums`, you want to return all the albums as JSON.
    - To do this, you’ll write the following: Logic to prepare a response → Code to map the request path to your logic
    
    ```go
    // getAlbums responds with the list of all albums as JSON.
    func getAlbums(c *gin.Context) {
        c.IndentedJSON(http.StatusOK, albums)
    }
    ```
    
    - `gin.Context` : Most important part of Gin. It carries request details, validates and serializes JSON, and more. Function takes a [gin.Context](https://pkg.go.dev/github.com/gin-gonic/gin#Context) parameter.
    - `Context.IndentedJSON` : Serialize the struct into JSON and add it to the response.
        - The function’s first argument is the HTTP status code and second argument is JSON. Here, you’re passing the [StatusOK](https://pkg.go.dev/net/http#StatusOK) constant from the `net/http` package to indicate `200 OK`.
    
    ```go
    func main() {
        router := gin.Default()
    		// Assign the handler function to an endpoint path.
        router.GET("/albums", getAlbums)
    
        router.Run("localhost:8080")
    }
    ```
    
    - `router.Default` : Initialize a Gin router.
    - `router.GET` : Use the function to associate the `GET` HTTP method and `/albums` path with a handler function.
        - Note that you’re passing the *name* of the `getAlbums` function, not the result of the function `getAlbums()`
    - `router.Run` : Use the function to attach the router to an `http.Server` and start the server.
    - `go get .` : Get dependencies for code in the current directory
    - `go run .` : Run code in the current directory.
- **Write a handler to add a new item**
    - When the client makes a `POST` request at `/albums`, you want to add the album described in the request body to the existing albums’ data.
    - To do this, you’ll write the following: Logic to add the new album to the existing list. → A bit of code to route the `POST` request to your logic.
    
    ```go
    // postAlbums adds an album from JSON received in the request body.
    func postAlbums(c *gin.Context) {
        var newAlbum album
    
        // Call BindJSON to bind the received JSON to
        // newAlbum.
        if err := c.BindJSON(&newAlbum); err != nil {
            return
        }
    
        // Add the new album to the slice.
        albums = append(albums, newAlbum)
        c.IndentedJSON(http.StatusCreated, newAlbum)
    }
    ```
    
    - `Context.BindJSON` : Bind the request body to `newAlbum`.
    - Append the `album` struct initialized from the JSON to the `albums` slice.
    - Add a `201` status code to the response, along with JSON representing the album you added.
    
    ```go
    // change your main function so that it includes the router.POST function
    router.POST("/albums", postAlbums)
    ```
    
    - `router.POST` : Associate method at the `/albums` path with the `postAlbums` function.
        - With Gin, you can associate a handler with an HTTP method-and-path combination. In this way, you can separately route requests sent to a single path based on the method the client is using
- **Write a handler to return a specific item**
    - When the client makes a request to `GET /albums/[id]`, you want to return the album whose ID matches the `id` path parameter.
    - To do this, you will: Add logic to retrieve the requested album. → Map the path to the logic.
    
    ```go
    // getAlbumByID locates the album whose ID value matches the id
    // parameter sent by the client, then returns that album as a response.
    func getAlbumByID(c *gin.Context) {
        id := c.Param("id")
    
        // Loop over the list of albums, looking for
        // an album whose ID value matches the parameter.
        for _, a := range albums {
            if a.ID == id {
                c.IndentedJSON(http.StatusOK, a)
                return
            }
        }
        c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
    }
    ```
    
    - `Context.Param` : Retrieve the `id` path parameter from the URL. When you map this handler to a path, you’ll include a placeholder for the parameter in the path.
    - Loop over the `album` structs in the slice, looking for one whose `ID` field value matches the `id` parameter value. If it’s found, you serialize that `album` struct to JSON and return it as a response with a `200 OK` HTTP code.
    - Return an HTTP `404` error with [http.StatusNotFound](https://pkg.go.dev/net/http#StatusNotFound) if the album isn’t found.
    
    ```go
    // change your main function so that it includes the router.GET function
    router.GET("/albums/:id", getAlbumByID)
    ```
    
    - `/albums/:id` : Associate the path with the `getAlbumByID` function.
        - In Gin, the colon preceding an item in the path signifies that the item is a path parameter.

### **[Tutorial: Getting started with generics](https://go.dev/doc/tutorial/generics.html)**

> With generics, you can declare and use functions or types that are written to work with any of a set of types provided by calling code.
> 
- **Add a generic function to handle multiple types**
    
    ```go
    func SumIntsOrFloats[K comparable, V int64 | float64](m map[K]V) V {}
    ```
    
    - `[K comparable, V int64 | float64]`: Type parameters make the function generic, enabling it to work with arguments of different types
    
    ```go
    SumIntsOrFloats[string, int64](ints)
    SumIntsOrFloats[string, float64](floats)
    ```
    
    - Specify type arguments – the type names in square brackets
- **Remove type arguments when calling the generic function**
    
    ```go
    SumIntsOrFloats(ints)
    SumIntsOrFloats(floats)
    ```
    
    - You can omit type arguments in calling code when the Go compiler can infer the types you want to use. The compiler infers type arguments from the types of function arguments.
- **Declare a type constraint**
    
    ```go
    type Number interface {
        int64 | float64
    }
    ```
    
    - Move the union from the function declaration into a new type constraint.
        - That way, when you want to constrain a type parameter to either `int64` or `float64`, you can use this `Number` type constraint instead of writing out `int64 | float64`.
    
    ```go
    func SumNumbers[K comparable, V Number](m map[K]V) V {}
    ```
    
    - Declare with new interface type instead of the union as the type constraint.

### **[Tutorial: Getting started with fuzzing](https://go.dev/doc/tutorial/fuzz.html)**

> Fuzzing can generate inputs to your tests that can catch edge cases and security issues that you may have missed.
> 
- **Add a fuzz test**
    - With fuzzing, random data is run against your test in an attempt to find vulnerabilities or crash-causing inputs.
    
    ```go
    func FuzzReverse(f *testing.F) {
    	tc := []string{}
    	for _, tc := range testcases {
    		f.Add(tc) // Use f.Add to provide a seed corpus
    	}
    	f.Fuzz(func(t *testing.T, orig string) {
    		// add test
    	})
    }
    ```
    
    - The function begins with `FuzzXxx` instead of `TestXxx`, and takes `*testing.F` instead of `*testing.T`
    - `f.Fuzz` : takes a fuzz target function whose parameters are `*testing.T` and the types to be fuzzed.
    - `f.Add` :  fuzz test input as seed corpus inputs
    - `go test -fuzz=Fuzz` : run fuzz test with fuzzing
        - The fuzz test will run until it encounters a failing input unless you pass the `-fuzztime` flag. The default is to run forever if no failures occur, and the process can be interrupted with `ctrl-C`.
        - `go test` : after fuzz test, the new failing seed corpus entry will be used
- **Fix the invalid error**
    - `t.Logf` : log useful debugging info to your terminal. It print to the command line if an error occurs, or if executing the test with `-v`, which can help you debug this particular issue.
    - `go test -run={FuzzTestName}/{filename}` : To run a specific corpus entry within fuzzXXX/testdata, helpful when debugging.
    - `go test -fuzz=Fuzz -fuzztime 30s` : fuzz for 30 seconds before exiting if no failure was found. If no failure, it returns ok.


---

## [Tour of Go](https://go.dev/tour/) Summary

## 1. Packages, variables, and functions

### Packages

```go
**package main**
```

- Every Go program is made up of packages.
- Programs start running in package `main`.
- By convention, the package name is the same as the last element of the import path.
  - ex) `"math/rand"` package comprises files that begin with the statement `package rand`.

### Imports

```go
import (
	"fmt"
	"math"
)
```

- It is good style to use the factored import statement

### Exported names

```go
fmt.Println(math.Pi)
```

- A name is exported if it begins with a capital letter
  - ex) `Pizza` is an exported name, as is `Pi`, which is exported from the `math` package.

### Functions

```go
func add(x int, y int) int {
	return x + y
}
```

- A function can take zero or more arguments.
- Notice that the type comes *after* the variable name.
- When two or more consecutive named function parameters share a type, you can omit the type from all but the last.
  - ex) `x int, y int` → `x, y int`

### Multiple results

```go
func swap(x, y string) (string, string) {
	return y, x
}

a, b := swap("hello", "world")
```

- A function can return any number of results.

### **Named return values**

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```

- Go's return values may be named. If so, they are treated as variables defined at the top of the function.
- These names should be used to document the meaning of the return values.
- A `return` statement without arguments returns the named return values. This is known as a "_naked_" return.
  - Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

### Variables

```go
var c, python, java bool
```

- The `var` statement declares a list of variables; as in function argument lists, the type is last.
- A `var` statement can be at package or function level.

### **Variables with initializers**

```go
var i, j int = 1, 2
var c, python, java = true, false, "no!"
```

- A var declaration can include initializers, one per variable.
- If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

### **Short variable declarations**

```go
k := 3
```

- Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.
- Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is not available.

### **Basic types**

```go
var (
	ToBe   bool       = false
	MaxInt uint64     = 1<<64 - 1
	z      complex128 = cmplx.Sqrt(-5 + 12i)
)
```

- Variable declarations may be "factored" into blocks, as with import statements.
- The `int`, `uint`, and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems. When you need an integer value you should use `int` unless you have a specific reason to use a sized or unsigned integer type.
- Go basic types

  ```go
  bool

  string

  int  int8  int16  int32  int64
  uint uint8 uint16 uint32 uint64 uintptr

  byte // alias for uint8

  rune // alias for int32
       // represents a Unicode code point

  float32 float64

  complex64 complex128
  ```

### **Zero values**

```go
var i int // 0
var f float64 // 0
var b bool // false
var s string // ""
```

- Variables declared without an explicit initial value are given their *zero value*.
  - The zero value is:
    - `0` for numeric types,
    - `false` for the boolean type, and
    - `""` (the empty string) for strings.

### **Type conversions**

```go
var i int = 42 // i := 42
var f float64 = float64(i) // f := float64(i)
var u uint = uint(f) // u := uint(f)
```

- The expression `T(v)` converts the value `v` to the type `T`.
- Unlike in C, in Go assignment between items of different type requires an explicit conversion.

### **Type inference**

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

- When declaring a variable without specifying an explicit type (either by using the `:=` syntax or `var =` expression syntax), the variable's type is inferred from the value on the right hand side.
- But when the right hand side contains an untyped numeric constant, the new variable may be an `int`, `float64`, or `complex128` depending on the precision of the constant

### **Constants**

```go
const Pi = 3.14
```

- Constants are declared like variables, but with the `const` keyword.
- Constants can be character, string, boolean, or numeric values.
- Constants cannot be declared using the `:=` syntax.

### **Numeric Constants**

```go
const (
	// Create a huge number by shifting a 1 bit left 100 places.
	// In other words, the binary number that is 1 followed by 100 zeroes.
	Big = 1 << 100
	// Shift it right again 99 places, so we end up with 1<<1, or 2.
	Small = Big >> 99
)
```

- Numeric constants are high-precision *values*.
- An untyped constant takes the type needed by its context.

## 2. Flow control statements: for, if, else, switch and defer

### For

```go
for i := 0; i < 10; i++ {
		sum += i
}
```

- Go has only one looping construct, the `for` loop.
- Go's for is like the one in C, C++, Java, JavaScript, except that there are no parentheses surrounding the three components of the `for` statement and the braces `{ }` are always required.
- The init statement will often be a short variable declaration, and the variables declared there are visible only in the scope of the `for` statement.
- Unlike other languages like C, Java, or JavaScript The init and post statements are optional.

### **For is Go's "while"**

```go
for sum < 1000 {
		sum += sum
}
```

- At that point you can drop the semicolons: C's `while` is spelled `for` in Go.

### **Forever**

```go
for {
}
```

- If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

### If

```go
if x < 0 {
		return sqrt(-x) + "i"
}
```

- Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.

### **If with a short statement**

```go
if v := math.Pow(x, n); v < lim {
		return v
}
```

- Like `for`, the `if` statement can start with a short statement to execute before the condition.
- Variables declared by the statement are only in scope until the end of the `if`.

### **If and else**

```go
if v := math.Pow(x, n); v < lim {
		return v
} else {
		fmt.Printf("%g >= %g\n", v, lim)
}
```

- Variables declared inside an `if` short statement are also available inside any of the `else` blocks.

### **Exercise: Loops and Functions**

As a way to play with functions and loops, let's implement a square root function: given a number x, we want to find the number z for which z² is most nearly x.

Computers typically compute the square root of x using a loop. Starting with some guess z, we can adjust z based on how close z² is to x, producing a better guess:

```go
z -= (z*z - x) / (2*z)
```

Repeating this adjustment makes the guess better and better until we reach an answer that is as close to the actual square root as can be.

Implement this in the `func Sqrt` provided. A decent starting guess for z is 1, no matter what the input. To begin with, repeat the calculation 10 times and print each z along the way. See how close you get to the answer for various values of x (1, 2, 3, ...) and how quickly the guess improves.

Hint: To declare and initialize a floating point value, give it floating point syntax or use a conversion:

```go
z := 1.0
z := float64(1)
```

Next, change the loop condition to stop once the value has stopped changing (or only changes by a very small amount). See if that's more or fewer than 10 iterations. Try other initial guesses for z, like x, or x/2. How close are your function's results to the [math.Sqrt](https://go.dev/pkg/math/#Sqrt) in the standard library?

**Answer Code**

```go
package main

import (
	"fmt"
	"math"
)

func Sqrt(x float64) float64 {
	z, zz := 1.0, 0.0

	for math.Abs(z-zz) > 1e-8 {
		z, zz = z - (z*z - x) / (2*z), z
	}

	return z
}

func main() {
	n := 2.
	fmt.Println(Sqrt(n))
	fmt.Println(Sqrt(n) == math.Sqrt(n))
}
```

### **Switch**

```go
switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
}
```

- A `switch` statement is a shorter way to write a sequence of `if - else` statements. It runs the first case whose value is equal to the condition expression.
- Go's switch is like the one in C, C++, Java, JavaScript, and PHP, except that Go only runs the selected case, not all the cases that follow.
- In effect, the `break`statement that is needed at the end of each case in those languages is provided automatically in Go.
- Another important difference is that Go's switch cases need not be constants, and the values involved need not be integers.
- Switch cases evaluate cases from top to bottom, stopping when a case succeeds.

### **Switch with no condition**

```go
switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
}
```

- Switch without a condition is the same as `switch true`.
- This construct can be a clean way to write long if-then-else chains.

### Defer

```go
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

- A defer statement defers the execution of a function until the surrounding function returns.
- The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

### **Stacking defers**

```go
func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

- Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

## 3. More types: structs, slices, and maps

### Pointers

```go
var p *int

p := &i         // point to i
fmt.Println(*p) // read i through the pointer

*p = 21         // set i through the pointer
fmt.Println(i)  // see the new value of i
```

- Go has pointers. A pointer holds the memory address of a value.
- The type `*T` is a pointer to a `T` value. Its zero value is `nil`.
- Like C, `&` operator generates a pointer to its operand, and `*` operator denotes the pointer's underlying value. (known as "dereferencing" or "indirecting".)
- Unlike C, Go has no pointer arithmetic.

### Structs

```go
type Vertex struct {
	X int
	Y int
}

v := Vertex{1, 2}
v.X = 4

fmt.Println(v.X)
```

- A `struct` is a collection of fields.
- Struct fields are accessed using a dot.

### **Pointers to structs**

```go
v := Vertex{1, 2}
p := &v
p.X = 1e9
```

- To access the field `X` of a struct when we have the struct pointer `p` we could write `(*p).X`. However, that notation is cumbersome, so the language permits us instead to write just `p.X`, without the explicit dereference.

### **Struct Literals**

```go
var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)
```

- A struct literal denotes a newly allocated struct value by listing the values of its fields.
- You can list just a subset of fields by using the `Name:` syntax. (And the order of named fields is irrelevant.)
- The special prefix `&` returns a pointer to the struct value.

### Arrays

```go
var a [2]string
a[0] = "Hello"
a[1] = "World"
```

- The type `[n]T` is an array of `n` values of type `T`.
- An array's length is part of its type, so arrays cannot be resized.

### **Slices**

```go
primes := [6]int{2, 3, 5, 7, 11, 13}

var s []int = primes[1:4]
```

- A slice is a dynamically-sized, flexible view into the elements of an array.
- In practice, slices are much more common than arrays.
- The type `[]T` is a slice with elements of type `T`.
- A slice is formed by specifying two indices, a low and high bound, separated by a colon: `a[low : high]`
- This selects a half-open range which includes the first element, but excludes the last one.

### **Slices are like references to arrays**

```go
names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
a := names[0:2]
b := names[1:3]

b[0] = "XXX"
```

- A slice does not store any data, it just describes a section of an underlying array.
- Changing the elements of a slice modifies the corresponding elements of its underlying array.
- Other slices that share the same underlying array will see those changes.

### **Slice literals**

```go
r := []bool{true, false, true, true, false, true}
```

- A slice literal is like an array literal without the length.
- This creates the same array as above, then builds a slice that references it

### **Slice defaults**

```go
s = s[:2]
s = s[1:]
```

- When slicing, you may omit the high or low bounds to use their defaults instead.
- The default is zero for the low bound and the length of the slice for the high bound.

### **Slice length and capacity**

```go
s := []int{2, 3, 5, 7, 11, 13}

// Slice the slice to give it zero length.
s = s[:0]

// Extend its length.
s = s[:4]

// Drop its first two values.
s = s[2:]
```

- A slice has both a *length* and a *capacity*.
- The length of a slice is the number of elements it contains.
- The capacity of a slice is the number of elements in the underlying array, counting from the first element in the slice.
- The length and capacity of a slice `s` can be obtained using the expressions `len(s)` and `cap(s)`.
- You can extend a slice's length by re-slicing it, provided it has sufficient capacity.

### **Nil slices**

```go
var s []int
```

- The zero value of a slice is `nil`.
- A nil slice has a length and capacity of 0 and has no underlying array.

### **Creating a slice with make**

```go
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```

- Slices can be created with the built-in `make` function; this is how you create dynamically-sized arrays.
- The `make` function allocates a zeroed array and returns a slice that refers to that array. To specify a capacity, pass a third argument to `make`.

### **Slices of slices**

```go
board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
}
```

- Slices can contain any type, including other slices.

### **Appending to a slice**

```go
var s []int

s = append(s, 2, 3, 4)
```

- It is common to append new elements to a slice, and so Go provides a built-in `append`
   function.
- `func append(s []T, vs ...T) []T` : The first parameter `s` of `append` is a slice of type `T`, and the rest are `T` values to append to the slice.
- If the backing array of `s` is too small to fit, all the given values a bigger array will be allocated. The returned slice will point to the newly allocated array.

### **Range**

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
}
```

- The `range` form of the `for` loop iterates over a slice or map.
- When ranging over a slice, two values are returned for each iteration. The first is the index, and the second is a copy of the element at that index.

### **Range continued**

```go
pow := make([]int, 10)

for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
}

for _, value := range pow {
		fmt.Printf("%d\n", value)
}
```

- `for i, _ := range pow, for _, value` := range pow : You can skip the index or value by assigning to `_`.
- `for i := range pow` : If you only want the index, you can omit the second variable.

### **Exercise: Slices**

Implement `Pic`. It should return a slice of length `dy`, each element of which is a slice of `dx` 8-bit unsigned integers. When you run the program, it will display your picture, interpreting the integers as grayscale (well, bluescale) values.

The choice of image is up to you. Interesting functions include `(x+y)/2`, `x*y`, and `x^y`.

(You need to use a loop to allocate each `[]uint8` inside the `[][]uint8`.)

(Use `uint8(intValue)` to convert between types.)

**Answer Code**

```go
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	s := make([][]uint8, dy)
	for y := range s {
		s[y] = make([]uint8, dx)
		for x := range s[y] {
			s[y][x] = interpretCoordinate(x,y)
		}
	}

	return s
}

func interpretCoordinate(x, y int) uint8 {
	n := x^y

	return uint8(n)
}

func main() {
	pic.Show(Pic)
}
```

### Maps

```go
type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex
m = make(map[string]Vertex)

m["Bell Labs"] = Vertex{
	40.68433, -74.39967,
}

fmt.Println(m["Bell Labs"])
```

- A `map` maps keys to values.
- The zero value of a map is `nil`. A `nil` map has no keys, nor can keys be added.
- The `make` function returns a map of the given type, initialized and ready for use.

### **Map literals**

```go
var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
```

- Map literals are like struct literals, but the keys are required.
- If the top-level type is just a type name, you can omit it from the elements of the literal.

### **Mutating Maps**

```go
m := make(map[string]int)

m["Answer"] = 42
fmt.Println("The value:", m["Answer"])

m["Answer"] = 48
fmt.Println("The value:", m["Answer"])

delete(m, "Answer")
fmt.Println("The value:", m["Answer"])

v, ok := m["Answer"]
fmt.Println("The value:", v, "Present?", ok)
```

- `m[key] = elem` : Insert or update an element in map `m`
- `elem = m[key]` : Retrieve an element
- `delete(m, key)` : Delete an element
- `elem, ok = m[key]` : Test that a key is present with a two-value assignment
  - If `key` is in `m`, `ok` is `true`. If not, `ok` is `false`.
  - If `key` is not in the map, then `elem` is the zero value for the map's element type.
  - `elem, ok := m[key]` : If `elem` or `ok` have not yet been declared you could use a short declaration form

### **Exercise: Maps**

Implement `WordCount`. It should return a map of the counts of each “word” in the string `s`. The `wc.Test` function runs a test suite against the provided function and prints success or failure.

You might find [strings.Fields](https://go.dev/pkg/strings/#Fields) helpful.

**Answer Code**

```go
package main

import (
	"golang.org/x/tour/wc"
	"strings"
)

func WordCount(s string) map[string]int {
	m := make(map[string]int)

	ss := strings.Fields(s)
	for _, word := range ss {
		_, exists := m[word]

		if exists {
			m[word]++
		} else {
			m[word] = 1
		}
	}

	return m
}

func main() {
	wc.Test(WordCount)
}
```

### **Function values**

```go
func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
}
fmt.Println(hypot(5, 12))

fmt.Println(compute(hypot))
fmt.Println(compute(math.Pow))
```

- Functions are values too. They can be passed around just like other values.
- Function values may be used as function arguments and return values.

### Function closures

```go
func adder() func(int) int {
	sum := 0
	// returns a closure. Each closure is bound to its own sum variable.
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

- Go functions may be closures. A **closure** is a function value that references variables from outside its body.
- The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.

### **Exercise: Fibonacci closure**

Let's have some fun with functions.

Implement a `fibonacci` function that returns a function (a closure) that returns successive [fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_number) (0, 1, 1, 2, 3, 5, ...).

**Answer Code**

```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
	f1, f2 := 0, 1

	return func() int {
		// f is previous fibonacci number
		// f() closure actually returns
		// previous fibonacci number
		f := f1
		f1, f2 = f2, f1+f2

		return f
	}
}

func main() {
	f := fibonacci()
	for i := 0; i < 10; i++ {
		fmt.Println(f())
	}
}
```

## 4. Methods and interfaces

### **Methods**

```go
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

- Go does not have classes. However, you can define methods on types.
- A **method** is a function with a special *receiver* argument.
  - The receiver appears in its own argument list between the `func` keyword and the method name.
- _Remember: a method is just a function with a receiver argument._

### **Methods continued**

```go
type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```

- You can declare a method on non-struct types, too.
- You can only declare a method with a receiver whose type is defined in the same package as the method. You cannot declare a method with a receiver whose type is defined in another package (which includes the built-in types such as `int`).

### Pointer receivers

```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)
}
```

- You can declare methods with pointer receivers.
- Methods with pointer receivers can modify the value to which the receiver points.
  - Since methods often need to modify their receiver, pointer receivers are more common than value receivers.
- With a value receiver, the method operates on a copy of the original value. (This is the same behavior as for any other function argument.) The method must have a pointer receiver to change the value

### **Methods and pointer indirection**

```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

var v Vertex
v.Scale(5)  // OK
p := &v
p.Scale(10) // OK
```

- Methods with pointer receivers take either a value or a pointer as the receiver when they are called.
- That is, as a convenience, Go interprets the statement if the method has a pointer receiver.

```go
func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK
```

- The equivalent thing happens in the reverse direction.
- Methods with value receivers take either a value or a pointer as the receiver when they are called.

### **Choosing a value or pointer receiver**

```go
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

- There are two reasons to use a pointer receiver.
  1. Can modify the value that its receiver points to.
  2. Avoid copying the value on each method call. This can be more efficient if the receiver is a large struct, for example.

### **Interfaces**

```go
type Abser interface {
	Abs() float64
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat implements Abser
	a = &v // a *Vertex implements Abser

	// In the following line, v is a Vertex (not *Vertex).
  // Abs method is defined only on *Vertex (the pointer type).
	// and does NOT implement Abser.
  // thus error occurs.
	a = v

	fmt.Println(a.Abs())
}
```

- An *interface type* is defined as a set of method signatures.
- A value of interface type can hold any value that implements those methods.

### **Interfaces are implemented implicitly**

```go
type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}
```

- A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.
- Implicit interfaces decouple the definition of an interface from its implementation, which could then appear in any package without prearrangement.

### **Interface values**

```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I

	i = &T{"Hello"}
  // Calling a method on an interface value
	describe(i)
	i.M()
}
```

- Interface values can be thought of as a tuple of a value and a concrete type
- `(value, type)` :An interface value holds a value of a specific underlying concrete type.
- Calling a method on an interface value executes the method of the same name on its underlying type.

### **Interface values with nil underlying values**

```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}

func main() {
	var i I

	var t *T
	// implemented by nil concrete value
	i = t
	describe(i)
	i.M()
}
```

- If the concrete value inside the interface itself is nil, the method will be called with a nil receiver.
- In some languages this would trigger a null pointer exception, but in Go it is common to write methods that gracefully handle being called with a nil receiver
- Note that an interface value that holds a nil concrete value is itself non-nil.

### **Nil interface values**

```go
type I interface {
	M()
}

func main() {
	var i I
	describe(i)
	i.M()
}
```

- A nil interface value holds neither value nor concrete type.
- Calling a method on a nil interface is a run-time error because there is no type inside the interface tuple to indicate which *concrete* method to call.

### **The empty interface**

```go
func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}
```

- The interface type that specifies zero methods is known as the *empty interface*
- An empty interface may hold values of any type. (Every type implements at least zero methods.)
- Empty interfaces are used by code that handles values of unknown type.

### **Type assertions**

```go
s := i.(string)
fmt.Println(s)

s, ok := i.(string)
fmt.Println(s, ok)

f, ok := i.(float64)
fmt.Println(f, ok)

f = i.(float64) // panic
fmt.Println(f)
```

- A *type assertion* provides access to an interface value's underlying concrete value.
- `t := i.(T)` : asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t`.
- To *test* whether an interface value holds a specific type, a type assertion can return two values: the underlying value and a boolean value that reports whether the assertion succeeded.
  - If `i` holds a `T`, then `t` will be the underlying value and `ok` will be true.
  - If not, `ok` will be false and `t` will be the zero value of type `T`, and no panic occurs.
  - If `i` does not hold a `T` without two-value-return assertions, the statement will trigger a panic.

### **Type switches**

```go
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}
```

- A *type switch* is a construct that permits several type assertions in series.
- A type switch is like a regular switch statement, but the cases in a type switch specify types (not values), and those values are compared against the type of the value held by the given interface value.
- `v := i.(type)` : The declaration in a type switch has the same syntax as a type assertion `i.(T)`, but the specific type `T` is replaced with the keyword `type`.

### **Stringers**

```go
func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```

- One of the most ubiquitous interfaces is `Stringer` defined by the `fmt` package.
  ```go
  type Stringer interface {
      String() string
  }
  ```
- A `Stringer` is a type that can describe itself as a string. The `fmt` package (and many others) look for this interface to print values.

### **Exercise: Stringers**

Make the `IPAddr` type implement `fmt.Stringer` to print the address as a dotted quad.

For instance, `IPAddr{1, 2, 3, 4}` should print as `"1.2.3.4"`.

**Answer Code**

```go
package main

import "fmt"

type IPAddr [4]byte

// TODO: Add a "String() string" method to IPAddr.
func (ipAddr *IPAddr) String() string {
	return fmt.Sprintf("%v.%v.%v.%v", ipAddr[0], ipAddr[1], ipAddr[2], ipAddr[3])
}

func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}
```

### **Errors**

```go
type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

- Go programs express error state with `error` values.
- The `error` type is a built-in interface similar to `fmt.Stringer`
  ```go
  type error interface {
      Error() string
  }
  ```
- Functions often return an `error` value, and calling code should handle errors by testing whether the error equals `nil`.
  - A nil `error` denotes success; a non-nil `error` denotes failure.

### **Exercise: Errors**

Copy your `Sqrt` function from the [earlier exercise](https://go.dev/tour/flowcontrol/8) and modify it to return an `error` value.

`Sqrt` should return a non-nil error value when given a negative number, as it doesn't support complex numbers.

Create a new type

```
type ErrNegativeSqrt float64
```

and make it an `error` by giving it a

```
func (e ErrNegativeSqrt) Error() string
```

method such that `ErrNegativeSqrt(-2).Error()` returns `"cannot Sqrt negative number: -2"`.

**Note:** A call to `fmt.Sprint(e)` inside the `Error` method will send the program into an infinite loop. You can avoid this by converting `e` first: `fmt.Sprint(float64(e))`. Why?

Change your `Sqrt` function to return an `ErrNegativeSqrt` value when given a negative number.

**Answer Code**

```go
package main

import (
	"fmt"
	"math"
)

type ErrNegativeSqrt float64

func (e ErrNegativeSqrt) Error() string {
	return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
}

func Sqrt(x float64) (float64, error) {
	if x < 0.0 {
		return x, ErrNegativeSqrt(x)
	}

	z, zz := 1.0, 0.0

	for math.Abs(z-zz) > 1e-8 {
		z, zz = z-(z*z-x)/(2*z), z
	}

	return 0, nil
}

func main() {
	fmt.Println(Sqrt(2))
	fmt.Println(Sqrt(-2))
}
```

### **Readers**

```go
r := strings.NewReader("Hello, Reader!")

b := make([]byte, 8)
for {
	n, err := r.Read(b)
	fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
	fmt.Printf("b[:n] = %q\n", b[:n])
	if err == io.EOF {
		break
	}
}
```

- The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data.
- The Go standard library contains [many implementations](<https://cs.opensource.google/search?q=Read%5C(%5Cw%2B%5Cs%5C%5B%5C%5Dbyte%5C)&ss=go%2Fgo>) of this interface, including files, network connections, compressors, ciphers, and others.
- The `io.Reader` interface has a `Read` method:
  ```go
  func (T) Read(b []byte) (n int, err error)
  ```
  - `Read` populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an `io.EOF` error when the stream ends.

### **Exercise: Readers**

Implement a `Reader` type that emits an infinite stream of the ASCII character `'A'`.

**Answer Code**

```go
package main

import "golang.org/x/tour/reader"

type MyReader struct{}
type MyReaderError int

func (e MyReaderError) Error() string {
	return "capacity max error, cap: " + string(e)
}

// TODO: Add a Read([]byte) (int, error) method to MyReader.
func (myReader MyReader) Read(b []byte) (int, error) {
	if cap(b) < 1 {
		return 0, MyReaderError(cap(b))
	}

	b[0] = 'A'
	return 1, nil
}

func main() {
	reader.Validate(MyReader{})
}
```

### **Exercise: rot13Reader**

A common pattern is an [io.Reader](https://go.dev/pkg/io/#Reader) that wraps another `io.Reader`, modifying the stream in some way.

For example, the [gzip.NewReader](https://go.dev/pkg/compress/gzip/#NewReader) function takes an `io.Reader` (a stream of compressed data) and returns a `*gzip.Reader` that also implements `io.Reader` (a stream of the decompressed data).

Implement a `rot13Reader` that implements `io.Reader` and reads from an `io.Reader`, modifying the stream by applying the [rot13](https://en.wikipedia.org/wiki/ROT13) substitution cipher to all alphabetical characters.

The `rot13Reader` type is provided for you. Make it an `io.Reader` by implementing its `Read` method.

**Answer Code**

```go
package main

import (
	"io"
	"os"
	"strings"
)

type rot13Reader struct {
	r io.Reader
}

func rot13(b byte) byte {
	var a, z byte

	switch {
		case 'a' <= b && b <= 'z':
			a, z = 'a', 'z'
		case 'A' <= b && b <= 'Z':
			a, z = 'A', 'Z'
		default:
			return b
	}

	return (b-a+13)%(z-a+1) + a
}

func (reader rot13Reader) Read(b []byte) (int, error) {
	n, err := reader.r.Read(b)

	for i := 0; i < n; i++ {
		b[i] = rot13(b[i])
	}

	return n, err
}

func main() {
	s := strings.NewReader("Lbh penpxrq gur pbqr!")
	r := rot13Reader{s}
	io.Copy(os.Stdout, &r)
}
```

### Images

```go
m := image.NewRGBA(image.Rect(0, 0, 100, 100))
fmt.Println(m.Bounds())
fmt.Println(m.At(0, 0).RGBA())
```

- [Package image](https://go.dev/pkg/image/#Image) defines the `Image` interface:

  ```go
  package image

  type Image interface {
      ColorModel() color.Model
      Bounds() Rectangle
      At(x, y int) color.Color
  }
  ```

  - the `Rectangle` return value of the `Bounds` method is actually an `[image.Rectangle](https://go.dev/pkg/image/#Rectangle)`, as the declaration is inside package `image`.
  - The `color.Color` and `color.Model` types are also interfaces, but we'll ignore that by using the predefined implementations `color.RGBA` and `color.RGBAModel`. These interfaces and types are specified by the [image/color package](https://go.dev/pkg/image/color/)

### **Exercise: Images**

Remember the [picture generator](https://go.dev/tour/moretypes/18) you wrote earlier? Let's write another one, but this time it will return an implementation of `image.Image` instead of a slice of data.

Define your own `Image` type, implement [the necessary methods](https://go.dev/pkg/image/#Image), and call `pic.ShowImage`.

`Bounds` should return a `image.Rectangle`, like `image.Rect(0, 0, w, h)`.

`ColorModel` should return `color.RGBAModel`.

`At` should return a color; the value `v` in the last picture generator corresponds to `color.RGBA{v, v, 255, 255}` in this one.

**Answer Code**

```go
package main

import (
	"golang.org/x/tour/pic"
	"image"
	"image/color"
)

type Image struct {
	width, height int
}

func (i Image) ColorModel() color.Model {
	return color.RGBAModel
}

func (i Image) Bounds() image.Rectangle {
	return image.Rect(0, 0, i.width, i.height)
}

func (i Image) At(x, y int) color.Color {
	return color.RGBA{interpretCoordinate(x,y),interpretCoordinate(x,y), 255, 255}
}

func interpretCoordinate(x, y int) uint8 {
	n := x^y

	return uint8(n)
}

func main() {
	m := Image{100, 100}
	pic.ShowImage(m)
}
```

## 5. Generics

### **Type parameters**

```go
// Index returns the index of x in s, or -1 if not found.
func Index[T comparable](s []T, x T) int {
	for i, v := range s {
		// v and x are type T, which has the comparable
		// constraint, so we can use == here.
		if v == x {
			return i
		}
	}
	return -1
}

func main() {
	// Index works on a slice of ints
	si := []int{10, 20, 15, -10}
	fmt.Println(Index(si, 15))

	// Index also works on a slice of strings
	ss := []string{"foo", "bar", "baz"}
	fmt.Println(Index(ss, "hello"))
}
```

- Generic functions: Go functions can be written to work on multiple types using type parameters.
- `func Index[T comparable](s []T, x T) int` : The type parameters of a function appear between brackets, before the function's arguments.
  - This declaration means that `s` is a slice of any type `T` that fulfills the built-in constraint `comparable` (`comparable` is a useful constraint that makes it possible to use the `==` and `!=` operators on values of the type.)

### **Generic types**

```go
// List represents a singly-linked list that holds
// values of any type.
type List[T any] struct {
	next *List[T]
	val  T
}
```

- Go also supports generic types
- A type can be parameterized with a type parameter, which could be useful for implementing generic data structures.

## 6. Concurrency

### **Goroutines**

```go
func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	// Goroutine
	go say("world")

	say("hello")
}
```

- A *goroutine* is a **lightweight thread** managed by the Go runtime.
- `go f(x, y, z)` : starts a new goroutine running
  - The evaluation of `f`, `x`, `y`, and `z` happens in the current goroutine and the execution of `f` happens in the new goroutine.
- Goroutines run in the same address space, so access to shared memory must be synchronized.
  - The [sync](https://go.dev/pkg/sync/) package provides useful primitives, although you won't need them much in Go as there are other primitives.

### **Channels**

```go
func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

- Channels are a **typed conduit** through which you can send and receive values with the channel operator, `<-`.
- `<-` : The data flows in the direction of the arrow.
  ```go
  ch <- v    // Send v to channel ch.
  v := <-ch  // Receive from ch, and
             // assign value to v.
  ```
- `ch := make(chan int)` : Like maps and slices, channels must be created before use.
- By default, sends and receives **block(ed) until the other side is ready**. → This allows goroutines to synchronize without explicit locks or condition variables.

### **Buffered Channels**

```go
ch := make(chan int, 2)
ch <- 1
ch <- 2
fmt.Println(<-ch)
fmt.Println(<-ch)
```

- Channels can be *buffered*
- `ch := make(chan int, 100)` : Provide the buffer length as the second argument to `make` to initialize a buffered channel.
- Sends to a buffered channel block(ed) only when the buffer is full. Receives block(ed) when the buffer is empty.
  - overfill buffer will casuse deadlock

### **Range and Close**

```go
func fibonacci(n int, c chan int) {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	go fibonacci(cap(c), c)

	// The loop for i := range c receives values from the channel repeatedly
  // until it is closed.
	for i := range c {
		fmt.Println(i)
	}
}
```

- A sender can `close` a channel to indicate that no more values will be sent.
- `v, ok := <-ch` : Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression
  - `ok` is `false` if there are no more values to receive and the channel is closed.
- **Note:** Only the sender should close a channel, never the receiver. Sending on a closed channel will cause a panic.
- **Another note:** Closing is only necessary when the receiver must be told there are no more values coming, such as to terminate a `range` loop.

### Select

```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

- The `select` statement lets a goroutine wait on multiple communication operations.
- A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

### **Default Selection**

```go
tick := time.Tick(100 * time.Millisecond)
boom := time.After(500 * time.Millisecond)

for {
		select {
			case <-tick:
				fmt.Println("tick.")
			case <-boom:
				fmt.Println("BOOM!")
				return
			default:
				fmt.Println("    .")
				time.Sleep(50 * time.Millisecond)
		}
	}
```

- The `default` case in a `select` is run if no other case is ready.
- Use a `default` case to try a send or receive without blocking:
  ```go
  select {
  	case i := <-c:
  	    // use i
  	default:
  	    // receiving from c would block
  }
  ```

### **Exercise: Equivalent Binary Trees**

A function to check whether two binary trees store the same sequence is quite complex in most languages. We'll use Go's concurrency and channels to write a simple solution.

**1.** Implement the `Walk` function.

**2.** Test the `Walk` function.

The function `tree.New(k)` constructs a randomly-structured (but always sorted) binary tree holding the values `k`, `2k`, `3k`, ..., `10k`.

Create a new channel `ch` and kick off the walker:

```
go Walk(tree.New(1), ch)
```

Then read and print 10 values from the channel. It should be the numbers 1, 2, 3, ..., 10.

**3.** Implement the `Same` function using `Walk` to determine whether `t1` and `t2` store the same values.

**4.** Test the `Same` function.

`Same(tree.New(1), tree.New(1))` should return true, and `Same(tree.New(1), tree.New(2))` should return false.

**Answer Code**

```go
package main

import (
	"fmt"
	"golang.org/x/tour/tree"
)

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int) {
	if t == nil {
		return
	}
	Walk(t.Left, ch)
	ch <- t.Value
	Walk(t.Right, ch)
}

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool {
	result := true
	ch1, ch2 := make(chan int), make(chan int)

	go Walk(t1, ch1)
	go Walk(t2, ch2)

	for {
		v1, ok1 := <-ch1
		v2, ok2 := <-ch2

		if (ok1 || ok2) == false {
			break
		} else if (ok1 && ok2) == false || v1 != v2 {
			result = false
			break
		}
	}

	return result
}

func main() {
	fmt.Println(Same(tree.New(1), tree.New(2)))
}
```

### **sync.Mutex**

```go
// SafeCounter is safe to use concurrently.
type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

// Inc increments the counter for the given key.
func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	c.v[key]++
	c.mu.Unlock()
}

// Value returns the current value of the counter for the given key.
func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	defer c.mu.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}
```

- Go's standard library provides mutual exclusion with [sync.Mutex](https://go.dev/pkg/sync/#Mutex) and its two methods: `Lock`, `Unlock`
  - _mutual exclusion:_ concept of make sure only one goroutine can access a variable at a time to avoid conflicts
  - _mutex:_ conventional name for the data structure that provides mutual exclusion
- We can define a block of code to be executed in mutual exclusion by surrounding it with a call to `Lock` and `Unlock`
  - We can also use `defer` to ensure the mutex will be unlocked as in the `Value` method.

### **Exercise: Web Crawler**

In this exercise you'll use Go's concurrency features to parallelize a web crawler.

Modify the `Crawl` function to fetch URLs in parallel without fetching the same URL twice.

_Hint_: you can keep a cache of the URLs that have been fetched on a map, but maps alone are not safe for concurrent use!

**Answer Code**

```go
package main

import (
	"fmt"
	"sync"
)

type Cache struct {
	mu sync.Mutex
	urls map[string]bool
}

func (c *Cache) InsertUrls(urls []string) {
	c.mu.Lock()
	defer c.mu.Unlock()

	for _, url := range urls {
		c.urls[url] = false
	}
}

func (c *Cache) UpdateFetchedUrl(url string) {
	c.mu.Lock()
	defer c.mu.Unlock()

	c.urls[url] = true
}

func (c *Cache) IsUrlFetched(url string) bool {
	c.mu.Lock()
	defer c.mu.Unlock()

	return c.urls[url]
}

type Fetcher interface {
	// Fetch returns the body of URL and
	// a slice of URLs found on that page.
	Fetch(url string) (body string, urls []string, err error)
}

// Crawl uses fetcher to recursively crawl
// pages starting with url, to a maximum of depth.
func Crawl(url string, depth int, fetcher Fetcher, cache *Cache, wg *sync.WaitGroup) {
	defer wg.Done()

	if depth <= 0 {
		return
	}
	body, urls, err := fetcher.Fetch(url)
	// insert new urls in cache
	// and update url fetched
	cache.InsertUrls(urls)
	cache.UpdateFetchedUrl(url)

	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("found: %s %q\n", url, body)
	for _, u := range urls {
		// check cache if url is already fetched
		if !cache.IsUrlFetched(u) {
			wg.Add(1)
			go Crawl(u, depth-1, fetcher, cache, wg)
		}
	}
	return
}

// synced crawl using sync.Metux and sync.WaitGroup
func SyncCrawl(url string, depth int, fetcher Fetcher) {
	var wg sync.WaitGroup
	cache := Cache{urls: make(map[string]bool)}

	wg.Add(1)
	go Crawl(url, depth, fetcher, &cache, &wg)
	wg.Wait()
}

func main() {
	SyncCrawl("https://golang.org/", 4, fetcher)
	fmt.Println("done")
}

// fakeFetcher is Fetcher that returns canned results.
type fakeFetcher map[string]*fakeResult

type fakeResult struct {
	body string
	urls []string
}

func (f fakeFetcher) Fetch(url string) (string, []string, error) {
	if res, ok := f[url]; ok {
		return res.body, res.urls, nil
	}
	return "", nil, fmt.Errorf("not found: %s", url)
}

// fetcher is a populated fakeFetcher.
var fetcher = fakeFetcher{
	"https://golang.org/": &fakeResult{
		"The Go Programming Language",
		[]string{
			"https://golang.org/pkg/",
			"https://golang.org/cmd/",
		},
	},
	"https://golang.org/pkg/": &fakeResult{
		"Packages",
		[]string{
			"https://golang.org/",
			"https://golang.org/cmd/",
			"https://golang.org/pkg/fmt/",
			"https://golang.org/pkg/os/",
		},
	},
	"https://golang.org/pkg/fmt/": &fakeResult{
		"Package fmt",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
	"https://golang.org/pkg/os/": &fakeResult{
		"Package os",
		[]string{
			"https://golang.org/",
			"https://golang.org/pkg/",
		},
	},
}
```
