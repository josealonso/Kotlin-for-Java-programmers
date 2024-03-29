## Kotlin advanced topics and comparison with Java

### Strings in Java and Kotlin

**Build a string**

```java
StringBuilder countDown = new StringBuilder();
for (int i = 5; i > 0; i--) {
    countDown.append(i);
    countDown.append("\n");
}
System.out.println(countDown);
```
In Kotlin, use buildString() – an inline function that takes logic to construct a string as a lambda argument:

```kotlin
// Kotlin
val countDown = buildString {   // Under the hood, the buildString uses the same StringBuilder class as in Java
    for (i in 5 downTo 1) {
        append(i)
        appendLine()
    }
}
println(countDown)
```

**Take a substring﻿**
```java
// Java
String input = "What is the best JVM language? Groovy";
String answer = input.substring(input.indexOf("?") + 1);
System.out.println(answer);
```

```kotlin
val input = "What is the best JVM language? Kotlin hands down"
val answer = input.substringAfter("?")  // or input.substringAfterLast("?")
println(answer)
```

**Set default value if the string is blank﻿**

```java
// Java
public void defaultValueIfStringIsBlank() {
    String nameValue = getName();
    String name = nameValue.isBlank() ? "MISSING NAME" : nameValue;
    System.out.println(name);
}
```

```kotlin
fun main() {
    val name = getName().ifBlank { "MISSING NAME" }
    println(name)
}
```

**Split a string**
```java
// Java
System.out.println(Arrays.toString("Once.upon.a.time".split("\\.")));
```

```kotlin
// Kotlin
fun splitStr() {
    var sentenceWithDelimiters = "Once.upon.a.time"
    val words = sentenceWithDelimiters.split(".")
    println(words)   // [Once, upon, a, time]
}
```

**Use multiline strings**
```java
// Java
String result = """
    Kotlin
       Java
    """;
System.out.println(result);
```
The difference with Java is that Java automatically trims indents, and in Kotlin you should do it explicitly:

```kotlin
// Kotlin   
val result = """
        Kotlin
           Java 
    """.trimIndent()
println(result)
```

**Create a string from collection items**

```java
// Java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);
String invertedOddNumbers = numbers
        .stream()
        .filter(it -> it % 2 != 0)
        .map(it -> -it)
        .map(Object::toString)
        .collect(Collectors.joining("; "));
System.out.println(invertedOddNumbers);
```

```kotlin
// Kotlin
fun createStringFromCollectionItems() {
    val numbers = listOf(1, 2, 3, 4, 5, 6)
    val invertedOddNumbers = numbers
        .filter { it % 2 != 0 }
        .joinToString(separator = "; ", prefix = "== ", postfix = " ==") {"${-it}"}
    println(invertedOddNumbers)   // == -1; -3; -5 ==
}
```

### Reading a nullable list 
```java
/*
    This is a common task in Java.
    Some of the read data may be null.
    Keep track of the index whose element is null.
*/
 
List<String> customers = new ArrayList<>();
customers.add("22");
customers.add(null);
// var customers = List.of("12", "22", null);
for (int i = 0; i < customers.size(); i++) {
    var elem = customers.get(i);
    if (elem == null) {
        elem = "NULL VALUE";
    }
    System.out.println(i + ": " + elem);
}
```

```kotlin
val customers = mutableListOf<String?>()  // "?" indicates it's a nullable list
customers.add(null)
customers.add("22")  
for ((index, elem) in customers.withIndex()) {  // No need to think about the index bounds
    println("$index: $elem")
}
```

### If-not-null shorthand﻿ or the safe-call operator 

```kotlin
val files = File("Test").listFiles()
println(files?.size) // size is printed if files is not null
```

### If-not-null-else shorthand﻿ or the Elvis operator

```kotlin
val files = File("Test").listFiles()

if (files != null) {
    println(files.size)
} else {
    println("empty")
}

// Shortcut provided by Kotlin
println(files?.size ?: "empty") // if files is null, this prints "empty"

// To calculate a more complicated fallback value in a code block, use `run`
val filesSize = files?.size ?: run {
    val someSize = getSomeSize()
    someSize * 2
}
println(filesSize)
```

**These two operators already existed in Groovy**

```groovy
def name = null
def length = name?.length() ?: 0
println length
```

### Functions returning a value or null﻿

```java
// In Java, you need to be careful when working with list elements.
// You should always check whether an element exists at an index before you attempt to use the element.
var numbers = new ArrayList<Integer>();
numbers.add(1);
numbers.add(2);

System.out.println(numbers.get(0));
numbers.get(5) // Exception!
```

```kotlin
/*
 The Kotlin standard library often provides functions whose names indicate whether they can possibly return a null value.
 This is especially common in the collections API:
*/
fun nullInLists() {
    val numbers = listOf(1, 2)
    println(numbers[0])  // Can throw IndexOutOfBoundsException if the collection is empty
    println(numbers.get(0))  // The indexing operator is preferred in Kotlin

    numbers[5]     // Exception!

    println(numbers.firstOrNull())
    println(numbers.getOrNull(5)) // null
}
```

### The Singleton Pattern

```java
public final class Connection {

    private static Connection INSTANCE;
    private String name = "my database";
    private Connection() { }

    public static Connection getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Connection();
        }
        
        return INSTANCE;
    }
}
```

```kotlin
/* Singleton pattern */
object Connection2 {
    val name: String = "my database"
}

fun main() {
    val connection = Connection2
    val connection2 = Connection2
    println("Only one object ?: ${connection === connection2}")         // true
    println("Only one name?: ${connection.name === connection2.name}")  // true
}
```

**Accessing Kotlin object from Java**

```java
/*
  Access the Kotlin singleton from Java
*/
var name = Connection2.INSTANCE.getName(); // only way if Kotlin code is not annotated
var name2 = Connection2.getName();         // only possible if Kotlin code is annotated
System.out.println(name);  // my database
System.out.println(name2); // my database
```

```kotlin
/* Singleton pattern */
object Connection2 {
    @JvmStatic  // static getter and setter methods are generated
    val name: String = "my database"
}
```

### Utility class

```kotlin
class Utils {
    companion object {  // Each class can have only one companion object

        fun calculateInterests(date: Instant): Int {
            .....................
            return 23
        }
        // other functions
    }
}
```

```Kotlin
Utils.calculateInterests(Instant.now())
```

```java
Utils.Companion.calculateInterests(Instant.now());
```

**Accessing Kotlin companion object from Java**

```Kotlin
class Utils {

    // A companion object is an instance of a real class called "Companion"
    companion object {

        @JvmField
        val magicWord = "PLEASE"

        // This annotation tells Java classes to treat this method as if it was static.
        @JvmStatic
        fun calculateInterests(date: Instant): Int {
            .....................
            return 23
        }
    }
}
```

```java
Utils.calculateInterests(Instant.now());
var magicWord = Utils.magicWord;
```

### Static Factory methods

```kotlin
class User private constructor(val id: Long, val name: String) {
    // Consistency of the next id generation is guaranteed because a companion object is a singleton.
    companion object {
        private var currentId = 0L;
        fun newInstance(name: String) = User(currentId++, name)
    }
}
```

```kotlin
val jose = User.newInstance("José")
```

### Builder Pattern

```java
public record Programmer(String name, String lastName, String preferredLanguage) {

    // Builder
    public static final class Builder {
        String name;
        String lastName;
        String preferredLanguage;

        public Builder(String name) {
            this.name = name;
        }
        public Builder lastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        public Builder preferredLanguage(String preferredLanguage) {
            this.preferredLanguage = preferredLanguage;
            return this;
        }

        public Programmer build() {
            return new Programmer(name, lastName, preferredLanguage);
        }
    }
}
```

Usage
```java
var programmer = new Programmer.Builder("Juan")
        .lastName("Pérez")
        .preferredLanguage("Python")
        .build();
```

```java
/*
 Using Lombok
 */
@Builder
record Programmer(String name, String lastName, String preferredLanguage) { }
.....................

var programmer = new Programmer.builder()
        .name("Juan")
        .lastName("Pérez")
        .preferredLanguage("Python")
        .build();
```

Builder Pattern in Kotlin

```kotlin
/*
 Default values and named arguments make the builder pattern not needed
 */
data class Programmer(
    // All optional fields must be initialized
    val name: String = "programmer",
    val lastName: String? = null,
    val preferredLanguage: String = "Kotlin"
)
```

```kotlin
val jose = Programmer(lastName = "Alonso", name = "José Ramón")
val juan = Programmer()
println(jose)  // Programmer(name=José Ramón, lastName=Alonso, preferredLanguage=Kotlin)
println(juan)  // Programmer(name=programmer, lastName=null, preferredLanguage=Kotlin)
```

### Java Streams and its equivalent in Kotlin

```java
var numbers = List.of(1, 2, 3, 4, 5, 6);
numbers.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

```kotlin
/*
 Conversion to streams is not needed.
 The standard collections have many methods.
*/
listOf(1, 2, 3, 4, 5, 6)
    .filter { it % 2 == 0 }
    .forEach { println(it) }
```

## B O N U S

### Call multiple methods on an object instance (with)

```kotlin
class Programmer {
    fun study(): Nothing = TODO()
    fun iterationMeeting(minutes: Int): Nothing = TODO()
    fun planningMeeting(minutes: Int): Nothing = TODO()
    fun code(numOfLines: Int): Nothing = TODO()
}

val ninjaProgrammer = Programmer()
with(ninjaProgrammer) { 
    study()
    for (i in 1..2) {
        iterationMeeting(60)
        planningMeeting(90)
    }
    code(50)
}
```

### TODO

Kotlin's standard library has a *TODO()* function that will always throw a *NotImplementedError*. 
Its return type is Nothing so it can be used regardless of expected type. 
There's also an overload that accepts a reason parameter.
