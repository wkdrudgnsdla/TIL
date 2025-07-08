Unity의 Time 클래스는 시간을 제어하고 조작하기 위해서 사용된다.

##### Time 클래스란?
타임 클래스는 UnityEngine 네임스페이스의 일부이며, 시간 정보를 처리하는데 사용되는
속성과 함수를 제공한다.


## 사용방법

#### 1.Time.time
`Time.time`은 게임이 실행된 이후의 누적시간을 초단위로 나타낸다.
정확하게는 첫 프레임 실행 이후부터의 시간을 나타낸다.
```csharp
void Update()
{
    if (Time.time > 5)
    {
        Debug.Log("게임 시작 이후 5초가 지남");
    }
}
```


#### 2.Time.deltaTime
'Time.deltaTime'은 마지막 프레임을 완료하는 데 걸린 시간을 초 단위로 나타낸 것이다. 게임 오브젝트 움직임을 부드럽게 하고 프레임 속도를 독립적으로 만드는 데 사용할 수 있다.
```csharp
float speed = 10f;
float Time = 0;

void Update()
{
    transform.Translate(Vector3.forward * speed * Time.deltaTime);

	Time += Time.deltaTime;
	if(Time > 5)
	{
		Debug.Log("5초가 지남);
	}
}

```

#### 3.Time.fixedDeltaTime
**`FixedUpdate()` 내부**에서 물리 시뮬레이션 보정(이동, 힘 적용 등)에 곱해 주는 용도로 사용한다.
`Time.fixedDeltaTime`은 `FixedUpdate()`가 호출되는 고정된 시간 간격을 뜻한다.
```csharp
float speed = 10f;

void FixedUpdate()
{
    rb.MovePosition(rb.position + Vector3.forward * speed * Time.fixedDeltaTime);
}
```


#### 4.Time.timeScale



###### DeltaTime과 fixedDeltaTime의 차이점
공부하다가 두개의 차이점을 명확히 모르겠어서 여러 문서를 찾아본 결과
`DeltaTime`은 프레임 속도에 가변하는 반면 `fixedDeltaTime`은 0.02초로 1초에 50번 호출된다.
0.02초마다 호출된다는 소리는 `FixedUpdate()`의 호출 주기가 기본값 0.02초이기 때문에 
`FixedUpdate()`가 1초에 50번 호출되는데 이때, 내부적으로 `Time.fixedDeltaTime`(0.02초)만큼 시간이 흘렀다고 간주하게 된다. 누적된 물리 시간은 `Time.fixedTime`의 값이 되는것으로 보면 된다고 한다.

