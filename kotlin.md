Hints for Kotlin programming language


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
Extension function extends the class. It is defined outside of the class but can be called as a regular member to this class


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

# Properties

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

### Lazy and lateinit properties
```
val lazyProp : String by lazy {
  println("Kuku epta!")
  "kuku"
}
```

`lateinit var myData : SomeType` // can't be __val__ and can't be _nullable_

# OOP in Kotlin
__final__ (used by default): cannot be overriden

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

`run, let, takeIf, takeUnless, repeat`

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

## Inline functions

compiler substitutes a body of the function instead of calling it








