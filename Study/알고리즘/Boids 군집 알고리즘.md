Boids는 인공 생명 알고리즘으로, 개체 간의 상호작용을 통해 동물의 군집 움직임을 표현하는데 사용된다
ex)무리지어 다니는 새,물고기등등

Boids는 각 객체마다 아래 3가지의 규칙을 가지고 움직인다.
- separation: 무리와 충돌하지 않게 반대 방향으로 이동  
- alignment: 무리의 평균 방향으로 조향  
- cohesion: 무리의 평균 위치를 향해 이동

### 1.separation
![[TIL-46.png]]
Separation(분리)는 자기 주변의 객체들이 **붐비는 것을 피하기 위해 근처 이웃들에서 벗어나는** 규칙이다.

separation을 구현할 때, 이웃 탐지범위보다 피할 객체들의 탐지 범위가 크다면 모든 객체들을 피하기 때문에
separation 탐지 범위는 이웃 탐지 범위와 따로 존재해야 한다
![[Pasted image 20251022154223.png]]
이웃들을 피할 방향을 구하는 방법은 어떻게 구현하느냐에 따라 다 달랐기에
제가 본 글에서 한대로 이웃 -> 자신으로 향하는 벡터들의 합을 나아갈 방향으로 따라했습니다.
![[Pasted image 20251022154638.png]]

### 2.Alignment
![[TIL-48.png]]
Alignment(정렬)은 주변 객체들의 **평균 방향**으로 이동하는 규칙이다.

예시 이미지를 보면 주변 객체들(파란색)들의 방향이 11~12시 방향으로 이동하고 있기 때문에
초록색 객체도 방향을 바꾸는 것을 볼 수 있다.(초록색 막대기가 원래 방향/파란 막대기가 바뀐 방향)

### 3.Cohesion
![[TIL-50.png]]
Cohesion(응집력)은 모든 이웃 사이의 중간점을 찾아 중간점을 향해 이동하는 규칙이다


### separation 구현
```csharp
using System.Collections.Generic;
using UnityEngine;

public class Separation : MonoBehaviour
{
    public Vector3 GetDirection(Transform agent, List<Transform> neighbor)
    { // agent는 분리할 대상, neighbor 다른 이웃들의 Transform
        if (agent == null || neighbor == null || neighbor.Count == 0) 
            return Vector3.zero;//주변에 아무도 없으니 방향 0,0,0 반환

        Vector3 dir = Vector3.zero;

        foreach (var ne in neighbor)
        {
            dir += agent.position - ne.transform.position;//벗어날 방향 계산
        }

        return dir.normalized;//방향만 반환환
    }
}

```


### Alignment 구현
Alignment(정렬)의 규칙을 다시 살펴보면 Alignment(정렬)은 **이웃 객체들의 평균 방향으로 이동하는 규칙**이다.
이웃 객체들의 진행 방향벡터를 모두 더한 뒤 객체 개수로 나누면 이웃들의 평균 방향을 구할 수 있다.

```csharp
using System.Collections.Generic;
using UnityEngine;

public class Alignment : MonoBehaviour
{
    public Vector3 GetDirection(Transform agent, List<Transform> neighbor)
    {
        if (agent == null)
            return Vector3.zero;
		//주변 객체가 아예 없다면 앞으로 이동
        if (neighbor == null || neighbor.Count == 0)
            return agent.forward;

        Vector3 neighborDir = Vector3.zero;

        foreach (var ne in neighbor)
        {
            neighborDir += ne.transform.forward; // 이웃들 방향을 다 더함
        }

        return (neighborDir /= neighbor.Count).normalized; // 이웃들 방향을 수만큼 나눠 평균 반환
    }
}

```



### Coehsion 구현
Coehsion(응집력)은 **모든 이웃 사이의 중간점**(Average Position)을 찾고 중간점을 향해 이동하는 규칙이다
따라서, 이웃들의 중간점을 찾아서 본인기준으로 중간점을 향하는 벡터를 구해주면 됩니다.

```csharp
using System.Collections.Generic;
using UnityEngine;

public class Cohesion : MonoBehaviour
{
    public Vector3 GetDirection(Transform agent, List<Transform> neighbor)
    {
        if (agent == null || neighbor == null || neighbor.Count == 0)
            return Vector3.zero;//아무도 없으니 방향 0,0,0 전환

        Vector3 averagePos = Vector3.zero;

        foreach (var ne in neighbor)
        {
            averagePos += ne.position;
        }

        averagePos /= neighbor.Count; // 이웃들의 평균 위치

        return (averagePos - agent.transform.position).normalized;//향할 위치 방향 반환
    }
}

```




문제점: 매 프레임마다 객체들이 나아가야 할 방향을 정하다 보니, 사용자가 군집 이동을 예측하기가 어렵다.