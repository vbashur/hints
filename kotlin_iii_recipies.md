
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

## 7.3 using the let function and elvis
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

use 'let' function reference
```
numbers.map { it.length }.filter { it > 3 }.let(::println)
```

## 8.3 ensure that value is not a null

use the notNull function to provide a delegate that throws an exception, if the value has not been set
Kotlin will throw an `IllegalStateException`
```
var shouldNotBeNull: String by Delegates.notNull<String>()
```
how to test it
```
@Test
fun `uninitialized value throws exception`() {
    assertThrows<IllegalStateException> { shouldNotBeNull }
}
```

## 8.4 use observable and vetoalble delegates
`observable` helps to detect changes

`vetoable` helps to decide whether to implement changes

```
fun <T> observable(
    initialValue: T,
    onChange: (property: KProperty<*>, oldValue: T, newValue: T) -> Unit
): ReadWriteProperty<Any?, T>

fun <T> vetoable(
    initialValue: T,
    onChange: (property: KProperty<*>, oldValue: T, newValue: T) -> Boolean
): ReadWriteProperty<Any?, T>
```

examples of use:
```
var watched: Int by Delegates.observable(1) { prop, old, new ->
    println("${prop.name} changed from $old to $new")
}

var checked: Int by Delegates.vetoable(0) { prop, old, new ->
    println("Trying to change ${prop.name} from $old to $new")
    new >= 0
}
....
@Test
fun `watched variable prints old and new values`() {
    assertEquals(1, watched)
    watched *= 2
    assertEquals(2, watched)
    watched *= 2
    assertEquals(4, watched)
}
....
watched changed from 1 to 2
watched changed from 2 to 4
```

## 9.4 parametrized tests

example of csv data source
```
ParameterizedTest
@CsvSource("1, 1", "2, 1", "3, 2",
    "4, 3", "5, 5", "6, 8", "7, 13",
    "8, 21", "9, 34", "10, 55")
fun `first 10 Fibonacci numbers (csv)`(n: Int, fib: Int) =
     assertThat(fibonacci(n), `is`(fib))
```

example of the method source
```
private fun fibnumbers() = listOf(
    Arguments.of(1, 1), Arguments.of(2, 1),
    Arguments.of(3, 2), Arguments.of(4, 3),
    Arguments.of(5, 5), Arguments.of(6, 8),
    Arguments.of(7, 13), Arguments.of(8, 21),
    Arguments.of(9, 34), Arguments.of(10, 55))

@ParameterizedTest(name = "fibonacci({0}) == {1}")
@MethodSource("fibnumbers")
fun `first 10 Fibonacci numbers (instance method)`(n: Int, fib: Int) =
    assertThat(fibonacci(n), `is`(fib))
```

example of applying data class
```
data class FibonacciTestData(val number: Int, val expected: Int)

@ParameterizedTest
@MethodSource("fibonacciTestData")
fun `check fibonacci using data class`(data: FibonacciTestData) {
    assertThat(fibonacci(data.number), `is`(data.expected))
}

private fun fibonacciTestData() = Stream.of(
    FibonacciTestData(number = 1, expected = 1),
    FibonacciTestData(number = 2, expected = 1),
    FibonacciTestData(number = 3, expected = 2),
    FibonacciTestData(number = 4, expected = 3),
    FibonacciTestData(number = 5, expected = 5),
    FibonacciTestData(number = 6, expected = 8),
    FibonacciTestData(number = 7, expected = 13)
)
```

## 10.1 use and useLines
Kotlin adds the extension functions `use` to `Closeable` and `useLines` to Reader` and File.

```
fun get10LongestWordsInDictionary() =
    File("/usr/share/dict/words").useLines { line ->
        line.filter { it.length > 20 }
            .sortedByDescending(String::length)
            .take(10)
            .toList()
    }
```

## 10.5 write to a file
write to a file and replace all of its existing contents `writeText`

`appendText` is an extenstion function that can add data to a given file

```
File("myfile.txt").writeText("My data")
```

writing to a file with `use`
```
File(fileName).printWriter().use { writer ->
    writer.println(data) }
```

## 11.2 executing lambda repeatedly
use the buiilt-in `repeat` function that takes int parameter which is a number of repeats
```
fun main(args: Array<String>) {
    repeat(5) {
        println("Counting: $it")
    }
}
```

## 11.8 starting a thread
below is the extension function called `thread` 
```
fun thread(
    start: Boolean = true,
    isDaemon: Boolean = false,
    contextClassLoader: ClassLoader? = null,
    name: String? = null,
    priority: Int = -1,
    block: () -> Unit
): Thread
```

example of initiating a thread
```
(0..5).forEach { n ->
    val sleepTime = Random.nextLong(range = 0..1000L)
    thread {
        Thread.sleep(sleepTime)
        println("${Thread.currentThread().name} for $n after ${sleepTime}ms")
    }
}

// initiating a daemon thread

(0..5).forEach { n ->
    val sleepTime = Random.nextLong(range = 0..1000L)
    thread(isDaemon = true) {      1
        Thread.sleep(sleepTime)
        println("${Thread.currentThread().name} for $n after ${sleepTime}ms")
    }
}
```

## 12.3 injecting properties with spring
4 ways of injecting properties
```
@RestController   // 1 Class with a single constructor
class GreetingController(val service: GreetingService) { /* ... */ }

@RestController   // 2 Explicit autowiring
class GreetingController(@Autowired val service: GreetingService) { /* ... */ }

@RestController   // 3 Autowiring constructor call, primarily for classes with multiple dependencies
class GreetingController @Autowired constructor(val service: GreetingService) {
    // ... (normal 4-space indent)
}

@RestController   // 4 Field injection (not preferred, but can be useful)
class GreetingController {
    @Autowired
    lateinit var service: GreetingService

    // ... rest of class ...
}
```

## 13.1 using coroutines with runBlocking
sample of `runBlocking`

```
import kotlinx.coroutines.delay
import kotlinx.coroutines.runBlocking

fun main() {
    println("Before creating coroutine")
    runBlocking {
        print("Hello, ")
        delay(200L)
        println("World!")
    }
    println("After coroutine is finished")
}

// output is
Before creating coroutine
Hello, World! // 200 ms delay between hello and world 
After coroutine finished
```

sample of `launch` function
```
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.runBlocking

fun main() {
    println("Before runBlocking")
    runBlocking {                 
        println("Before launch")
        launch {                  
            print("Hello, ")
            delay(200L)
            println("World!")
        }
        println("After launch")
    }
    println("After runBlocking")
}
```

## 13.3 using dispatcher for coroutine contexts
dispatcher determines which thread or thread pool the coroutines use for their execution

Built-in dispatchers provided by the library include the following:
* Dispatchers.Default
* Dispatchers.IO
* Dispatchers.Unconfined

Example:
```
fun main() = runBlocking<Unit> {
    launchWithIO()
    launchWithDefault()
}

suspend fun launchWithIO() {
    withContext(Dispatchers.IO) { // IO dispatcher
        delay(100L)
        println("Using Dispatchers.IO")
        println(Thread.currentThread().name)
    }
}

suspend fun launchWithDefault() {
    withContext(Dispatchers.Default) { // default dispatcher
        delay(100L)
        println("Using Dispatchers.Default")
        println(Thread.currentThread().name)
    }
}
```

## 13.5 cancelling the job
```
fun main() = runBlocking {
    val job = launch {
        repeat(100) { i ->
            println("job: I'm waiting $i...")
            delay(100L)
        }
    }
    delay(500L)
    println("main: That's enough waiting")
    job.cancel()
    job.join()
    println("main: Done")
}
```








