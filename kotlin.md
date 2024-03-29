Hints for Kotlin programming language

- [Operators](#operators)
  * [Conditional](#conditional)
    + [when](#when)
  * [Loop](#loop)
    + [for](#for)
  * ['in' check ranges](#-in--check-ranges)
- [Extension functions](#extension-functions)
- [Nullability](#nullability)
- [Safe cast](#safe-cast)
- [Lambdas](#lambdas)
  * [Closures](#closures)
  * [Common operations](#common-operations)
    + [map[key] vs map.getValue(key)](#map-key--vs-mapgetvalue-key-)
    + [Function types](#function-types)
    + [Member reference](#member-reference)
- [Properties / Constructors / Constants](#properties---constructors---constants)
    + [setter](#setter)
    + [Lazy and lateinit properties](#lazy-and-lateinit-properties)
    + [Constants](#constants)
- [Enum](#enum)
- [OOP in Kotlin](#oop-in-kotlin)
  * [Class modifiers](#class-modifiers)
  * [Conventions](#conventions)
- [Useful libraby functions](#useful-libraby-functions)
  * [Inline functions](#inline-functions)
  * [Unlimited parameters in function](#unlimited-parameters-in-function)
  * [Anonymous functions](#anonymous-functions)
- [Kotlin advanced topics](#kotlin-advanced-topics)
- [Object declaration](#object-declaration)
  * [Singleton](#singleton)
  * [Companion, nested and inner classes](#companion--nested-and-inner-classes)
      - [Companion object](#companion-object)
      - [nested vs inner](#nested-vs-inner)
- [Collection processing](#collection-processing)
  * [fold](#fold)
  * [map vs flatMap](#map-vs-flatmap)
- [Misc](#misc)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



# Operators

## Conditional

### when

Simple `when`
```
when (color) {
   BLUE -> println("blue")
   RED -> println("red")
   else -> println("i donno")
}
```

Check for several values

```
when (color) {
   BLUE,BLACK -> println("dark")
   WHITE,YELLOW -> println("light")
   else -> println("i donno")
}
```

Check for types
```
when (animal) {
  is Cat -> print("meow")
  is Dog -> print("bark")
}
```



Check for types wth capturing
```
when (val animal = getSomePet()) {
  is Cat -> animal.meow()
  is Dog -> animal.bark()
}
```

*when* without arguments
```
val somepair = when {
  somevalue < 5 -> "less" to 5
  somevalue > 4 -> "greater" to 4
}
```

## Loop

### for

*for* iterating through map
```
for ((k,v) in somemap) {
  println("$k is mapped to $v")
}
```

iterate with index

```
for ((iter,el) in someList.withIndex()) {
  println("element $el has index $iter")
}
```

iterate in range
```
for (iter in 0..3)              // prints 0,1,2,3
for (iter in 0 until 3)         // prints 0,1,2
for (iter in 4 downto 0 step 2) // prints 4,2,0
```

## 'in' check ranges
```
'c' in 'a'..'z'
```
```
when (val c = getSomeChar()) {
  c in '0'..'9' -> println("it is digit")
  c in 'a'..'z' -> println("it is lowercase letter")
}
```

# Extension functions
* Extension function extends the class. It is defined outside of the class but can be called as a regular member to this class

*e.g. type String is a **receiver***
```
fun String.lastChar() = this.get(this.length -1)

// getOrNull extension function sample for Array
fun <T> Array<T>.getOrNull(index: Int) = 
   if (inde in 0 until this.size) this[index] else null

// isLetter extension function sample for Char
fun Char.isLetter() = this in 'a'..'z' || this in 'A'..'Z'

```

Sample of using toRegx extenstion function
```
val regex = """\d{4}-\d{2}-\d{2}""".toRegex()
```
* Extension functions are statically resolved: if we have ext functions with identical signature for base and derived classes we'll get the function called that bound to the declared type of the target object, e.g.
```
val obj : Base = Derived()
obj.callExtension() // will be called extension function declared at Base class
```

* If a class defines a member function with the same name as an extension function, the member function __always takes precedence__ over the extension function.

# Nullability
How to understand nullable
```
val l = s?.length // if (s != null) s.length else null
```

How to use 'elvis' operator
```
val a: Int? = null 
val c: Int = 2

val s1 = (a ?: 0) + c // prints 2 
```

Not-null assertion (`!!`): throws NPE if for `foo!!` when `foo` is null


# Safe cast
```
if (any is String) {
  any.toUpperCase
} 
// OR
(any as? String)?.toUpperCase()   // if(any is String) any else null
```
# Lambdas
```
list.any { it > 0 }  // 'it' denotes the argument if it's only one
```

If some lambda parameter is not used it could be omitted with *_*
```
map.mapValues { (_, value) -> "$value!" } 
```

## Closures
_A closure is a function that has access to variables that are defined in the outer scope_

Example
```
   val number = 10
   unaryOperation(20, { x -> x * nunmber }) // number is captured by closure
```

## Common operations
* map
* any (all, none)
* find (first / firstOrNull)
* count
* partition
* groupBy
* associateBy
* associate // build the map based on a list, eg { 'a' + it to 10 * it }
* zip //organize a couple of lists
* flatten
* flatMap


### map[key] vs map.getValue(key)
```
mapByName["unknown"]?.age           // null
mapByName.getValue("unknown").age   // NoSuchElementException
```

### Function types

`run { println("hey scuko") }` <- call lambda function

### Member reference
How to use function reference
```
fun isEven(i: Int): Boolean = i % 2 == 0
val predicate = ::isEven
```

# Properties / Constructors / Constants

```
class Person(val name: String, var age: Int) // === getName, getAge, setAge
```
Backing field might be absent 
```
class Rectangle(val height: Int, val width: Int) {
  val isSquare: Boolean
   get() {
     return height == width
   }
}
```
How to define mutliple constructors to init various properties
```
class Person(var name: String, var age: Int) {
   init {
      name = name.toUppercase()
   }
   constructor(email: String): this("", 23) {

   }
}
```
### setter
```
class Person(var name: String, var age: Int) {
   var socialId : String = ""
   set (socId) {
      if (!socId.startsWith("SN)) throw...
      field = socId
   }
}

```


### Lazy and lateinit properties
```
val lazyProp : String by lazy {
  println("Kuku epta!")
  "kuku"
}
```

`lateinit var myData : SomeType` // can't be __val__ and can't be _nullable_

### Constants
constants can be declared within __object__ class
```
object Copyright {
   val author = "Mark Twain"
}
```
or like separate variable
```
val author = "Mark Twain"
```

# Enum

Sample of enum with an abstract method
```
enum class Priority(val value: Int) {
   MINOR(-1) {
      override fun text(): String {
         throw UnsupportedOperationException("not implemented in MINOR")
      }
   }

   MAJOR(1) {
      override fun text(): String {
         throw UnsupportedOperationException("not implemented in MAJOR")
      }
   }

   abstract fun text(): String
}
```

# OOP in Kotlin
__final__ (used by __default__): cannot be overriden // by default all classes and its methods are marked final

__open__: can be overriden

__abstract__: must be overriden

__override__: mandatory for cases when override be intention

__internal__: visible in module only

__protected__: visible for subclasses (not in package unlike java)


One file could contain several classes and top-level declarations


## Class modifiers
__data__ modifier generates usefule methods _equals_, _hashCode_, _copy_, _toString_

`set1 == set2` - calls equals
`set1 === set2` - checks reference equality

__sealed__ class modifier is allowed when we assured that we know our class hierarchy and we don't let this this class be modified anymore, i.e. we restrict class hierarchy


static nested class is the __default__ behavior in kotlin, if youn need reference to outer class use __inner__ class modifier

keyword __object__ is used to declare singleton classes

To use the methods of the singleton class you call: `ClassName.INSTANCE.foo()`


__obejct__ replaces Java's anonymous clases, e.g.
```
window.addListener(
    object : MouseClick() {
        override fun onClick(e: MouseEvent) { ... }
    }
)
```

__companion object__ - special object inside of a class (there is no _static_ methods in Kotlin, that's why companion objects)
```
class A {
    companion object {
        fun foo() = 1
    }
}
...
A.foo()
```
* companion object might implement an interface (`companion object : Factory<A>`)
* companion object can be a receiver of extension function: `fun Person.Companion.fromJSON`
* companion object could only be one per a class


## Conventions

`s1 == s2` // by default calls 'equals' under the hood and handles nulls

__in__ convention
```
if (key in map) {}        // map.contaknsKey(key) under the hood
if (element in list) {}

if (s in "abc".."def") {} // 
for (s in start..end) {}  // start.rangeTo(end)
```

# Useful libraby functions

`run, let, takeIf, takeUnless, repeat, with`

```
// runs the block of code (lambda) and returns the last expression as the result
val foo = run { 
   println("Calculating foo...")
      "foo"
}
```

__let__ allows to check the argument for being non-null, not only the receiver
```
  fun getEmail(): Email?
  val email = getEmail()
  if (email != null) sendEmailTo(email)
  ..==..
  email?.let { e -> sendEmailTo(e) }
  getEmail()?.let { sendEmailTo(it) }
```

__takeif__ returns the receiver object if it satisfies the given predicate, otherwise returns null

Example:
```
val number = 42
number.takeIf { it > 10 } // 42
...
val number = 42
number.takeIf { it > 70 } // null

```

__takeUnless__ returns the receiver object if it _does not_ satisfy the given predicate, otherwise returns null


__repeat__ repeats an action for a given number of times

```
repeat(10) {
  println("Welcome!")
}
```

__with__ calls the specified function with the given receiver

```
with (file) {
   println(isAbsolute) // we can refernece the methods/properties without using the object as it's specified at 'with'
}
```

## Inline functions

compiler substitutes a body of the function instead of calling it

## Unlimited parameters in function

__vararg__ - keyword for making function to accept unlimited number of parameters

```
fun printString(varagr strings: String) {
  reallyPrintStrings(*strings)
}

fun reallyPrintStrings(vararg strings: String) {
  for (string in strings) {
    print(string)
  }
}

```

## Anonymous functions
```
   fun unaryOperation(x: Int, op: (Int) -> Int)
   ...
   unaryOperation(3, fun(x: Int): Int { return x * x })
```

# Kotlin advanced topics
_the notes taken from_ [Kotlin coursera classes](https://www.coursera.org/learn/advanced-programming-in-kotlin)

# Object declaration

## Singleton
```
object Company { // no parenthesis on declaration
    
}

```

## Companion, nested and inner classes

#### Companion object
Companion objects are used in Kotlin to store static data or to provide static methods. Companion object is associated with a class. A class may have only one companion object.

`companion` objects usually located at the bottom of the class - this is a code convention

```
class Contest {
  companion object Registration { // companion object name is optional here as the class can have only one companion object
    fun doApply() : TODO
  }
}

```

```
// companion object usage example https://pl.kotl.in/Ffd6_Z7KC
/**
 * You can edit, run, and share this code.
 * play.kotlinlang.org
 */
fun main() {
    println("Hello, world!!!")        
    val soupPrice: Double = 7.8
    val pastaPrice: Double = 12.9
    val orders = listOf<OrderItem>(OrderItem("soup", soupPrice), OrderItem("pasta", pastaPrice))
    val taxAmount : Double = TaxCalculator.getTaxAmountForOrders(orders) 
    println("Tax amount : $taxAmount")
}

class OrderItem (
    val name : String,
    val price : Double) {}


class TaxCalculator {
    companion object {
        val taxPercentage : Double = 0.13
        fun getTaxAmountForOrders(orderItems : List<OrderItem>) : Double {
            var orderAmount : Double = 0.00
            for (item in orderItems) {
                val currentOrderTaxAmount : Double = item.price * taxPercentage
                orderAmount += currentOrderTaxAmount
            }
            return orderAmount
        }

    }
}
```


#### nested vs inner
Nested classes cannot access field of outer classes but inner classes can do it
```
// NESTED
class Order { 
    val orderId = 1 

    class DeliveryDetails {  // nested class
        val deliveryFeeInDollars = 10 
    } 

} 
```
To initialize an obejct of nested class one writes `Order.DeliveryDetails()`

```
// INNER
class Order(val orderAmount: Int) { 

    inner class TaxDetails { 
        val taxAmount = 0.05 * orderAmount 
    } 

}
```
To initialize an obejct of inner class we need an instance of the outer classs, e.g. `Order().TaxDetails()`

# Collection processing

## fold 
Very usefule if we want aggregate the elements of the collection

```
// `fold` implementation from Kotlin stdlib
inline fun <T, R> Iterable<T>.fold(
    initial: R,
    operation: (acc: R, T) -> R
): R {
    var accumulator = initial
    for (element in this) {
        accumulator = operation(accumulator, element)
    }
    return accumulator
}
```

## map vs flatMap
how we'd describe the difference:
```map
[a, b, c] f(x) => [f(a), f(b), f(c)]
```

```flatMap
[[a, b], [c, d]] f(x) => [f(a), f(b), f(c)]
```





# Misc

generate selfcontained jar file with kotlin app
```
kotlinc Main.kt -include-runtime -d hello.jar
//and then
java -jar hello.jar
```






