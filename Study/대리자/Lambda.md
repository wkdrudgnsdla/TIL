## 예시1

---

```csharp
private int health = 0;

public void RestoreHealth(int amount) 
{
		health += amount;
}

public bool IsDead() 
{
		return (health <= 0);
}
```

위 함수는 아래의 람다식으로 획기적으로 줄일 수 있다.

```csharp
private int health = 0;

public void RestoreHealth(int amount) => health += amount;
public bool IsDead() => (health <= 0);
```

이런식으로 메서드의 내용이 단일 식 즉, 한줄로 표현 가능한 로직에 사용해서 중괄호를 생략할 수 있다.

## 예제2

---

```csharp
private int health = 0;

public int Health 
{
		get { return health; }
		set { health = value; }
}

public bool IsDead 
{
		get { return (health <= 0); }
}
```

위의 프로퍼티는 람다식을 사용하여 아래처럼 바꿀 수 있다.

```csharp
private int health = 0;

public int Health 
{
	get => health;
	set => health = value;
}

public bool IsDead => (health <= 0);
```

## 예시3

---

```csharp
delegate int Num(int num1, num2);

private void Start()
{
		Num min = (num1, num2) => num1 - num2;
		int result = min(5, 2);
		
		Debug.Log(result);//3
}
```

내가 이해한 바로는 ⇒뒤에 어떤 방식으로 사용할지 형식을 정해주고

숫자를 넣어서 계산하는것같다.

[[대리자]]