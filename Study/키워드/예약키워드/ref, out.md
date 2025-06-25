ref와 out 키워드는 함수 인자를 다룰 때 중요한 역할을 한다.

## ref

---

> **ref?**
ref 키워드는 메서드에 변수를 참조로 전달할 때 사용된다
이를 통해 메서드 내에서 인자의 값을 변경하면, 그 변경사항이 메서드를 호출한 곳에서도 반영된다.
> 

즉 ref를 사용하면 메서드가 인자로 받은 변수 자체를 직접 수정할 수 있다.

```csharp
void Start()
{
		int num = 10;
		sub(ref num, 2);
		Debug.Log(num); //8
}

void sub(ref int num, int minnum)
{
		num -= minnum;
}
```

## out

---

> **out?**
out키워드는 메서드에서 값을 반환할 때 사용되며, 주로 메서드가 여러 값을 반환해야 할 때 유용하다
out을 사용하는 변수는 메서드 내에서 반드시 초기화 되어야한다.
> 

```csharp
void Start()
{
		int num;
		if(Mul(10, 5, out result))
		{
				Debug.Log(result);
		}
}

bool Mul(int a, int b, out int result)
{
		result = a * b;
		return true;//그냥 true 반환
}
```

## ref와 out 차이점

---

- ref는 메서드로 전달되기 전에 초기화 되어야 하지만 out은 메서드 내에서 초기화 되어야 한다.
- ref는 기존 변수의 데이터를 메서드 내부로 가져와서 변경할 수 있도록 한다
out은 메서드가 결과를 외부로 출력하는데 사용된다

[[[예약키워드 설명]]]