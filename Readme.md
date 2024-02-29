Code examples for preparing the speech "Kotlin for Java programmers", given in the Madrid Java Users Group (JUG) in March 2024.
The presentation has two different parts:
- Introduction to Kotlin.
- Advanced topics.

## Kotlin for Java programmers, introduction


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
/*
 A function that returns no meaningful value.
 "Unit" return type can be omitted. It is equivalent to "void" in Java.
*/
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
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

/* if can be a statement, like in Java */
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b
    }
}
```

```kotlin
/* if is an expression here */
fun maxOf(a: Int, b: Int): Int {
    return if (a > b) {
        a
    } else {
        b
    }
}
```

```kotlin
/* if expression in only one line */
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

**when expression**
```kotlin
fun describe(obj: Any): String =  // what if the parameter is null?
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }
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

**LOOPS**

**for-each loop**
```kotlin
    val kotlinLovers = listOf("Juanjo", "Jose", "David")
    for (j in kotlinLovers) {
        println(j)
    }
```

**for loop: the range operator**
```kotlin
/*
 Descending, inclusive range
*/
for (i in 6 downTo 0) {
    print("$i ")
}

/*
 Only even numbers
*/
for (i in 6 downTo 0 step 2) {
  print("$i ")
}

/*
 Exclusive range
*/
for (i in 0 until 6) {
    print("$i ")
}
```

**C L A S S E S**

```kotlin
class Person(val name: String, var age: Int)  
/*
 There is no "new" keyword in Kotlin.
 Kotlin will generate a getter method for each attribute.
 And for mutable attributes also a setter method.
*/
val onePerson = Person("José", 24)   // The default constructor with parameters listed in the class declaration is available automatically.
println("New Person: $onePerson")  // New Person: learningkotlin.examples.Person@17f052a3
onePerson.age = 47
println("New Person: $onePerson")  // New Person: learningkotlin.examples.Person@17f052a3
println("New age: ${onePerson.age}")  // New age: 47
```

**Custom setter**

```kotlin
/*
 Custom setter
 - The property must be inside the class body.
 - Common use case: having a mutable property with some validations
 */
class Person(val name: String, var age: Int) {
    var dateOfBirth: Int = 1970
        set(value) {
            field = if (value >= 1970) value else 1970
        }
    /* Custom getter */
    val nameInUpperCase = name
        get() = field.uppercase()

}
```

**Inheritance**

```kotlin
/*
 Kotlin classes are final by default
*/
open class Animal 
class Dog: Animal()
```

```kotlin
/*
 Both interfaces and abstract members are always open, so the open modifier is not needed.
*/
interface Clickable {
    fun click()
}

class Button : Clickable {
    // The override modifier is mandatory
    // The overridden member is open by default, if not marked final
    override fun click() = println("Button was clicked")
}
```

**A sealed hierarchy**

```kotlin
/* 
 A sealed hierarchy 
*/
sealed class Expr
class Num(val value: Int) : Expr()
class Sum(val left: Expr, val right: Expr) : Expr()
class Mul(val left: Expr, val right: Expr): Expr()
fun eval(e: Expr): Int =
    when (e) {
        is Num -> e.value
        is Sum -> eval(e.right) + eval(e.left)
// ERROR: 'when' expression must be exhaustive,
// add necessary 'is Mul' branch or 'else' branch instead
        is Mul -> TODO()
    }
```

**Data classes**

```kotlin
/*
 equals, toString and hashCode functions are generated automatically.
*/
data class Person(val name: String, var age: Int)
```

Similar to Java records

```java
public record Person(String name, Integer age) {}
```

Differences
- Kotlin data classes also generates a copy() function.
- In a record, all properties must be final.
- The data classes can inherit from other classes, while records don't allow that.

```kotlin
/* 
 Access a data class from Java 
*/
@JvmRecord    // Using @JvmRecord annotation on data classes for interoperability.
data class Person(val name: String, var age: Int)
```

**EXTENSION PROPERTY**
```kotlin
val String.lastChar22: Char
    get() = this.get(length - 1)
```

**EXTENSION FUNCTION**
```kotlin
/***********************************************************************
EXTENSION FUNCTIONS
The String class is called a method receiver.
Even though you may not even have the source code to that class,
you can still extend it with the methods you need in your project.
Unlike methods defined in the class, extension functions don’t have access
to private or protected members of the class.
Extension functions are effectively syntactic sugar over static method calls.
Many extension functions are declared in the Kotlin standard library.
***********************************************************************/
fun String.lastChar(): Char = get(this.length - 1)
```

```kotlin
val text = "kotlin"
println("Property: ${text.lastChar22}")
println("Function: ${text.lastChar()}")
```
