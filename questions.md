# Questions

## How do we run code in our project?

    go run <filename.go> -> this compiles and executes the result
    go build <filename.go> -> just compiles but does not execute
    go fmt -> formats all the code in each file in the curr directory
    go install -> compiles and installs packages
    go get -> downloads the raw source code of someone else's package
    go test -> runs ant tests associated with the current project

## Where to find documentation for standard lib?

[Documentation](https://pkg.go.dev/std)

## Are packages from other engineers imported the same way as standard lib?

Standard library comes as part of Go, but other packages are resolved through modules, which is how Go handles dependencies.
A package is looked up in a module that has been downloaded/installed

[Go Modules Reference](https://go.dev/ref/mod)

## How does statically typed compile code vs dynamic typed?

Statically typed programming languages do type checking (i.e. the process of verifying and enforcing the constraints of types) at compile-time as opposed to run-time.

Dynamically typed programming languages do type checking at run-time as opposed to compile-time.

## Why does **go run main.go state.go** run correctly but vscode complains about declaration missing for the func in a different file?

I had to update the go plugins on vscode

## How do I declare a slice with the long declaration form?

    var greetings []string = []string{"hi", "hola", "bonjour"}

Slices are a struct that has an field that points to position 0 of the underlying array

## In go, is the index always required in the loop?

    for card := range cards {
    fmt.Println(card)
    }

The codeblock above outputs the index numbers

In order to ignore a particular variable you can put underscore's in its place

## What does the range keyword actually return?

    for i, suit := range cardSuits

Its a keyword that before the first run of the loop and sets up the values to be iterated over

[For with range clause](https://go.dev/ref/spec#For_range)

## How does garbage collection work in go?

Ex. I create a slice and then append to it, what happens to the original

TLDR: Go tries to put everything it knows the lifetime about on the stack (LIFO) structs tend to go on the heap and get collected once they aren't needed anymore.

A slice will go on the stack unless its too big and leaks into the heap. Basically anything that can go on the stack is preferred

[In depth analysis of GC](https://go.dev/blog/ismmkeynote)
[Summary](https://medium.com/safetycultureengineering/an-overview-of-memory-management-in-go-9a72ec7c76a8)

## What happens if I pass this method a slice of type string instead of a deck

    func deal(d deck, handSize int) (deck, deck) {}

This actually allows me to pass in a slice of type string, so it seems like whatever type a new extends, can be used almost like a parent

    test := []string{"abc", "123"}
    hand, remainingCards := deal(test, 1)
    Result: Hand: abc, Remaining: 123

## Is there a limit on the amount of return values from each func?

There is not real limit set on this, apart from what your computer and brain could handle

## Can you create a string (or any other var) with

    x := type("value")

Whatever type you are using allows you to call it like a method

    test := []string{"123"}
    x := deck(test)
    x.print()
    Result: 123

When joining the deck, it works whether I convert the deck to a string slice or not

## Does go automatically close files

The GC will eventualy handle unclosed files but it is much better practice to explicitly close them

## What is the go.mod file and what does this command do?

    $ go env -w GO111MODULE=auto

(Module-aware commands)[https://go.dev/ref/mod#mod-commands]
GO111MODULE controls if go ignores modules or not. I found this out by running

    go help environment

## When I use a pointer in function receiver type, why do I need to put another \* in the variable being used to get the value

    description of the type more than an actual operators

## Is it more efficient to iterate over a map or over a struct... can I even iterate over a struct?

Out of the box, iterating over a map is straight forward, to iterate over a struct, reflection is needed

## How does a nested interface work?

    https://pkg.go.dev/io#ReadCloser --> example of this

Anyone can be under a nested interface as long as they meet the requirements for the interfaces inside of it
In this case, Read([]byte)(int,error) and Close()(error)

## How exactly does the map of string, interface work?

When using interface{}, this essentially will accept any time since interfaces only care about methods and an empty interface will match with anything. This is really useful when we do not know what type of data we are going to receive but requires us to eventually match the actual type later on, otherwise there is no useful data to work with.

[Map, String and Interface](https://bitfieldconsulting.com/golang/map-string-interface)

## Is it good practice to use the \_ as a way to match function types?

It depends on the reason for doing it, but generally it seems like if its being done just to match an interface its probably not good practice
