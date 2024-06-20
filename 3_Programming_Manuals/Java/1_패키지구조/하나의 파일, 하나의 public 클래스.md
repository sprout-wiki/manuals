Java에서는 일반적으로 하나의 소스 파일에 하나의 public 클래스만을 가질 수 있습니다. 이 클래스의 이름은 파일의 이름과 일치해야 합니다.

하지만, 하나의 소스 파일에 여러 개의 non-public 클래스(즉, public이 아닌 클래스)를 가질 수 있습니다. 이러한 non-public 클래스는 어떤 이름으로든 소스 파일에 선언될 수 있습니다.

예를 들어, 다음과 같은 소스 파일이 있다고 가정해보겠습니다.

```java
// MyClass.java
public class MyClass {
    // MyClass에 대한 정의
}

class AnotherClass {
    // AnotherClass에 대한 정의
}

```


위의 경우, 소스 파일 `MyClass.java`에는 `MyClass`라는 public 클래스와 `AnotherClass`라는 non-public 클래스가 선언되어 있습니다. 이러한 구조는 Java 언어에서 허용되며, 이러한 방식으로 다수의 클래스를 하나의 소스 파일에 정의할 수 있습니다. 그러나 public 클래스는 소스 파일의 이름과 일치해야 합니다.