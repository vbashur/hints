
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

