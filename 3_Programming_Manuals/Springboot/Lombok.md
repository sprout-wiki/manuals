

### lombok.Getter / lombok.Setter

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Messenger{
	private String str_message;
	private int msg_count;

	public static void main(String[] args){
		Messenger m1 = new Messenger();
		m1.setStr_message; 
	}
}
```