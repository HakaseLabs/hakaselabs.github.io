---
layout: post
title: "How I Learned Golang"
author: "@codehakase"
---

Go is a relatively new programming language, and nothing makes a developer go crazier than a new programming language, haha! As many new tech inventions, Go was created as an experiment. The goal of its creators was to come up with a language that would resolve bad practices of others while keeping the good things. It was first released in March 2012. Since then Go has attracted many developers from all fields and disciplines.

During the second quarter of this Year, I joined *ViVA XD*, an In-flight entertainment platform changing the world one trip at a time. We chose Go to build the whole infrastructure. Go was/is still an awesome choice for us in terms of performance, and its fast and simple development process.

In this article, I'll give a short introduction of the Go language and it would serve as a motivation for further reading. 

## Getting Started
I had no prior experience with Go, as a norm, Learning a new programming language, I started out by checking the [Golang offical webpage](https://golang.org/). I found the installation instructions for my OS, tutorials, an awesome documentation. It was terrifying (as the first Statically Typed language I've used). [The Tour of Go](https://tour.golang.org/welcome/1) was recommended, its an interactive Go tutorial (comes bundled with Go) which got me familiar with Go's syntax and literals. I finished with the Tour in a couple of days, and got an idea of how things work in the Go environment like variables, types, structs, pointers, lists, etc. At this point, I didn't really grasp all about Go, my experience from other languages gave me enough knowledge during my first task at ViVA XD. Go does an awesome job making us compile our source code to a single binary for any platform. Our app runs on Linux, so if we had a developer working on a Mac she can compile it down easily
`GOOS=linux go build` and in a matter of seconds a Linux build is ready.

## Going deeper
I got familiar with the syntax, and other important part of the Go language, like goroutines, concurrency, error handling, packages, interfaces, two-value assignments, etc. 

### No Exceptions 😉 
In Go, functions usually return a value and an error, but not limited to that alone. Functions can return multiple values. Handling errors in Go is simple, we check the second return value (or the position where the error is returned from the function). This structure gives a good way to chain errors from one point to another. 
```go
type Server struct {}

func main() {
	err := s.Run()
}

func (s *Server) Run() (err error) {
	http.HandleFunc("/", s.handleHomeRequest)
	err = http.ListenAndServe(":4000", nil)
	if err != nil {
		return err
	}
	return nil
}
```
### Variable and Declarations
In Go, all variables must be defined. We can't have an assignment like `x = 2`. 
```go
package main

import (
	"fmt"
)

func main() {
	var num int
	num = 20
	fmt.Printf("The number is %d\n", num)
}
```
We declare a variable `num` of type `int`. Go by default, assigns a **zero** value to variables. Integers are assigned **0**, booleans, `false`, strings `""`, and so on. That pattern may look like a lot of typing, Go also provides us a handy operator `:=`. 
So we can rewrite the declaration above to `num := 29`

> As long as the variable is new, `:=` can be used.

### Two-value Assignment
Interfaces in Go are used to do exactly what they were designed to do in any language, which is to logically group code that has similar behaviour, not to achieve less code. 
In Go, every type implements at least zero methods, which means each type implements a special zero `interface{}`. This is useful when we do no not know what type a variable is.
```go
var someData interface{}
err := json.Unmarshal(raw_bytes, &someData)
if err != nil {
	log.Error(err)
}
```
To get a type from this, we use Type assertion. It is a simple statement, if the assertion fails, the assigned value will have the default value of the type. Another way to do this is to use a Two-value assignment. We assign a second value in the type assertion statement which will hold a boolean stating if it was successful.
```go
jsonStruct := someData.(map[string]interface{})
num := jsonStruct["num"].(int)
str, ok := jsonStruct["str"].(string)
if !ok {
	log.Error("error converting to string")
}
```
Same applies for retrieving values from a map
```go 
data := make(map[string]int)
data["result"] = 20
val, ok := data["result"] // returns false if key isn't found
if !ok {
	log.info("Value for key not found!!")
}
```


### Goroutines 
Most of the modern programming languages like Python, Java, etc originated from the single threaded environment. Most of them supports multi-threading, but they still face some problems that comes with concurrent execution, race conditions, threading-locking and deadlocks. Those of which makes it hard to create a multi-threading application on those languages. 

In Java (from research, I don't have experience with Java), creating new threads is not memory efficient. Every thread consumes approx 1MB of memory heap size. If you start spinning thousands of threads, they will put tremendous pressure on the heap and will cause a shut down due to loss of memory. Also, Communication between two or more threads, is very difficult.

Go was released in 2009 when multi-core processors were already available. Go is built with keeping concurrency in mind. Go has `goroutines` instead of threads. They consume almost *2KB* memory from the heap. So one can spin millions of goroutines at any time.


![How Goroutines work](https://cdn-images-1.medium.com/max/1600/1*NFojvbkdRkxz0ZDbu4ysNA.jpeg "How Goroutines work - http://golangtutorials.blogspot.in/2011/06/goroutines.html")


##### Some benefits from using goroutines
- Goroutines allow you avoid having to resort to mutex locking when sharing data structures.
- Goroutines have faster startup time than threads.
- Goroutines makes use of *channels*, a built-in way used to safely communicate between themselves.
- Goroutines and OS threads do not have a 1:1 mapping. A single goroutine can run on multiple threads. Goroutines are multiplexed into small number of OS threads.

> More on Concurrency by [Rob Pike](https://blog.golang.org/concurrency-is-not-parallelism)

### Written in Go? Its easy to maintain
Go intentionally leaves out many features of modern OOP languages.
- No classes. Everything is divided into packages. Go has only structs.
- No support for inheritance. Go makes it easy to understand the code, as there's no super class to look out for.
- No Execeptions
- No generics
- No annotations
- No constructors

## Getting better
After checking all the official Go resources I was constantly on the lookout to get to know more about the Go language, I've come across awesome contents that teaches one Go. There's [Go by example](https://gobyexample.com/), [Go Book](https://golang-book.com), [Effective Go](https://golang.org/doc/effective_go.html), among others online. 
These resources helped out and is still helping me out, even though I there was some times the topic was frustrating, but it all became fun now)

## Conclusion
Go is very different from other object-oriented languages, it still has its own good sides. Its backed by Google, and is also used by some big companies like IBM, Intel, Medium, Adobe (**https://github.com/golang/go/wiki/GoUsers**).

I had programming experience before learning Go, most of the concepts wasn't new to me, Go is still very easy to start out with even if you're an absolute beginner. 

Want to share how you started out with Go, ask me questions? Let me know in the comments 🙂 
