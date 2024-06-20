
String.split() 은 쪼개진 배열을 반환함.

```java
String str = "Hello, World! How are you?";
String[] tokens = str.split(" ");

for (String token : tokens) {
    System.out.println(token);
}
```