## Kotlin for Java programmers


### The Basics

```kotlin
/*
  Kotlin does not need ";" to end statements
  No wrapping class
  No arguments
  No static modifier
 */
fun main() {
    println("Welcome to the Madrid JUG")     
}
```

```kotlin
/*
  Kotlin has no primitive data types. EVERYTHING is an object.
 */
fun sumTwoNumbers(firstNumber: Int, secondNumber: Int): Int {
    return firstNumber + secondNumber
}
```

```kotlin
/*
 Assignments are NOT expressions in Kotlin
*/
var n = m = getNumber()     // Compile error
```

**String interpolation**
```kotlin
fun greetUser() {
    val name = readln()
    println("Hello, ${if (name.isBlank()) "someone" else name}!")   // An expression is evaluated insude the argument to println
}
```

```kotlin
fun comparisonAndEquality() {
    val num1 = 1000
    val num2 = 1000
    println(num1 == num2) // true
    println(num1.equals(num2)) // true    // we get a warning, because "equals" does not allow the operands to be null
    // If you do want to check for reference equality, use === . This won't work for some of the basic types:
    println(num1 === num2) // Still true
}
```

**Functions**
```kotlin
fun welcomeUser(message: String) {   // the function parameter can't be modified
    return "Hello, $message"
}
```

```kotlin
fun main() {
    println(welcomeUser("Welcome to the Madrid JUG"))
}
```

```kotlin
/* A function as an expression */
fun welcomeUser(message: String) = "Hello, $message"
```

**COLLECTIONS**
```kotlin
fun collections() {
    println("========== Collections ============")
    val hobbits = listOf("Frodo", "Sam", "Pippin", "Merry")   // List.of() in Java, since version 9
    println("List: $hobbits")
    val uniqueHobbits = setOf("Frodo", "Sam", "Pippin", "Merry", "Frodo")  // Set.of() in Java, since version 9
    println("Set: $uniqueHobbits")
    val movieBatmans = mapOf(                      // Map.of() and Map.ofEntries() in Java, since version 9
        "Batman Returns" to "Michael Keaton",
        "Batman Forever" to "Val Kilmer",
        "Batman & Robin" to "George Clooney"
    )
    println(movieBatmans)
}
```

```kotlin
fun mutableCollections() {
    println("========== Mutable Collections ============")
    val editableHobbits = mutableListOf(
        "Frodo", "Sam",
        "Pippin", "Merry"
    )
    editableHobbits.add("Bilbo")
    println(editableHobbits)
}
```

**IF in Kotlin**

```kotlin
/* if is a statement here, like in Java */
fun getUnixSocketPollingAsIfItWereJava(isBsd: Boolean): String {
        var pollingType = "ePoll"
        if (isBsd) {
            pollingType = "kqueue"
        }
        return pollingType
}
```

```kotlin
/* if is an expression here */
fun getUnixSocketPolling(isBsd: Boolean): String {
        return if (isBsd) {
            "kqueue"
        } else {
            "epoll"
        }
}
```

```kotlin
/* if expression in only one line */
fun getUnixSocketPollingInOneLine(isBsd: Boolean) = if (isBsd) "kqueue" else "epoll"
```

**Pattern Matching**
```kotlin
interface Expr
class Num(val value: Int) : Expr    // ":" is equivalent to Java "implements"
class Sum(val left: Expr, val right: Expr) : Expr

/*
 Using "when" instead of if/else
*/
fun eval(e: Expr): Int =
    when(e) {
        is Num -> e.value
        is Sum -> eval(e.right) + eval(e.left)
        else -> throw IllegalArgumentException("Non-valid expression")
    }
```

