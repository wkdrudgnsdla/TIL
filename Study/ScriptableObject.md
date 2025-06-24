여러 객체의 인스턴스에서 사용 가능한 정의한 형식의 데이터를 담는 컨테이너

## **사용방법**

---

ScriptableObject를 상속받는다

[CreateAssetMenu]를 통해 파일의 이름과 메뉴 구조를 지정한다

지정된 메뉴 구조를 통해 새로운 ScriptableObject를 생성한다

```csharp
using UnityEngine;
[CreateAssetMenu(fileName = "Example", menuName = "Create New ScriptableObject")]

public classs Example : ScriptableObject
{
	public GameObject exampleGameObject;
	public int damage;
	public int health;
	public int moveSpeed;
}
```

## ScriptableObject에서 struct, tuple 사용

---

ScriptableObject에서 직렬화 없이 Struct, Tuple을 사용하면

생성한 ScriptableObject의 인스펙터 창에서 보이지 않는다

```csharp
using UnityEngine;

[CreateAssatMenu(fileName = "Example", menuName = "Create New ScriptableObject")]
public class Example : ScriptableObject
{
	public struct ExampleStruct
	{
		public int Damage;
	}
	public (string, int) exampleTuple;
	public ExampleStruct exampleStruct;
}
```

 Struct, Tuple은 기본적으로 직렬화 되는 타입이 아니다. 인스펙터에 보이기 위해서는 별도로 직렬화가 가능한 타입으로 만들어 주어야 한다.

### 직렬화 방법

---

- Struct:기본적으로 직렬화 타입이 아니기 때문에 [System.Serializable]을 이용해서 직렬화 가능하게 해줘야 한다
- Tuple:Tuple은 C#지원하는 기능으로 유니티에선 기본적으로 직렬화 되지 않는다 만들고자 하였던 tuple과 같은 구조의 직렬화 가능한 클래스로 대체하여서, 해당 클래스의 변수를 통해 tuple과 같이 사용할 수 있다.

```csharp
using UnityEngine;
[System.Serializable]
public class ExampleTuple
{
		public int Damage;
		public int health;
}
[CreateAssetMenu(fileName = "Example", menuName = "Create New ScriptableObject")]
public class Example : ScriptableObject
{
		[System.Serializable]
		public struct ExampleStruct
		{
				public float Damage;
				public int health;
		}
		public ExampleTuple exampleTuple;
		public ExampleStruct exampleStruct;
}
```

### 복사와 참조의 기준

---

우선 빌드가 된 게임에는 생성된 ScriptableObject에 저장된 값은 불변한다

여기서 불변하는 값을 기준으로 복사와 참조의 사용을 정하면 된다

### 복사

ScriptableObject를 통해서 가져온 값으로 초기화 이후에 실시간으로 변하는 값을때 사용한다

### 참조

ScriptableObject를 통해 가져온 값이 바뀌지 않을 때 사용한다

## 장점

---

값의 불필요한 복사를 하나의 ScriptableObejct에 대한 참조로, 메모리 효율을 높인다.

에디터에서 생성하고 값을 수정할 수 있다.

## 주의할점

---

에디터의 플레이모드에서 SO의 값을 변경하는 과정에서, 기존에 설정해 놓은

ScriptableObject의 값이 변경될 수 있다

빌드된 게임에서는 ScriptableObject의 값을 변경할 수 없기 때문에 변경 가능한 정보를 저장하려는 목적으로 사용되어서는 안된다

### 요약

---

ScriptableObject는 인스턴스의 값을 에디터에서 스크립트보다 편하게 생성, 관리, 저장, 수정할수 있게 해준다

이를 통해 개발자가 아닌 누구라도 쉽게 값을 효율적이고, 이해하기 쉽게 다룰 수 있다.