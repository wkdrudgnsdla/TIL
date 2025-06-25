delegate란 함수에 대한 참조이다

하나의 델리게이트로 여러개의 함수를 접근하여 실행을 할 수 있다.

함수를 파라미터로 전달할 수 있고 여러개의 함수를 한번에 실행하거나

어떠한 상황에 대한 이벤트를 발생시킬 수도 있다.

쉽게 설명하자면 [[대리자]] 역할을 하지만 정해준 일밖에 하지 못한다.

# 델리게이트의 기본

---

```csharp
delegate int Num(int a, int b);

int add(int a, int b)
{
		return a + b;
}

int sub(int a, int b)
{
		return a - b;
}

void Start()
{
		Num num = new Num(add);
		
		Debug.Log(num(20, 10)); //30나옴
}
```

> 델리게이트는 리턴 타입과 매개 변수가 모두 일치해야 한다. 
add, sub 함수의 형태와 같으며 
참조변수와 같기때문에 객체를 생성하고 괄호 안에
참조할 함수를 넣는게 델리게이트의 기본 사용법이다.
> 

(어떠한 자료형의 함수 2개를 한번에 저장할수 있는것으로 이해함)

# 델리게이트의 활용

---

```csharp
delegate int Num(int a, int b);

int add(int a, int b)
{
		return a + b;
}

int sub(int a, int b_)
{
	return a - b;
}

int Action(int a, int b, Num num)
{
		return Num(a, b);
}

void Start()
{
		int Number = Action(10, 20, add)
		Debug.Log(Number); //30나옴
```

> Action이라는 함수에 파라미터에 델리게이터를 통해서 함수를 참조로 넘긴다
add로 함수의 참조 객체를 전달해 출력된 값은 더하기 연산이 나온다
> 

(action과 같이 함수에 델리게이트를 넣어서 Start에서 무슨 연산을 할지 정할 수 있다고 이해)

# 델리게이트의 활용2(델리게이트 체인)

---

## 델리게이트 체인?

> 델리게이트 체인은 하나의 델리게이트 안에 여러개의 함수를 연결하여
연쇠적으로 호출하는 방식이다

+= 연산자를 이용해서 인스턴스를 추가 할 수 있고,
-= 연산자를 통해서 인스턴스를 삭제할 수 있다.
> 

```csharp
delegate void Num(int a, int b);
Num num;

void add(int a, int b)
{
		Debug.Log(a + b);
}

void sub(int a, int b)
{
		Debug.Log(a - b);
}

void mul(int a, int b)
{
		Debug.Log(a * b);
}

void div(int a, int b)
{
		Debug.Log(a / b);
}

void Start()
{
		num = add;
		num += sub; //인스턴스 추가
		num += mul;
		num += div;
		
		num -= div; //인스턴스 삭제
		
		num(10, 5);
}

//15, 5, 50 출력
```

(델리게이트에 여러 함수를 연결해서 호출, 추가, 삭제 할수있다고 이해)

[[[대리자]]]