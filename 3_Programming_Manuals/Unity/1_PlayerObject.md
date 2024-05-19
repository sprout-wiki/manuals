
>Script 를 Object 에 Drag 하면 Object 가 해당 Script 로 작동한다.

RequireComponent 구문으로 다른 Script 를 불러온다.


Player.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(PlayerController))]
public class Player : MonoBehaviour
{
    public float moveSpeed = 5;

    Camera viewCamera;
    PlayerController controller;

    // Start is called before the first frame update
    void Start()
    {
        controller = GetComponent<PlayerController>();
        viewCamera = Camera.main;
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 moveInput = new Vector3(Input.GetAxis("Horizontal"),0,Input.GetAxis("Vertical")); // 키보드 입력 받기. wasd 와 방향키가 기본값임
        // Edit/Project Settings/Input 에서 Axes 의 기본값이 arrow key 와 WASD 임
        Vector3 moveVelocity = moveInput.normalized * moveSpeed;
        controller.Move(moveVelocity);

        Ray ray = viewCamera.ScreenPointToRay(Input.mousePosition);
        Plane groundPlane = new Plane(Vector3.up, Vector3.zero);
        float rayDistance;

        if(groundPlane.Raycast(ray,out rayDistance)) // 평면과 Ray 가 교차하는지 검사 (플레이어가 밖으로 떨어지거나 하면 시선-마우스가 해제된다.)
        {
            Vector3 point = ray.GetPoint(rayDistance);
            //Debug.DrawLine(ray.origin, point, Color.red);
            controller.LookAt(point);
        }
    }
}
```

PlayerController.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(Rigidbody))]

public class PlayerController : MonoBehaviour
{
    Vector3 velocity;
    Rigidbody myRigidbody;

    // Start is called before the first frame update
    void Start()
    {
        myRigidbody = GetComponent<Rigidbody>(); // Unity Rigidbody 를 상속하여 충돌 등을 간편하게 구현       
    }

    public void Move(Vector3 _velocity)
    {
        velocity = _velocity;
    }

    public void LookAt(Vector3 lookPoint)
    {
        Vector3 heightCorrectedPoint = new Vector3(lookPoint.x, transform.position.y, lookPoint.z); // 아래를 바라봐서 플레이어가 기울어지는 것을 방지
        transform.LookAt(heightCorrectedPoint); // transform : position, rotation, scale 을 저장하는 component object
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        myRigidbody.MovePosition(myRigidbody.position + velocity * Time.fixedDeltaTime);
    }
}

```