유니티 SerializeField는 스크립트에서 private필드를 직렬화 시키기 위해 사용한다

직렬화는 개체의 상태를 저장,전송, 재구성 할수 있는 형식으로 변환하는 프로세스이다

유니티에서 직렬화는 게임 상태를 저장 및 로드하거나 에디터와 런타임 간의
데이터를 전송하는 데에 사용된다

유니티에서는 public 필드(public 변수)만 직렬화 할 수 있지만[SerializeField]를 붙이면 
private 필드도 직렬화 할 수 있다

**ex)**

```csharp
**using UnityEngine;

public class Example : MonoBehaviour
{

[SerializeField]

private int myPrivateField;

}**
```

위와 같이 private로 선언된 변수 myPrivateField 앞에 [SerializeField]를 붙이

이 변수를 직렬화 할 수 있으며 결과로 유니티에 해당 변수가 노출된다

## SerializeField의 장점

SerializeField를 사용하면 유니티 게임 오브젝트의 상태를 쉽게 저장하고 로드할 수 있다

게임을 저장하거나 다시 시작할 때 변수의 값을 유지하기 위해

SerializeField를 사용할 수 있다 또한, 인스펙터에서 변수 값을 직접 수정할 수 있기 때문에 편리하