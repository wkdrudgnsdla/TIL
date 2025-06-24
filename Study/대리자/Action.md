Action은 입력과 출력이 없는 메서드를 가리킬 수 있는 델리게이트이다.

다만 델리게이트와 액션의 차이점은 델리게이트는 사용자가 지정한 어떤 반환형도 가능하다
하지만 Action은 void 타입만 반환 가능하다. 반환값이 필요하다면 Func를 사용한다.

## 예시1

---

```csharp
Action Math;
private int num1 = 4;
private int num2 = 2;
private int sum = 0;

void Start()
{
		Math += Min;
		Math?.Invoke();
		Math += Plus;
		Math?.Invoke();
}

void Min()
{
		sum = num1 - num2;
		Debug.Log(sum);
}

void Plus()
{
		sum = num1 + num2;
		Debug.Log(sum);
}

//2 6 출력
```