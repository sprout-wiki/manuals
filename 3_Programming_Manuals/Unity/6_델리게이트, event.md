
델리게이트는 C# 에서의 함수 포인터 같은것이다.
함수를 변수에 저장해서 쓸 수 있음
```csharp

public class Example
{
    public void Method1()
    {
        Console.WriteLine("Method 1");
    }

    public void Method2()
    {
        Console.WriteLine("Method 2");
    }

    public void SubscribeToAction()
    {
        System.Action action = Method1; // action 이라는 델리게이트를 선언, Method1 을 등록. 델리게이트는 C# 에서 일종의 함수 포인터임.
        action += Method2; // action 에 Method2 를 추가 (구독이라고 부름) 이때, += 는 연산자오버로드로, .NET 에서 알아서 처리해줌. 내부작동은 몰라도 됨
        // 구독 해제는 -= 로 한다.
	    // 델리게이트 객체의 "호출목록" 배열에 함수가 push 된다.

        // Action 호출
        action(); // Method1 과 Method2 가 차례로 실행된다.
    }
}

public class Program
{
    public static void Main()
    {
        Example example = new Example();
        example.SubscribeToAction();
    }
}

```