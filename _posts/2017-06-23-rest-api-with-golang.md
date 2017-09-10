---
title: "RESTful APIs with Go"
layout: post
author: "@codehakase"
---
![golang-api](https://user-images.githubusercontent.com/9336187/27503566-f8079884-5874-11e7-9dab-42b287a16d75.png)

**Go** is an open source programming language that makes it easy to build simple, reliable, and efficient software.
Go was created at Google in 2007 by Robert Grisemer, Rob Pike and Ken Thompson. Go is a compiled, statically typed language.
> PS: This article is not an introduction to the Go programming language, its an intermediate one.

For the purpose of this tutorial, we'll be creating a RESTful API for a Phone Book app.

## REST, RESTful, WTH!!
**REST** is an acronym for Representational State Transfer. It is a web standards architecture and HTTP Protocol. The REST protocol, decribes six (6) constraints:
1. Uniform Interface
2. Cacheable
3. Client-Server
4. Stateless
5. Code on Demand
6. Layered System

REST is composed of methods such as a base URL, media types, etc. RESTful applicaitons uses HTTP requests to perform the CRUD operations.

## Setup
To start up writing our API, we need to setup our environment. Get the Latest Release of Go (as of this writing, its *1.8.3* ) > [here](https://golang.org). Once you've installed Go, you should have a `$GOPATH` sys variable set.
We need a Project directory to store our source, we make one:
```shell
$ mkdir -p $GOPATH/src/github/{your username}/rest-api
```
The command, will create a `rest-api` project folder in your Go home.
```shel
$ cd rest-api
```
Next we create a `main.go` file

### Router
We'll need to use a `mux`  to route requests, so we need a Go package for that (**mux** stands for *HTTP request multiplexer* which matches an incoming request to against a list of routes (registered)). In the `rest-api` directory, lets require the dependency (package rather).
```shell
rest-api$ go get github.com/gorilla/mux
```
We set up our `main.go` file
```go
package main

import (
    "encoding/json"
    "log"
    "net/http"
    "github.com/gorilla/mux"
)

// our main function
func main() {
    router := mux.NewRouter()
    log.Fatal(http.ListenAndServe(":8000", router))
}
```
If you run this file, it doesn't really respond to any , so we need to write our Endpoints.
For the phonebook api, we'd need these s:
* `/people` (GET) -> All persons in the phonebook document (database)
* `/people/{id}` (GET) -> Get a single person
* `/people/{id}` (POST) -> Create a new person
* `/people/{id}` (DELETE) -> Delete a person

We should write the routes handle for those s, and their methods too
```go
// fun main()
func main() {
    router := mux.NewRouter()
    router.HandleFunc("/people", GetPeople).Methods("GET")
    router.HandleFunc("/people/{id}", GetPerson).Methods("GET")
    router.HandleFunc("/people/{id}", CreatePerson).Methods("POST")
    router.HandleFunc("/people/{id}", DeletePerson).Methods("DELETE")
    log.Fatal(http.ListenAndServe(":8000", router))
}
```
Let's run our program
```shell
rest-api$ go build
rest-api$ ./rest-api
```
The above would return an error, we haven't created the functions to handle those requests. Lets create them (blank for now):
```go
...
func GetPeople(w http.ResponseWriter, r *http.Request) {}
func GetPerson(w http.ResponseWriter, r *http.Request) {}
func CreatePerson(w http.ResponseWriter, r *http.Request) {}
func DeletePerson(w http.ResponseWriter, r *http.Request) {}
...
```
Building the file again shouldn't return any errors, but the functions are empty.
Each  methods, take two parameters `w` and `r` which are of types `http.ResponseWriter` and `*http.Request`. These two parameters are populated once hit by a request.

### Pull all data from the phonebook
Lets write the code to get all records **tires screech>>>** We don't have any data... yeah, lets populate manually (databases are out of scope for this tutorial). Lets add a `Person` struct (think of it as an object) and an `Address` struct, and a people variable to populate with.
```go
//main.go
...
type Person struct {
    ID        string   `json:"id,omitempty"`
    Firstname string   `json:"firstname,omitempty"`
    Lastname  string   `json:"lastname,omitempty"`
    Address   *Address `json:"address,omitempty"`
}
type Address struct {
    City  string `json:"city,omitempty"`
    State string `json:"state,omitempty"`
}

var people []Person
...
```
With our structs ready, lets manually add records to the `people` slice:
```go
// main()
...

people = append(people, Person{ID: "1", Firstname: "John", Lastname: "Doe", Address: &Address{City: "City X", State: "State X"}})
people = append(people, Person{ID: "2", Firstname: "Koko", Lastname: "Doe", Address: &Address{City: "City Z", State: "State Y"}})
people = append(people, Person{ID: "3", Firstname: "Francis", Lastname: "Sunday"})

...
```
With our data set, lets fetch all from the people slice:
```go
//main.go
...

func GetPeople(w http.ResponseWriter, r *http.Request) {
    json.NewEncoder(w).Encode(people)
}

...
```
Rebuilding (`go build && ./rest-api ` ) and testing with postman we'd get a JSON response. Navigate to `localhost:8080/people`

### Get a single data
Now lets write the  to show a single person:
```go
// main.go
...

func GetPerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    for _, item := range people {
    if item.ID == params["id"]
}
}

...
```
The `GetPerson` function here loops through the mapped `names` from the incoming request, to check if the `id` params sent matches any in the Person struct.

### Other Endpoints
Lets finish our other s (`CreatePerson`, `DeletePerson`)
```go
func CreatePerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    var person Person
    _ = json.NewDecoder(r.Body).Decode(&person)
    person.ID = params["id"]
    people = append(people, person)
    json.NewEncoder(w).Encode(people)
}
func DeletePerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    for index, item := range people {
        if item.ID == params["id"] {
            people = append(people[:index], people[index+1]...)
            break
    }
    json.NewEncoder(w).Encode(people)
}
}
```

## Putting it all together
Now our Final `main.go` file should look as such:
```go
package main

import (
    "encoding/json"
    "github.com/gorilla/mux"
    "log"
    "net/http"
)

// The person Type (more like an object)
type Person struct {
    ID        string   `json:"id,omitempty"`
    Firstname string   `json:"firstname,omitempty"`
    Lastname  string   `json:"lastname,omitempty"`
    Address   *Address `json:"address,omitempty"`
}
type Address struct {
    City  string `json:"city,omitempty"`
    State string `json:"state,omitempty"`
}

var people []Person

// Display all from the people var
func GetPeople(w http.ResponseWriter, r *http.Request) {
    json.NewEncoder(w).Encode(people)
}

// Display a single data
func GetPerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    for _, item := range people {
        if item.ID == params["id"] {
            json.NewEncoder(w).Encode(item)
            return
        }
    }
    json.NewEncoder(w).Encode(&Person{})
}

// create a new item
func CreatePerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    var person Person
    _ = json.NewDecoder(r.Body).Decode(&person)
    person.ID = params["id"]
    people = append(people, person)
    json.NewEncoder(w).Encode(people)
}

// Delete an item
func DeletePerson(w http.ResponseWriter, r *http.Request) {
    params := mux.Vars(r)
    for index, item := range people {
        if item.ID == params["id"] {
            people = append(people[:index], people[index+1:]...)
            break
        }
        json.NewEncoder(w).Encode(people)
    }
}

// main function to boot up everything
func main() {
    router := mux.NewRouter()
    people = append(people, Person{ID: "1", Firstname: "John", Lastname: "Doe", Address: &Address{City: "City X", State: "State X"}})
    people = append(people, Person{ID: "2", Firstname: "Koko", Lastname: "Doe", Address: &Address{City: "City Z", State: "State Y"}})
    router.HandleFunc("/people", GetPeople).Methods("GET")
    router.HandleFunc("/people/{id}", GetPerson).Methods("GET")
    router.HandleFunc("/people/{id}", CreatePerson).Methods("POST")
    router.HandleFunc("/people/{id}", DeletePerson).Methods("DELETE")
    log.Fatal(http.ListenAndServe(":8000", router))
}

```

## Testing
To test out yor API, either view them from a browser (you'd need a JSON formatter extension, to read the output clearly) or use [Postman](https://getpostman.com).

> Source for this tutorial can be found [here](https://github.com/HakaseLabs/source-blog/tree/master/rest-api)

## Conclusion
You just saw how to build a very simple RESTful API using the Go programming language.  While we used mock data instead of a database, we saw how to create endpoints that do various operations with JSON data and GoLang slices.
Did I miss anything important? Let me know in the comments.

:)