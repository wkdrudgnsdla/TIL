유니티 에디터에서 Window→AI→Navigation 선택

![image.png](image%207.png)

만약 안보인다면 오브젝트에서 add component → nav mesh agent 추가 후

install 하면 보이게 됨

이후 간단히 타겟을 따라가는 스크립트 작성

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class navigation: MonoBehaviour
{
    public Transform target;

    NavMeshAgent Agent;

    void Start()
    {
        Agent = GetComponent<NavMeshAgent>();
    }
    
    void Update()
    {
        Agent.SetDestination(target.transform.position);
    }
}
```

NavMeshAgent의 컴포넌트를 받아와서 Update 함수에 SetDistination함수로 목적지를 설정

이러면 적을 따라가는 오브젝트가 간단히 완성

<주의점>

```csharp
target = GetComponent<Transform>();
```

이런식으로 게임 오브젝트의 트랜스폼을 가져오면

자기 자신으로 이동하려 하기때문에 움직이지 않는다