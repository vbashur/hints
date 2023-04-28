# Kotlin advanced topics
_the notes taken from_ [Kotlin coursera classes](https://www.coursera.org/learn/advanced-programming-in-kotlin)

### Object declaration

#### Singleton
```
object Company { // no parenthesis on declaration
    
}

```

#### Companion, nested and inner classes

##### Companion object
Companion objects are used in Kotlin to store static data or to provide static methods. Companion object is associated with a class. A class may have only one companion object.

`companion` objects usually located at the bottom of the class - this is a code convention

```
class Contest {
  companion object Registration { // companion object name is optional here as the class can have only one companion object
    fun doApply() : TODO
  }
}

```
##### nested vs inner
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
```
// INNER
class Order(val orderAmount: Int) { 

    inner class TaxDetails { 
        val taxAmount = 0.05 * orderAmount 
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

### Extension functions
If a class defines a member function with the same name as an extension function, the member function __always takes precedence__ over the extension function.


