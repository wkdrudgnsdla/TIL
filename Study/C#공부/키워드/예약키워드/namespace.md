유니티 메뉴얼 네임스페이스 부분을 보면, 대형 프로젝트로 많은 스크립트를 사용할수록 스크립트 클래스

이름이 충돌할 가능성이 커진다고 한다. 여러 프로그래머가 서로 다른 파트를 작업하고 나중에 하나로

통합할 때 이런 현상이 두드러지게 된다고 한다.

이러한 문제를 피하기 위해 사용할 수 있는것이 바로 네이밍 규칙을 도입하는 것인데,

이 또한 이름이 충돌하는 클래스가 여러 개이거나 해당 이름으로 변수가 선언되어 있다면 번거로운 작업이 될 수 있다는 허점이 존재한다.

이러한 현상들은 **네임스페이스** 기능으로 이러한 문제를 확실히 해결할 수 있다.

네임스페이스는 클래스 이름에 선택한 접두어를 사용하여 참조하는 클래스의 모음이다.

## 사용 방법
```csharp
namespace Want
{
		public class GoHome : MonoBehaviour
		{
				public static void Home()
				{
						Debug.Log("집가고싶어");
				}
		}
		
		public class WantChicken : MonoBehaviour
		{
				public static void Chicken()
				{
						Debug.Log("치킨 먹고싶어);
				}
		}
}
```

class를 하나 만들어서 namespace Want라고 정의했습니다.

또한 아래 GoHome과 WantChickien 클래스를 만들었습니다

```csharp
using Want;

public class WantWant : MonoBehaviour
{
		void Start()
		{
				GoHome.Home();//집가고싶어 출력
		}
}
```

```csharp
using Want;

public class WantWant : MonoBehaviour
{
		void Start()
		{
				WantChicken.Chicken();//치킨 먹고싶어 출력
		}
}
```

- 유니티 메뉴얼 링크

[https://docs.unity3d.com/kr/530/Manual/Namespaces.html](https://docs.unity3d.com/kr/530/Manual/Namespaces.html)

[[예약키워드 설명]]