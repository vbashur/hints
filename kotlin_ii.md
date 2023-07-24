# Kotlin advanced topics
_the notes taken from_ [advanced Kotlin programming](https://www.oreilly.com/videos/advanced-kotlin-programming/9781491964149/)

## Functions advanced

### Infix functions

for extensions or member functions with a __single__ parameter we can apply _infix_ keyword
```
infix fun String.shouldBeEqualTo(value: String) = this ==value

// then might be called

"Hello" shouldBeEqualTo "Hello"
```

### local return 
To make particular inner function return you need to specify what exactly the return belongs to with __@__

```
fun containingFunction() {
  val nums = 1..100
  numbers.forEach {
    retrun@forEach      // without @forEach it would make a return from containingFunction
  }
}
```

The same but with custom label
```
fun containingFunction() {
  val nums = 1..100
  numbers.forEach myLabel@ {
    retrun@myLabel      // without @myLabel it would make a return from containingFunction
  }
}
```

### operator overloading
```
data class Time(val hours: Int, val minutes: Int) {
  operator fun plus(time: Time): Time {
    //...
  }
}

val newTime = Time(10, 40) + Time(3, 55)
```


### lambda extensions
Given that we have
`class Request(var query: String)`

`class RouteHandler(val request: Request, val response: Response)`


we can introduce the following lambda extension

`fun routeHandler(f: RouteHandler.() -> Unit): RouteHandler.() -> Unit = f` _// it takes an extension function of class RouteHandler as a parameter_


It helps us to write something like:
```
routeHandler {
  request.query = ...
}
```

If we add andother lambda extension
`class Request(var query: Query)`

`class Query(var body: String)`

`fun query(query: Query.() -> Unit) {}`


then we can do the following
```
routeHandler {
  request {
    query {
      body = "{ }"
    }
  }
}
```

## Classes advanced

### lateinit
_lateinit_ modifier allows for late initialisation after constructor (only on _var_)

### sealed classes

__sealed__ keyword for class restricts the inheritance of the class to its nested classes, everything that is the part of _sealed_ class hierarchy has to be defined inside the class or inside of the same file

```
sealed class PageResult {
  class Success(val content: String)
  class Error(val code: Int)
}

fun main(args: Array<String>) {
  when (pageResult) {
    is PageResult.Success -> print("OK")
    is PageResult.Error -> print("KO")
  }
}
```

### type alias
`typealias Name = String` _// declaring alias to type String_


## Delegation

### observable delegate (property change detection)
this delegate roughly speaking looks as follows:
`observable(initialValue, onChange(KProperty<*>, T, T) -> Unit)`
it lets us to identify property change
```
data class Employee(val name: String) {
  var department: String by Delegates.observable("", {property, oldValue, newValue -> println("$property value was changed from $oldValue to $newValue")})
}
```

### vetoable delegate (property change restriction)
this delegate roughly speaking looks as follows:

it lets us to restrict property change
```
//the employee could only change the department to 'Analytics' or 'Supply', otherwise will be set to default 'IT'
data class Employee(val name: String) {
  var department: String by Delegates.vetoable("IT") {property, oldValue, newValue -> newValue.equals("Analytics") || newValue.equals("Supply")}
}
```

### extension properties

```
val String.hasAmpersand: Boolean
  get() = this.contains("&")
...
println("Hello".hasAmpersand()) // returns false

```

## Async programming
standard approaches
* threading
* async/await pattern  //_default C# approach_
* futures
* promises
* reactive extensions
* callbacks


Kotlin is offering __coroutines__

__Coroutines__ are light-weight threads that allow you to write asynchronous non-blocking code. Kotlin provides the kotlinx. coroutines library with a number of high-level coroutine-enabled primitives

__building blocks__
* coroutine builders - function that defines a controller, taking coroutine as a block of code
* controllers - defines suspension points, how to return values are processed, exceptions are handled and all in general the coroutines behaviour
* suspension functions - specially marked functions where suspension points occur

