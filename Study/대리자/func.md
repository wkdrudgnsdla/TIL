#### Func란?
func란 반환타입이 있는 함수를 담는 델리게이트이다.
Func는 제네릭 형식으로 무조건 자료형을 명시해야만 한다.

### 사용예시1
```csharp
Func<int> Math;

private int GetRandom()
{
	return Random.Range(1,10);
}

void Start()
{
	Math += GetRandom();
	Debug.Log(Math?.Invoke()); //1~9중 무작위 숫자 출력
}
```
위와 같이 `return`즉 반환값이 `int`형인 함수를 `Func<T>`형식에 자료형을 명시하여 사용하였다.