# Go Study
Go study with [Go offical documentation](https://go.dev/doc/)

## Documentation Summary

### [Tutorial: Get started with Go](https://go.dev/doc/tutorial/getting-started.html)

1. `go mod init` : Enable dependency tracking 
    1. **Dependency Tracking**: When your code imports packages contained in other modules, you manage those dependencies through your code's own module. That module is defined by a go.mod file that tracks the modules that provide those packages. That go.mod file stays with your code, including in your source code repository.
2. `go run .` : Run go code
3. `go mod tidy` : Locate dependencies and download imported package and check sums + synchronize and add module
    1. [pkg.go.dev](http://pkg.go.dev) site to find published modules whose packages have functions you can use in your own code

### [Create a module](https://go.dev/doc/tutorial/create-module.html)

1. **Modules**
    1. You collect one or more related packages for a discrete and useful set of functions.
    2. Go code is grouped into packages, and packages are grouped into modules. Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.
    3. If you publish a module, this *must* be a path from which your module can be downloaded by Go tools. That would be your code's repository.
2. **Function**
    
    ![function-syntax.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fb591da-8c69-4316-b0e5-51431181d3cc/function-syntax.png)
    
    1. In Go, a function whose name starts with a capital letter can be called by a function not in the same package. This is known in Go as an exported name.
3. `:= Operator`
    
    ```go
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    ```
    
    1. shortcut for declaring and initializing a variable in one line (Go uses the value on the right to determine the variable's type).
    2. Long verison: 
        
        ```go
        var message string
        message = fmt.Sprintf("Hi, %v. Welcome!", name)
        ```
        

### [Call your code from another module](https://go.dev/doc/tutorial/call-module-code.html)

1. `main` package: In Go, code executed as an application must be in a main package.
2. `go mod edit -replace` : replace moudle location (ex: replace module location to internal file system)
    
    ```go
    $ go mod edit -replace example.com/greetings=../greetings
    ```
    
3. `require` in go.mod: To reference a *published* module, a go.mod file would typically omit the `replace` directive and use a `require` directive with a tagged version number at the end.
    
    ```go
    require example.com/greetings v1.1.0
    ```
    