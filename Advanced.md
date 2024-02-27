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
object Connection {
    val name: String = "my database"
}

fun main() {
    println("Only one object ?: ${connection === connection2}")         // true
    println("Only one name?: ${connection.name === connection2.name}")  // true
}
```

