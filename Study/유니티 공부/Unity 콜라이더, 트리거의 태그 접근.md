
## 목차

---

A.기본상식

B.콜라이더

a.콜라이더 설명

b.복합 콜라이더

c.메시 콜라이더

d.정적 콜라이더

C.트리거

D.물리 머터리얼

E.몰랐던 용어들

## A.기본상식

---

콜라이더와 트리거는 충돌을 감지하는 기능이다.

콜라이더는 무슨 물체라도 통과할 수 없지만 

트리거는 모든 물체를 통과할 수 있다

기본적으로 감지하는데 태그가 필요, 경우에 따라 감지하지 못하게 하려면 레이어 사용

- 콜라이더 사용
    
    ```csharp
    void OnCollisionEnter (Collision collision)
    {
        if(collision.collider.CompareTag("wall_spike"))
        {
    	Debug.Log ("Player get hit spike damage");
            Debug.Log (collision.gameObject);
        }
    }
    ```
    
- 트리거 사용
    
    ```csharp
    void OnTriggerEnter (Collider other)
    {
        if (other.tag == "fireTrap")
        {
    	Debug.Log ("Player get hit Fire Damage.");
            Debug.Log (other.gameObject);
        }
    }
    ```
    

collision 은 Collision변수를 사용했고, Trigger은 Collider변수를 사용한 차이점이 있음

콜라이더가 적용된 오브젝트의 태그를 읽어 들일 때 Collision.collider을 사용해 

물체가 콜라이더 사용함을 강조, 다음 태그를 읽어들이는 함수 .compareTag를 사용

하지만 트리거의 경우 변수 뒤에 .tag를 사용하면 된다는 장점이 있다. 

## B.콜라이더

---

### a.콜라이더 설명

---

collider 컴포넌트는 물리적 충돌을 위해 게임 오브젝트의 모양을 정의한다

보이지 않는 콜라이더는 게임 오브젝트의 메시와 완전히 같을 필요는 없다

가장 간단한 콜라이더는 기본 콜라이더 타입이다.

3D에선 박스, 스피어, 캡슐이

2D에선 박스,서클이 이에 해당한다.

복합 콜라이더를 만들기 위해 이러한 콜라이더를 단일 게임 오브젝트에 원하는 만큼

추가할 수 있다

### b.복합 콜라이더

---

복합 콜라이더를 사용하면 프로세서 부하를 낮게 유지하며

게임 오브젝트의대략적인 모양을 만들 수 있다.

유연성을 강화하기 위해 자식 게임 오브젝트에 콜라이더를 추가할 수 있다

대부분의 콜라이더와 달리 복합 콜라이더는 고유의 모양을 정의하지 않는다.

대신 사용자가 설정하는 모든 박스 콜라이더 또는 폴리곤 콜라이더의

모양을 통합하여 사용한다

복합 콜라이더는 이러한 콜라이더의 버텍스를 사용하여 복합 콜라이더가 제어하는

새로운 지오메트리로 통합한다

| 프로퍼티 | 기능 |
| --- | --- |
| **Density** | 리지드바디 2D와 연관된 게임 오브젝트의 질량 계산을 바꾸려면 밀도를 변경한다. 해당 값을 0으로 설정할 경우, 관련 리지드바디 2D는 질량 중심의 계산을 포함, 모든 질량 계산에 해당하는 콜라이더 2D를 무시한다. 이 옵션은 사용자가 관련 Rigidbody 2D 컴포넌트에 있는 Use Auto Mass를 활성화할 경우에만 사용 가능함. |
| Material | 마찰이나 바운스등의 충돌과 관련된 속성을 결정하는 물리 머터리얼 |
| Is Trigger | 복합 콜라이더 2D가 트리거처럼 행동하도록 하고 싶을 경우 이 박스를 체크 |
| Used by Effector | 복합 콜라이더 2D가 연결된 Effector 2D 컴포넌트에 의해 사용되길 원할 경우 이 박스를 체크 |
| **Offset** | 콜라이더 2D 지오메트리의 로컬 오프셋을 설정한다 |
| Geometry Type | 콜라이더를 합칠 때, 선택된 콜라이더의 버텍스는 두 가지의 지오메트리 타입 중 하나로 구성됩니다. 지오메트리 타입을 Outlines 또는 Polygons로 설정하려면 이 드롭다운을 사용합니다. |
| Outlines | 에지 콜라이더 2D가 생성하는 것과 동일한 비어 있는 아웃라인으로 된 콜라이더 2D를 생성합니다. |
| Polygons | [폴리곤 콜라이더 2D](https://docs.unity3d.com/kr/2021.2/Manual/class-PolygonCollider2D.html)가 생성하는 것과 동일한 솔리드 폴리곤으로 된 콜라이더 2D를 생성한다. |
| Generation Type | 복합 콜라이더 2D가 변경되거나, 그것을 구성하고 있는 콜라이더 중 어느 하나라도 변경될 경우, 지오메트리가 생성되는 시기를 제어하는 메서드가 변경된다. |
| Synchronous | 복합 콜라이더 2D 또는 사용하고 있는 콜라이더 중 어느 하나라도 변경될 경우, Unity는 즉시 새로운 지오메트리를 생성합니다. |
| Vertex Distance | 구성되는 콜라이더에서 수집한 모든 버텍스들에 허용되는 최소 간격값을 설정합니다. 이 한도보다 가까이 있는 버텍스는 제거됩니다. 버텍스 합성의 해상도를 효과적으로 조정한다. |
| Edge Radius | 버텍스가 원 모양이 되도록 에지 주변의 반지름을 조정합니다. 이 경우 둥근 컨벡스 코너를 가지는 콜라이더 2D의 크기는 더 커지게 된다. 이 설정의 기본값은 0(반지름 없음)이다. Geometry Type이 Outlines로 설정됐을 경우에만 해당한다. |

복합 콜라이더의 가장 큰 사용 예시로는 타일맵 콜라이더가 있다.

타일 단위로 콜라이더가 생성되다 보니 무의미한 충돌을 유발하고 있다

이 문제를 해결하는게 복합 콜라이더(Composit Collider)이다

### c.메시 콜라이더

---

복합 콜라이더를 만들어도 정확히 표현하지 못하는 경우가 있다 

3D에선 메시 콜라이더를 사용해서 게임 오브젝트의 메시 모양에 정확하게 맞출 수 있다.

2D에서는 폴리곤 콜라이더가 스프라이트 크래픽 모양과 완벽히 일치하지 않지만, 원하는 디테일로

모양을 다듬을 수 있다.

이러한 콜라이더는 기본형보다 프로세서에 훨씬 더 큰 부하를 주므로 꼭 필요한 경우에만

사용해야만 한다

또한 메시 콜라이더는 다른 메시 콜라이더와 충돌할 수 없다

간단히 말하면 콜라이더 끼리 접촉해도 아무 일도 일어나지 않는다.

이러한 문제를 피하기 위해서 메시 콜라이더를 인스펙터에서 Convex로 설정할 수 있다 

이렇게 하면 볼록 다각형 모양의 콜라이더가 생성되는데

이는 원본 메시와 유사하지만 움푹 파인 부분이 전부 메워져 있다.

이 때의 장점으로는 컨벡스 메시 콜라이더가 다른 메시 콜라이더와 충돌할 수 있다는 점이다.

따라서 적합한 모양의 움직이는 캐릭터가 있을 때 이 기능을 사용할 수 있다.

그러나 일반적으로는 메시 콜라이더는 씬 지오메트리에 사용하고, 움직이는 게임 오브젝트의

모양은 복합 기본 콜라이더를 사용하여 유사하게 만드는 것이 좋다.

| 프로퍼티 | 기능 |
| --- | --- |
| Convex | 이 체크박스를 활성화하면 메시 콜라이더가 다른 메시 콜라이더와 충돌한다. Convex 메시 콜라이더는 삼각형 255개로 제한된다. |
| Is Trigger | 이 체크박스를 활성화하면 Unity가 이 콜라이더를 사용하여 이벤트를 트리거하고, 물리 엔진은 이를 무시한다. |
| Cooking Options | 메시 쿠킹 옵션을 활성화하거나 비활성화하면 물리 엔진이 메시를 처리하는 방식에 영향을 준다. |
| None | 아래에 나열된 Cooking Options들을 모두 비활성화 한다. |
| Everything | 아래에 나열된 Cooking Options들을 모두 활성화 한다. |
| Cook for Faster Simulation | 빠른 시뮬레이션을 위해 물리 엔진이 메시를 쿠킹하도록 만든다. 활성화하면 런타임 성능에 최적화된 메시를 생성한다. |
| Enable Mesh Cleaning | 물리 엔진이 메시를 청소하도록 만든다. 활성화 시 메시의 손상된 삼각형과 지오메트리 결함을 제거하여 충돌 검사의 정확도를 높인다. |
| Weld Colocated Vertices | 물리 엔진이 메시에서 동일한 위치의 버텍스를 결합하도록 만든다. 이 옵션은 런타임 충돌 피드백에 중요하다. |
| Use Fast Midphase | 물리 엔진이 가장 빠른 중간 단계 가속도 구조 및 알고리즘을 사용하도록 만든다. 필요 시 레거시 중간 단계 알고리즘으로 대체할 수 있다. |
| Material | 콜라이더가 다른 콜라이더와 상호작용하는 방법을 결정하는 물리 머티리얼에 대한 레퍼런스이다. |
| Mesh | 충돌에 사용할 메시에 대한 레퍼런스이다. |

메시 콜라이더는 게임 오브젝트에 연결된 메시를 토대로 충돌체를 재구성하고 빌드해서

연결된 트랜스폼의 프로퍼티를 읽고 포지션과 스케일을 올바르게 설정한다.

이렇게 하면 콜라이더의 모양이 게임 오브젝트에 보이는 메시의 문양과 정확히 일치해서

더욱 정확하고 실제같은 충돌을 얻을 수 있다는 장점이 있다.

하지만 이 높은 정밀도는 기본 콜라이더가(구, 박스, 캡슐등등) 관련된 충돌보다 많은

프로세싱 오버헤드를 사용하여 얻어지므로 사용을 조심하는게 좋다.

### d.정적 콜라이더

---

Rigid body컴포넌트 없이도 콜라이더를 게임 오브젝트에 추가하여 씬의 바닥, 벽등 기타의 고정된

요소를 생성할 수 있다.

이러한 콜라이더는 정적 콜라이더라고 부른다. 이와 반대로

리지드 바디가 있는 게임 오브젝트의 콜라이더는 동적 콜라이더라고 부른다.

정적 콜라이더는 동적 콜라이더와 상호작용이 가능하지만

정적 콜라이더에는 리지드바디가 없기 때문에 리스폰스하여 움직이지는 않는다.

정적 콜라이더는 물리 엔진에서 더 효율적으로 처리된다 이는 충돌계산이 비교적 단순하고

빠르게 이루어질  수 있도록 도와준다.

## C.트리거

---

스크립팅 시스템은 충돌이 일어나는 시점을 감지하고 

OnCollisionEnter 함수를 사용하여 액션이 초기화 할 수 있다.

또한 하나의 콜라이더가 충돌을 일으키지 않고 다른 콜라이더 공간에 들어가는 것을

물리엔진이 감지할 수 있게 해준다. 

콜라이더를 트리거로 설정하면 이 콜라이더는 솔리드 오브젝트로 동작하지 않으며

다른 콜라이더가 통과하도록 허용한다.

이 콜라이더의 공간에 들어오면 트리거는 트리거 오브젝트의 스크립트에 있는 

OnTriggerEnter함수를 실행한다

## D.물리 머터리얼

---

콜라이더가 상호작용 할 때 콜라이더의 표면은 각각 나타내는

머터리얼의 프로퍼티를 시뮬레이션 한다

예를들어 얼음판은 미끄러워야 하고, 고무공은 마찰이 크며 탄성이 강해야 한다

둘중 콜라이더의 형태가 변형되지는 않지만, 마찰과 탄성은 물리 머터리얼을 사용하여

설정할 수 있다.

요약하자면 그냥 콜라이더에서 조절하는 마찰력, 공기저항등이다.

## E.몰랐던 용어들

---

버텍스:모서리

지오메트리:지오메트리는 폴리곤 메시라고도 불린다

지오메트리는 3D모델링, 씬 구성, 물리 시뮬레이션 등에서 중요한 역할을 하는 개념이다.

기본적으로 지오메트리는 객체의 형상과 구조를 정의하는 요소를 말한다

이에는 메쉬,메쉬 렌더러, 프리미티브*, 콜라이더, 스켈레탈 애니메이션*, 곡면들이 있다.

- 용어 설명
    
    프리미티브*:유니티에서 기본적으로 제공하는 객체들로 큐브, 구, 원통, 평면도형등이다.
    
    이들은 간단한 지오메트리로 복잡한 모델링 작업이 없어도
    
    기본적인 형태를 만들 수 있게 해준다
    
    스켈레탈 애니메이션*:복잡한 애니메이션을 위한 지오메트리 구조이다
    
    스켈레톤 구조를 가진 메쉬를 애니메이션화 하여 캐릭터나 기타 객체의 움직임을 구현한다.