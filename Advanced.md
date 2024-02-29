## Kotlin advanced topics and comparison with Java

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
