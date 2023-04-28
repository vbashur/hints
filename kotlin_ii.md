# Kotlin advanced topics
_the notes taken from_ [Kotlin coursera classes](https://www.coursera.org/learn/advanced-programming-in-kotlin)

### Object declaration

#### Singleton
```
object Company { // no parenthesis on declaration
    
}

```

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

