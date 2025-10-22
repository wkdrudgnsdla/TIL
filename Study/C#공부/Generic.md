## Generic?

제네릭은 메서드나 클래스를 작성할 때 데이터 형식을 지정하지 않는다

그렇기에 코드로 작성한 후, 실제 사용하는 시점에서 데이터 형식을 지정할 수 있도록 하는 기능이다.

제네릭은 재사용성이 높으며 컴파일 시점에서 타입 체크를 하기에 안정적이고

제네릭을 사용하면 코드의 가독성과 유지보수성을 향상시킬 수 있다

제네릭은 `<T>` T부분에 데이터 타입을 아무것이나 넣을 수 있다

int, float, bool같은 값부터 string, object, class등의 참조형식,

enum, delegatem interface등의 사용자 정의 형식등 자료형이면 아무것이나 다 넣을 수 있다

## 제네릭 메서드
---

```csharp
public T Generic<T>(T a)
{
		return a;
}
```

사용할때 T 즉 타입에 해당하는 부분에 int를 넣는다면 나머지 T도 int로 정해진다

```csharp
var num = Generic<int>(2);
```

이런식으로 함수를 선언할 때 `Generic<T>` 의 T 부분에 데이터 형식을 위처럼 int로 넣으면

num의 자료형은 int가 된다

```csharp
var num = Generic<Transform>(transform);
```

int 대신 Transform을 사용하면 괄호 안의 리턴값도 Transform이여야 한다

## 제네릭 클래스
---

제네릭은 메서드보단 클래스가 더 많이 사용되는데

제네릭 클래스의 형태는`접근제한자 calss classname<T>`  와 같다

그리고 기본적인 선언 형태는 `classname<T>genericClass = new classname<T>();` 와 같다

```csharp
ex1)
public class GenericClass<T>
{
		private T data;
		
		public GenericClass(T data)
		{
				this.data = data;
		}
		
		public T GetData()
		{
				return data;
		}
}
```

`private`의 멤버변수 하나를 선언하고 생성자에게 매개변수로 입력아 저장되게 만들어져있다

```csharp
ex2)
GenericClass<int> intClass = new GenericClass<int>(1);
Debug.Log(intClass.GetData()); //1

GenericClass<string> stringclass = new GenericClass<string>("Hello world");
Debug.Log(stringClass.GetData()); //Hello world
```

ex1번은 클래스를 ex2번은 사용하는 방식을 보여주는데

ex2번과 같이 타입에 원하는 자료형을 넣은 뒤 괄호 안에 데이터를 저장한다

이러한 방식으로 타입 매개변수의 응용이 가능하다.

## ref 키워드

---

> **ref?** 
ref키워드는 메서드의 인수로 전달되는 값의 참조를 전달하기 위해 사용한다.
일반적으로 값 형식인 변수의 값을 변경하건나 외부에게 객체의 참조를 전달하는 식으로 사용한다.
> 

```csharp
public T Generic<T>(ref T a)
{
		return a;
}
```

위와 같은 형식으로 사용된다

## where키워드,제약조건

---

> **where?**
where 키워드는 타입 매개변수가 만족해야 할 조건을 명시할 수 있다.
> 

형식은 접근제한자 `class classname where T : 제약조건` 처럼 사용된다

```csharp
public class GenericClass<T> where T : new()
{

}
```

제약 조건에는 new 말고도 다양하게 올 수 있다

> new : 매개변수가 없는 기본 생성자를 가지는 형식에 대한 제약 조건이다
> 

> struct : 값 타입인 형식에 대한 제약조건이다.
> 

> class : 참조 타입인 형식에 대한 제약조건이다.
> 

> ParentClass : 파생된 형식에 대한 제약조건이다(무조건 ParentClass를 상속해야한다)
> 

> ISomeIntercace : 인터페이스를 구현하는 형식에 대한 제약조건이다.
> 

> strrct, ISomeInterface : 값 타입 형식 + 인터페이스 형식에 대한 제약조건이다.
>