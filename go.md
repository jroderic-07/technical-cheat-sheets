# Go Programming Cheat Sheet

# Index
1. [Tools](#tools)
2. [Modules and Packages](#modules-and-packages)
3. [Pointers](#pointers)
4. [Method Receivers](#method-receivers)
5. [Interfaces](#interfaces)
6. [Object Oriented](#object-oriented)
7. [Concurrency](#concurrency)
8. [Mutexes](#mutexes)
9. [Generics](#generics)
10. [High Order Functions](#high-order-functions)

# Tools
## Go
- Main tool for managing Go source code
- Can be used to build packages: go build [-o output] [build flags] [packages]
- Can be used to print off environment information: go env

## Go Test

# Modules and Packages
## Modules
- A Go module is a collection of packages.
- To create a Go module, run: go mod <module_name>
- Go.mod is an important file for a Go project. It contains information about the module and versions of other modules that it depeneds on.

## Packages
- Packages are directories in a Go workspace that contain Go source files or other Go package.
- Every single piece of code written is linked to a package.
- The package is specified at the start of every go file.
- The main package indicates that the page contains code for an executable application.

## Example
- To add a new module to your go.mod file, run go get <module_name>
- When a module as been added to your go.mod file, the //indirect implies that it is not directly called by any source files.
- To create a package in your module, create a directory and add some go files with the first line being 'package <your_package_name>'. Import the code in your main package, and build or run the code. The new package will appear in your go.mod file.

# Pointers
- \* operator - dereferencing operator. Used for accessing the value in the address stored by a pointer.
- & operator - address operator. Used for returning the address of a variable stored in the pointer.
- Pass by value - will pass the value of a variable into a method, and we can say that the original variable copy copy the value into anther memory location and pass the newly creates one into the method. Anything happening inside of the method will not affect the original variable.
- Pass by reference - will pass the memory location istead of a value. In other words, it passes the container of a variable to a method. Anything happening inside of the method will affect the original variable.

```
    import (
            "fmt"
    )

    func main() {
            num := 5
            fmt.Printf("Initial variable value %d\n", num)
            fmt.Printf("Initial variable address %d\n", &num)

            Add(num)
            fmt.Printf("Variable after Add function %d\n", num)

            AddPtr(&num)
            fmt.Printf("Variable after AddPtr function %d\n", num)
    }

    func Add(num int) {
            num++
            fmt.Printf("New value: %d\n", num)
    }

    func AddPtr(num *int) {
            *num++
            fmt.Printf("New value: %d\n", *num)
    }
```
# Method Receivers
-  A special parameter that is passed to a method. The method receiver can be used to access and motify the object the metod is called on.

# Interfaces
- An interface is a collection of method signatures. It is a way to define a contract between a type and its clients. It specifies which methods a type must implement, but it does not specify how they should be implemented.
- They can be used to decouple your code. This can make it more maintainable and usable.
- They can be used to make your code more generic.

```
    package main

    import (
            "fmt"
            "math"
    )

    type geometry interface {
            area() float64
            perim() float64
    }

    type rect struct {
            width, height float64
    }

    type circle struct {
            radius float64
    }

    func (r rect) area() float64 {
            return r.width * r.height
    }

    func (r rect) perim() float64 {
            return 2*r.width + 2*r.height
    }

    func (c circle) area() float64 {
            return math.Pi * c.radius * c.radius
    }

    func (c circle) perim() float64 {
            return 2 * math.Pi * c.radius
    }

    func measure(g geometry) {
            fmt.Println(g)
            fmt.Println(g.area())
            fmt.Println(g.perim())
    }

    func main(){
            r := rect{width: 3, height: 4}
            c := circle{radius: 5}

            measure(r)
            measure(c)
    }
```

- The above example uses a value receiver. You can also use a pointer receiever.
-  A pointer receiver means that the method can modify the value that the receiver points to.
- It also avoids copying values on each method call, making it more efficient if you have a larger struct.

```
    package main

    import (
            "fmt"
            "math"
    )

    type geometry interface {
            area() float64
            perim() float64
    }

    type rect struct {
            width, height float64
    }

    type circle struct {
            radius float64
    }

    func (r *rect) area() float64 {
            return r.width * r.height
    }

    func (r *rect) perim() float64 {
            return 2*r.width + 2*r.height
    }

    func (c *circle) area() float64 {
            return math.Pi * c.radius * c.radius
    }

    func (c *circle) perim() float64 {
            return 2 * math.Pi * c.radius
    }

    func measure(g geometry) {
            fmt.Println(g)
            fmt.Println(g.area())
            fmt.Println(g.perim())
    }

    func main(){
            r := &rect{width: 3, height: 4}
            c := &circle{radius: 5}

            measure(r)
            measure(c)
    }
```

# Object Oriented
- Go is not a class based object oriented programming language. It does support some OOP concepts.
- It uses structs, methods and interfaces.
- Structs can be used to represent objects and methods.

```
    package main

    import (
            "fmt"
    )

    type Person struct{
            Name string
    }

    func (p Person) Speak() {
            fmt.Printf("Hello, my name is %s\n", p.Name)
    }

    func main() {
            person1 := Person{Name: "joe"}
            person1.Speak()
    }
```

- Interfaces can also be used too

```
    package main

    import (
            "fmt"
    )

    type Person struct{
            Name string
    }

    func (p Person) Speak() string {
            return "Hello World!"
    }

    type Speaker interface {
            Speak() string
    }

    func PrintSpeakerGreeting(s Speaker) {
            fmt.Println(s.Speak())
    }

    func main() {
            person1 := Person{Name: "joe"}
            PrintSpeakerGreeting(person1)
    }
```

- GoLang does not use inheritence. It can do object oriented by composition, which allows you to combine objects.

```

```

# Concurrency

# Mutexes

# Generics
- Generics provide a way to write code that is not specific to any particular type, which can make you code more flexible and reusable.
- Generics are implemented in Go using type parameters - these are placeholders that can be replaced with any type when the code is used.

```
        package main

        import (
                "fmt"
        )

        type Num interface {
                int | int8 | int16 | int32 | float64
        }

        func Max[T Num](a, b T) T {
                if a > b {
                        return a
                }
                return b
        }

        func main() {
                maxInt := Max(10.5, 2.5)
                fmt.Println(maxInt)

                maxFloat := Max(12, 16)
                fmt.Println(maxFloat)
        }
```

- Rather than defining your own type parameters, this library has them: https://pkg.go.dev/golang.org/x/exp/constraints

```
        package main

        import (
                "fmt"
                "golang.org/x/exp/constraints"
        )

        func Add[T constraints.Ordered](a T, b T) T {
                return a + b
        }


        func main() {
                result := Add(5.5,6.7)
                fmt.Println(result)
        }
```

- Tilde is used to denote the set of types whose underlying type is T

```
        package main

        import (
                "fmt"
        )

        type myString string

        func Print[T ~string](s T)  {
                fmt.Println(s)
        }

        func main() {
                Print("Hello World")
                Print(myString("Hello World Test"))
        }
```

# High Order Functions

- Higher-order function - a function that either takes in another function as an argument or returns one.
- Common types of higher order functions include map, reduce and filter.

```
        package main

        // input: [1, 2, 3], (n) => n * 2
        // output: [2, 4, 6]

        import (
                "fmt"
                "golang.org/x/exp/constraints"
        )

        func MapValues[T constraints.Ordered](values []T, mapFunc func(T) T) []T {
                var newValues []T
                for _, v := range values {
                        newValue := mapFunc(v)
                        newValues = append(newValues, newValue)
                }

                return newValues
        }

        func main() {
                vals := MapValues([]int{1,2,3}, func(n int) int {
                        return n * 2
                })

                fmt.Println(vals)

                vals2 := MapValues([]float64{1.2,2.4,3.6}, func(n float64) float64 {
                        return n * 2
                })

                fmt.Println(vals2)
        }
```

# Unsafe