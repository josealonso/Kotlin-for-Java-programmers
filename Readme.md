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
fun comparisonAndEquality() {
    val num1 = 1000
    val num2 = 1000
    println(num1 == num2) // true
    println(num1.equals(num2)) // true    // we get a warning, because "equals" does not allow the operands to be null
    // If you do want to check for reference equality, use === . This won't work for some of the basic types:
    println(num1 === num2) // Still true
}
```


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

```kotlin

```

```kotlin
```

```kotlin
```

```kotlin
```

