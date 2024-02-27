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
