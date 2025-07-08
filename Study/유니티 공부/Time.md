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
float Speed = 10.0f;
float Time = 0;

void Update()
{
    transform.Translate(Vector3.forward * Speed * Time.deltaTime);

	Time += Time.deltaTime;
	if(Time > 5)
	{
		Debug.Log("5초가 지남);
	}
}

```
