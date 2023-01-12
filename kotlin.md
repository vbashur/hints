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

