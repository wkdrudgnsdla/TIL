외부에서 어떤 클래스 내부의 변수를 사용할 때 해당 변수를 이곳 저곳에서 가져가

사용하는 곳에서마다 조건을 보는건 비효율적이다.

- **변수의 값이 항상 올바르도록(일정 수치이도록) 강제하여 확실히 하기 원하는 경우**
- **변수의 값이 변경되었을 때를 감지하여 이 값에 영향을 받는 함수등을 실행시키기 원하는 경우**

위의 상황등에서 사용할 수 있는것이 바로 프로퍼티 이다.

### get? set?

- **get: 외부에서 해당 프로퍼티에 접근하여 읽어야 하는 상황에서 호출된다.**
- **set: 외부에서 해당 프로퍼티에 접근하여 값을 할당하는 상황에서 호출된다.**
- 변수처럼 선언되지만 함수처럼 중괄호로 묶인다.

### 2.정해진 범위 안의 값만 할당하기.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Property : MonoBehaviour
{
    [SerializeField]
    private int _num;

    public int NUM
    {
        get
        {
            return _num;
        }

        set
        {
            if(_num >= 0 && _num <= 100)
            {
                _num = value;
            }
        }
    }

    public void Start()
    {
        NUM = 0;
        _num = 0;
    }

    public void Update()
    {
        if (Input.GetKeyDown(KeyCode.K))
        {
            Debug.Log(_num);
        }
    }

}

```

이런식으로 private로 선언된 _num을 

```csharp
using UnityEngine;

public class propertyTest1 : MonoBehaviour
{
    private Property property;

    void Awake()
    {
        property = GameObject.Find("1234").GetComponent<Property>();
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            property.NUM = 100;
            //property.NUM = 10000;값이 할당되나 프로퍼티 내부에서 필터링 된다.
        }
    }
}

```

이런식으로 다른 코드에서 바꿔줌으로써 `Property` 코드의 프로퍼티 내부에서 set 조건을 통하여

값을 할당한다

### 3.자동구현 프로퍼티

- **조건이 있는 것이 아닌, 단순히 변수 값 할당과 반환만 할 것이라면 자동 구현 프로퍼티 사용이
편하다**
- **get set 접근자에 아무것도 쓰지 않으면 된다.**
- **클래스 내부 변수에 대한 할당, 반환이 아닌 프로퍼티 자체를 사용하는 것이다.**

```csharp
public class EX : MonoBehaviour
{
		public float Num{ get; set;}
}
```

이게 자동구현 프로퍼티로 자주 사용하던 `public float Num;` 과 비슷하다

### 4. 프로퍼티 사용 팁

```csharp
public float Num{ get; set;} = 100f; //선언과 동시에 필드에 대한 초기화가 가능하다

EX ex = new EX() { Num = 100f }; //클래스 객체를 생성할때도 초기화 가능

ver ex = new { Num = 100f };//무명 형식 지원, 하지만 한번 할당하면 변경 불가
```

### 5.필드와 자동구현 프로퍼티의 차이

1. `public float num;` 과 같은 형식은 필드이다.
    - 별도의 접근자 로직이 없기에 값에 대한 제어를 할 수 없다.
    - 데이터 캡슐화의 관점에서 보면 바로 접근 가능한 필드는 나중에 내부 구현을 바꾸기 어렵게 만들수 있다.
2. `public float Num{ get; set;}`  과 같은 형식은 자동구현 프로퍼티이다
    - 컴파일러가 내부적으로 보이지 않는 필드를 자동 생성하여   get과 set 접근자를 통해
    값을 읽고 쓸 수 있게 해준다
    - 나중에 getter과 setter에 추가 로직을 넣고싶을 때 쉽게 수정할 수 있다.
    - 캡슐화가 가능하여 직접 필드에 접근하는 것을 제한하고 프로퍼티를 통해
    간접 접근하게 할 수 있다

**요약**

- **프로퍼티**는 내부에 자동 생성된 필드를 통해 간접적으로 데이터를 다루기에, 나중에 로직 추가나 내부 구현 변경 시 유연성이 높다.
- **필드**는 바로 데이터를 저장하는 변수로, 단순하지만 캡슐화나 데이터 제어에 한계가 있다.
[[문맥키워드 설명]]
