
```java
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) {
        String str = "apple,banana,cherry,date";
        StringTokenizer tokenizer = new StringTokenizer(str, ",");

        while (tokenizer.hasMoreTokens()) {
            String token = tokenizer.nextToken();
            System.out.println(token);
        }
    }
}
```