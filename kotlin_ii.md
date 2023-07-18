# Kotlin advanced topics
_the notes taken from_ [advanced Kotlin programming](https://www.oreilly.com/videos/advanced-kotlin-programming/9781491964149/)

# Functions advanced

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
