
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
