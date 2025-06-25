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

### 사용예시2
또한 `Func`델리게이트는 최대 16개의 매개변수를 받을 수 있고 맨 마지막 제네릭 타입이 반환형이다.
```csharp
Func<int, int, bool, String> asdf;

private String fdsa(int x, int y, bool a)
{
	if(a)
	{
		return $"{x} + {y} = {x + y}";//문자열 보간 중괄호 안의 식을 계산 후 문자열로 계산
	}
	else
	{
		return $"{x} - {y} = {x - y}"; 
	}
}

void Start()
{
	asdf += fdsa(5, 2, true);
	asdf?.Invoke(); //7
	asdf += fdsa(5, 2, false);
	asdf?.Invoke(); //3
}
```
마지막 제네릭 타입인 `String`을 반환형으로 해야하기 때문에 위와같이 `return` 형식을 문자열 형식으로 맞춰 반환해주면 된다.
`fdsa()`에 반환형인 `String`을 제외한 `int, int, bool`형식을 매개변수로 넣어주고 그 값을 대입시켜주면 된다.