**invoke?**:invoke란 자신이 설정한 시간만큼 함수 시작 시간을 지연하는 기능을 말합니다.

**왜 사용함?**:CPU를 최적화 시키기에 유용하며, 함수 실행 순서를 조절하기에 유용하기 때문

**Coroutine과의 차이점**:코루틴과 인보크는 둘다 시간을 지연시키는 기능임은 동일하지만,

단순히 함수의 시간 지연을 위해서라면 인보크가 훨씬 간단하게 작성할 수 있기에 간편합니다.

## 예시1

```csharp
void Start()
{
		Invoke("W3", 3f);
}

void W3()
{
		Debug.Log("3초 지남");
}
```

위는 `Invoke()` 로 일정시간동안 지연시킨 후, 함수를 실행시키는 기능을 가지고 있습니다.

위의 코드를 보면 스타트에서 `Invoke()`에 `“함수명”, 지연시킬 시간` 의 형식으로 작성하였습니다.

위처럼 작성하면 `W3`라는 메서드를 `3f` 즉 3초동안 지연시키기에 스타트 호출 이후 3초 뒤에 해당 함수를
실행합니다.

## 예시2

```csharp
void Start()
{
		InvokeRepeating("W3R1", 3f, 1f);
}

void W3R1()
{
		Debug.Log("3초 이후 1초마다 반복중");
}
```

위는 `InvokeRepeating()`으로 일정 시간 지연 후 일정 시간마다 함수를 반복하는 기능을 가지고 있습니다.

작성 방식은 `InvokeRepeating()` 에 `“함수명”, 지연시킬 시간, 몇초마다 반복할건지` 의 형식으로
작성합니다.

위처럼 작성하면 `W3R1`이라는 메서드를 3초동안 지연시키고, 추가적으로 1초마다 해당 함수를 반복합니다.

## 예시 3

```csharp
void Start()
{
		InvokeRepeating("R1", 1f, 1f);
		Invoke("Cancel10lef", 10f);
}

void R1()
{
		Debug.Log("1초마다 실행중");
}

void Cancel10lef()
{
		CancelInvoke("R1");
}
```

위는 `CancelInvoke()`로, 반복중인 Invoke를 취소 시킬 수 있습니다.

작성 방식은 `CancelInvoke()` 에 `“반복중인 인보크 함수명”` 을 넣는 형식으로 작성합니다.

위처럼 작성하면 10초가 지날때 `Cancel10lef()` 함수가 실행되기에 총 9번의 디버그가 찍히게 됩니다.