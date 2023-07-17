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

