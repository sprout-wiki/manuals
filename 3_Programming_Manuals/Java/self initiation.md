클래스 안에서 해당 클래스의 인스턴스를 생성할 수 있다.

1. 클래스의 정적(static) 영역이 로드됩니다. 이 때 클래스의 정적 멤버 변수와 정적 메서드가 메모리에 로드됩니다.
2. 클래스의 생성자가 호출됩니다. 이때 생성자는 클래스의 인스턴스를 생성하고, 해당 인스턴스의 메모리를 할당합니다.
```java

public class Apple(){
	public String color = "Red";
	public static void main(String[] args){
		Apple f1 = new Apple();
		System.out.println("f1.color")
	}
}

```