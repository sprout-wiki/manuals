
### c# 에서 out 키워드 

return 문 없이도 반환이 가능하도록 하는 c# 의 편리한 기능임
```csharp
void Swap(out int x, out int y)
{
	x = 3;
    y = 4;
	int temp = y;
    int y = x;
    int x = temp;
} // return x,y and x=x, y=y 와 동일함.
 
void main()
{
	int x; //값 초기화 필요없음
    int y;
    Swap(out x, out y); // 계산된 값이 x 와 y 로 반환된다.
    
    print(x, y);  //결과 x : 4, y : 3
}
```

Unity 에서의 충돌은 "레이" 라는 객체를 만들어서, 해당 레이가 Collisionmask 와 충돌했을때, 이벤트를 발생시키는 식으로 구현한다.
```csharp
    void CheckCollisions(float moveDistance)
    {
        Ray ray = new Ray(transform.position, transform.forward); // 레이의 시작지점, 종료지점 설정
        RaycastHit hit; // Physics.Raycast 로 ray 와 hit 을 등록하면 collision event 발생 시, hit 을 반환.

        if(Physics.Raycast(ray, out hit,moveDistance, collisionMask, QueryTriggerInteraction.Collide))
        {
            OnHitObject(hit);
        }
    }
```