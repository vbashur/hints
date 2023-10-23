
# Recipies taken from the 'kotlin-cookbook' by Ken Kousen


## 3.1 property validation
How to validate variable before setting its value
```
var priority = validPriority(_priority)       
        set(value) {
            field = validPriority(value)
        }

private fun validPriority(p: Int) =           
        p.coerceIn(MIN_PRIORITY, MAX_PRIORITY)
```

## 3.2 define getter-setter 
the format is:
```
var <propertyName>[: <PropertyType>] [= <property_initializer]
    [<getter>]
    [<setter>]
```

## 3.4 apply 'lazy' to enforce initialization of a property
using a private backing field to enforce initialization of a property is a useful technique
```
class Customer(val name: String) {

    val messages: List<String> by lazy { loadMessages() }  

    private fun loadMessages(): MutableList<String> =
        mutableListOf(
            "Initial contact",
            "Convinced them to use Kotlin",
            "Sold training class. Sweet."
        ).also { println("Loaded messages") }
}
```
## 3.6 using lateinit for delayed initialization
The purpose: you don’t have enough information to initialize a property in a constructor, but you don’t want to have to make the property nullable as a result.
Example from spring framework:
```
@Autowired
lateinit var client: WebTestClient    
```
The `lateinit` modifier can be used only on `var` properties declared inside the body of a class, and only when the property does not have a custom getter or setter.
Since Kotlin 1.2, you can also use `lateinit` on top-level properties and even local variables. The type must be non-null, and it cannot be a primitive type

## 4.1 & 4.2 fold vs reduce

Use `fold` to reduce a sequence or collection to a single value 
```
inline fun <R> Iterable<T>.fold(
    initial: R,
    operation: (acc: R, T) -> R
): R
```

Use `reduce` to reduce on a non-empty collection of values, but don’t need to set an initial value for the accumulator

`reduce` does not have an argument that provides an initial value for the accumulator. The accumulator is therefore initialized with the first value from the collection
```
inline fun <S, T : S> Iterable<T>.reduce(
    operation: (acc: S, T) -> S
): S
```

## 5.4 associate
Use `associate` to generate a map from the set of keys, each key might be associated with dedicated value
```
val keys = 'a'..'f'
val map = keys.associate { it to it.toString().repeat(5).capitalize() }
println(map) // {a=Aaaaa, b=Bbbbb, c=Ccccc, d=Ddddd, e=Eeeee}
```

## 5.7 split the list on smaller windows
Use `chunked` or `windowed` with the window size

**chunked**
```
val range = 0..10
val chunked = range.chunked(3)
assertThat(chunked, contains(listOf(0, 1, 2), listOf(3, 4, 5), listOf(6, 7, 8), listOf(9, 10)))
```
**windowed**
```
assertThat(range.windowed(3, 1),
        contains(
            listOf(0, 1, 2), listOf(1, 2, 3), listOf(2, 3, 4),
            listOf(3, 4, 5), listOf(4, 5, 6), listOf(5, 6, 7),
            listOf(6, 7, 8), listOf(7, 8, 9), listOf(8, 9, 10)))
```

## 7.3 using the let Function and Elvis
use `let` with elvis operator when you want to execute a block of code only on a non-null reference, but return a default otherwise
```
fun processNullableString(str: String?) =
    str?.let {          
        when {
            it.isEmpty() -> "Empty"
            it.isBlank() -> "Blank"
            else -> it.capitalize()
        }
    } ?: "Null"         
```

