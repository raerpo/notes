## Instalation 
To install the Go compiler you just need to download the installer from the website. To check if the installation was correct you can run:
```
go version
```

## Packages
It seems that third party packages are installed on the `$HOME/go` folder. Kind of like a node_modules but available to every program :thinking:. This could be problematic when we need to upload the code to a server. I´m sure there is way to make sure everything runs fine on the origin device like venv in python.

The book says that is recommended to add set the *GOPATH*

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

We can see a list of env variables available with the command `go env`. That looked very organized and clear. 

## Go command
The compiler comes with a bunch of preinstalled tools. There´s one command that works as prettier.

```
go fmt hello.go
```

We can also run the file with *run* or compile it with *build*.

Go is compiled language but what the *run* command does is to compile a temporal binary and deletes it after the program was executed. That´s cool because it acts like an scripting language like python.

## Install packages
In Go there´s not something like NPM as a centralized place with packages but instead we need to provide the URL of the package and the version that we want to install.

```
go install github.com/rakyll/hey@latest
```

the *install* command will download the package, compile it and put it in the *GOPATH/bin* folder.

After the package was installed it is now accesible directly on the command line:

```
hey https://www.golang.org
```
## Formating
Tne compiler comes with a formater included *go fmt* but there´s also a enhanced version:

```
# to install 
go install golang.org/x/tools/cmd/goimports@latest

# to use it
goimports -l -w .
```

- The -l flag print the files with incorrect formating.
- The -w flag modify the files in place.
- The dot match every file on the folder.

## Linting
there´s also a tool to lint our code:

```
go install golang.org/x/lint/golint@latest
golint ./...
```

The linting process is basically an automatic enforcer of the [Effective Go](https://golang.org/doc/effective_go) and the [Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

## Vetting
the include tool *go vet* tries to find errors on the code like passing a wrong number of paramaters.

> Vet should be mandatory on the CI process but linting can be more flexible since there are false positives.

## Make process
In order to automate the linting, vetting, formating and building process we can use a Makefile.

```
.DEFAULT_GOAL := build

fmt:
	go fmt ./...
.PHONY:fmt

lint: fmt
	golint ./...	
.PHONY:lint

vet: lint
	go vet ./...
.PHONY:vet

build: vet
	go build hello.go
.PHONY:build
```

In this Makefile we set up the default target in case nothing is passed.

> What comes after the : is the requirement. So build: vet means that vet should run before build.

## Module
When can initialize a module by running the command `go mod init example.com/m`. This will create a file named go.mod that contains the version of go that I´m working with and also what it seems to be the URL from where someone else can install it. 

TODO: I should check if my assumption of the URL is correct.

# Primitives

## Literals
- integer literals: 20, 100, etc. We can use underscores to separate them. Eg: 10_000 is equal to 10000
- floating point literals: 3.1416
- rune literals: they represent characters and they are surrounded by single quotes. The difference between a char in other languages is that rune is bigger so it can have any character in any human language.
- string literals: Are defined with double quotes or backtick quotes to use raw strings.
- booleans: something interesting here is that because Go has the concept of zero values, when we defined a boolean variable without assigning it a value, the default value will be *false*.

## Types
Integers can have multiple types. For example: int8, int16, uint32, uint64, etc. The zero value of a string is *0*.

> The type Int can be different between platforms. In a 32-bit platform the int will be int32 but in a 64-bit platform the type int will be a int64. That´s why a operation between a int and a int32 or int64 will result in a error.

For floating point numbers we have float32 and float64. Unless we have a specific restriction we should always use *float64*

> A floating point number cannot represent a decimal value exactly. Do not use them to represent money or any other value that must have an exact decimal representation!

One important thing about floats is that they are inacurate so that is something we should take in consideration when we compare float values.

## Operations
We can use the usual operators in the same way as javascript. In Go if we operate integers the response will be a integer.

## Explicit type conversion
Go does not allow automatic type conversions like javascript. We need to be explicit by changing the type of the operands in an operation. One interesting effect of this is that we cannot use any other value as a boolean. So if need to check some value in a conditional the only way to do it is by using operators.

## Ways to declare variables

```
var x int = 10
```

if the type of the right part of the declaration is the same as the one that we defined, we can omit the typing.

```
var x = 10
```

We can also declare a variable without any value and it will take the zero value for that type:

```
var x int;
// The zero value of int is 0
```

We can declare multiple variables of the same type in the same line:

```
var x, y int = 10, 20
```

We also have a short declaration when we want to infer the value:

```
var x = 10
x := 10
// These two are equivalent
```

> The := operator is can only be use inside functions

## Which declaration should we use?

- Inside functions prefer shorthand.
- Use `var` if you want to initialize with the zero value.
- When assigning an untyped constant or a literal to a variable and the default type for the constant or literal isn’t the type you want for the variable.
  Even when is not illegal is better to write `var x byte = 20` than `x := byte(20)`

> As a general rule we shouldn't define variables outside of the block in which we plan to use it or change it

## Const
We can use *const* in Go to create constant values in the same way we use *var* to create them. The difference between the const in javascript for example is that in Go we cannot assign a value that will be calculated at runtime. We can only assign primitive values to consts in Go.

> Basically a const in Go is a way to add a name to a literal but there is no way to make a variable inmutable.

In Go constants are not written as in javascript by using everything in uppercase. It seems that variables that start with an uppercased letter has a specific meaning.

TODO: review why we can name constants as all uppercase letters

# Composite types
## Arrays
In Go, every element of an array should be of the same type. There's a couple ways to declare arrays.

- Declare the type of the array and the length: `var x [3]int`
- If we have the initial values we can define then directly `var x = [3]int{10,20,30}`
- We can also create a new array with zero values and some index: `x := [5]int{2: 1}` this will print *[0 0 1 0 0]*
- If we provide the literal array we can use `...` inside the length: `x := [...]string{"hello", "world"}`.

In Go we can compare arrays unlike Javascript that the comparasion is by reference and not by value:

```
x := [...]int{1, 2, 3}
y := [3]int{1, 2, 3}
println(x == y) // is true
```

We can simulate a matrix like this:

```
x := [3][3]int
```

- We can use the `len` function to get the length of an array.

> In Go the size of an array is part of it´s type. So a [3]int is a different type than [4]int.

## Slices

Since the arrays in GO should have a defined length, normally what we want to use are slices.

Declaring a slice is very similar to declaring an array:

```
var x = []int{1,2,3,4}
```

That means that with `[...]` we create an array and with `[]` we create a slice.

We can declare a slice without a literal value.

```
var x int[]
```

- The zero value of a slice is *nil*
- We cannot compare slices like we did we arrays using == or =!