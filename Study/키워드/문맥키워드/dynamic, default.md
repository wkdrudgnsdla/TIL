**dynamic?**
컴파일 시점이 아닌, **런타임 시점에서 형식 검사**를 수행하게 하는 키워드이다.

예시로 [[Generic]]을 사용한 예를 들어보겠다.
```csharp
void Start()
{
	int[] arr = {};
	SetArray<int>(arr);
}

static T SetArray<T>(T[] Datas)
{
	int temp = 0;

	for(int i = 0; i < Datas.Length; i++)
	{
		temp += Datas[i];
	}
	return temp;
}
```
제네릭 메소드 SetArray를 호출하여 특정 변수에 값을 담고, 그 값에 SetArray의 매개변수를 +=로 더해주는 코드이다.

하지만 이렇게 코드를 작성하면 **temp를 생성할 때도 오류**가 생기고, **초기값을 설정할 때도 오류**가 생기는데.
이 문제를 **dynamic과 default**를 사용하여 아래와 같은 방법으로 해결할 수 있다.
```csharp
void Start()
{
	int[] arr = {};
	SetArray<int>(arr);
}

ststic T SetArray<T>(T[] Datas)
{
	dynamic temp = default(T);
	
	for(int i = 0; i < Datas.Length; i++)
	{
		temp += Datas[i];
	}
	return temp;
}
```
**dynamic** 키워드를 사용하여 **temp를 선언**하고, 그 초기값을 설정할 때는 **default** 키워드를 사용하여 **초기값을 설정**하여 문제를 해결했다

위의 상황에서 `default`를 사용한 이유는 제네릭 메소드로 받아온 **매개변수의 자료형과 똑같은 자료형인 변수를 만들고**, 그 변수의 초기값을 설정해주고 싶었기에 사용하였는데, `temp = null`이나 `temp = 10`과 같은 식으로 초기화 시키기엔 형식 매개변수 T를 받아온 이유가 없다.

```csharp
ststic T SetArray<T>(T[] Datas)
{
	dynamic temp = default(T);
	
	for(int i = 0; i < Datas.Length; i++)
	{
		temp += Datas[i];
	}
	return temp;
}
```
따라서 이렇게 **제네릭으로 들어오는 변수의 초기값을 주고싶을 때** `default`를 사용하면 된다.

쉽게 설명하자면, `default(T)`를 사용하여 **제네릭, 즉 T부분에 들어오는 자료형에 맞는 형식들의 빈값**(초기값)
을 얻기 위하여 사용한다. 만약 T부분에 `int`형이 들어온다면 0을, `String`이 들어온다면 `null`을, `bool`이 들어온다면 `false`를 받는다.

###### dynamic과 var의 차이점
`dynamic`은 모든 타입을 다 허용하기에 제네릭과 사용하기에 적합하다.
그렇다면 `var`와의 차이점은 과연 무엇일까?
[[var]]키워드는 `var a = 10;`과 같은 식으로 선언하면 뒤의 값이 10이기에 `int`자료형이 적용된다.
하지만 `var a = default(T)`와 같은식으로 사용한다면 **뒤의 값을 모르기에 에러가 발생**하게 된다.
이러한 이유로 `var`는 **제네릭과 사용하기엔 부적합**하며, `dynamic`으로 대체한다.
`dynamic`키워드는 `dynamic a = 10;`과 같은 식으로 선언하여도  `int`형이 부여되지 않고,
**런타임 직전에 자료형이 부여**된다. 또한 `dynamic a = default(T)`와 같은 식으로 값에 **빈값을 넣어도 오류가 뜨지 않는다.**
즉, **변수 선언시 값을 꼭 넣어줄 필요도 없으며**, **자료형이 결정된 이후에도 다른 형태의 자료형을 입력**할 수 있다.  이러한 이유로 인해서 `dynamic`이 제네릭과 사용하기에 적합한 이유이다.

`var`와의 차이점 정리

|           | `var`        | `dynamic`     |
| --------- | ------------ | ------------- |
| 자료형 결정 시점 | 컴파일 시        | 런타임 직전        |
| 초기화 필요 여부 | 선언과 동시에 값 할당 | 초기값 없어도 선언 가능 |


#### 주의점
`dynamic`을 사용하면 **강제 형변환**을 하기 때문에 런타임 때 오류가 발생할 수 있다.
또한 `dynamic`은 **리소스를 많이 차지하고 런타임 때 오류가 발생**할 확률이 높기 때문에 필요한 상황이 아니면
사용하지 않는 것이 좋다.
