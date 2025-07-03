**SOLID**원칙

SOLID 원칙은 [[객체지향 프로그래밍]]에서 유지보수성과 확장성을 높이기 위한 5가지 설계 원칙을
의미한다.

**S(Single responsibility) :** 단일 책임 원칙

-클래스는 한 가지 작업만 수행해야 한다

**O(Open-closed) :** 개방 폐쇠 원칙

-작동하는 방식을 변경하지 않고 클래스의 기능을 확장한다.

**L(Liskov subtitution) :** 리스코프 치환 원칙

-자식 클래스는 부모 클래스를 대체할 수 있어야 하며, 부모 클래스의 방향성을 유지해야한다.

**I(Interface segregation) :** 인터페이스 분리원칙

-인터페이스를 작게 유지하며 클라이언트는 필요한 것만 구현한다.

**D(dependency inversion) :** 의존 역전 원칙

-추상화에 의존하고, 하나의 구체 클래스에서 다른 클래스로 직접 의존 금지한다.

## SRP(Single responsibility principle, 단일 책임 원칙)

---

***모든 클래스는 하나의 책임만 가져야 하며, 클래스는 완전히 캡슐화 해야한다.***

하나의 클래스는 하나의 기능을 담당함으로써 하나의 책임을 수행하는데에 집중되도록 클래스를 

여러개 설계함으로써 한 책임의 변경으로부터 다른 책임의 변경으로의 연쇠작용을 극복할 수 있다.

> 하나의 클래스로 많은 일을 하지 말고 딱 한가지의 책임만 수행하라는 것이다.
> 

이를 잘 지키면 가독성, 확장성, 재사용성이 좋아진다

```csharp
public class Player : MonoBehaviour
{
    [SerializeField]
    float _moveSpeed;

    void Update()
    {
        MyPlayerMovement();
        MyPlayerRotation();
        MyPlayerInput();
    }

    void MyPlayerMovement()
    {
        float h = Input.GetAxis("Horizontal") * _moveSpeed;
        float v = Input.GetAxis("Vertical") * _moveSpeed;

        transform.Translate(h, 0, v);
    }

    void MyPlayerRotation()
    {
        if(Input.GetMouseButton(0))
        {
            float h = -Input.GetAxis("Mouse Y");
            float v = Input.GetAxis("Mouse X");

            transform.Rotate(h, v, 0);
        }
    }

    void MyPlayerInput()
    {
        if (Input.GetKeyDown(KeyCode.Space)) Debug.Log("점프");
        else if (Input.GetKeyDown(KeyCode.Z)) Debug.Log("공격");
    }
}
```

예를들어 이런 코드가 있다면

플레이어 코드 안에 이동, 회전이 모두 작성되어있기때문에

플레이어의 기능을 추가하면 추가할수록 코드는 복잡해지게 된다

또한, 확장이 불가능해지고 다른 코드에 재사용 하기 힘들어진다

쉽게 말해서 움직이는 코드만 있다면 다른 코드를 짤 때 움직이는 부분이 필요하면

가져다 붙일 수 있지만 많은 기능들이 하나의 코드에 들어가 있으면 코드를 재사용하기 불편해지고, 그에 따라서, 여러모로 불편하다.

## OCP(Open-Close principle, 개방 폐쇄원칙)

---

***확장에는 열려있어야 하고, 수정에는 닫혀있어야 한다***

기능 추가 요청이 오면 클래스 확장을 통해서 손쉽게 구현하면서, 확장에 따른 클래스 수정은 최소화 하도록 프로그램을 작성해야한다.

> 추상클래스와의 상속을 통한 클래스 관계 구축을 이야기한다
> 

```csharp
public class Rectangle
{
    public float _width, _height;
}

public class Circle
{
    public float _radius;
}

public class AreaCalculator
{
    public float GetRectangleArea(Rectangle obj)
    {
        return obj._width * obj._height;
    }

    public float GetCircleArea(Circle obj)
    {
        return obj._radius * obj._radius * Mathf.PI;
    }
}
```

위에 있는 예시 코드를 보면 AreaCalculator안에

도형의 넓이를 구하는 코드들이 있다

만약 도형을 몇개 더 추가한다고 가정하면 AreaCalculator안에 또 코드를 추가해야한다

그에 따라서 AreaCalculator 코드는 더 복잡해지게 되고,

새로운 클래스를 사용할 때마다 AreaCalculator또한 수정해야한다

이런 경우엔

```csharp
public abstract class Shape : MonoBehaviour
{
    public abstract string GetName();
    public abstract float GetArea();
}

public class Rectangle : Shape
{
    public float _width, _height;

    public override string GetName()
    {
        return "Rectangle";
    }

    public override float GetArea()
    {
        return _width * _height;
    }
}

public class Circle : Shape
{
    public float _radius;
    
    public override string GetName()
    {
        return "Circle";
    }

    public override float GetArea()
    {
        return _radius * _radius * Mathf.PI;
    }
}

public class AreaCalculator : MonoBehaviour
{
    [SerializeField]
    Shape[] _obj;

    private void Start()
    {
        foreach (Shape shape in _obj) GetArea(shape);
    }

    public float GetArea(Shape obj)
    {
        float ret = obj.GetArea();
        Debug.Log(obj.GetName() + " Area : " + ret);
        return ret;
    }
}
```

위 코드와 같이 상속을 이용한다면 AreaCalculator의 수정없이 

AreaCalculator의 GetArea기능을 이용할 수 있다.

요약하자면 처음 보여줬던 잘못된 예시의 코드처럼 한다면

하나의 도형을 추가할 때마다 그에 맞는 넓이 계산식을 AreaCalculator의 코드를

수정해야만 했지만

위의 코드처럼 수정한다면 AreaCalculator코드를 수정하지 않고 상속을 이용해

원하는 값을 손쉽게 얻어낼 수 있다

## LSP(Liskov substitution principle, 리스코프 치환 원칙)

---

***자식 클래스는 부모클래스를 대체할 수 있어야하며 방향성을 유지해야 한다.***

다형성의 특징을 이용하기 위해서 상위 클래스 타입으로 객체를 선언하여

하위 클래스의 인스턴스를 받으면, 업캐스팅된 상태에서 부모의 메서드를 사용해도

동작이 의도대로 흘러가야 하는것을 의미한다.

> 다형성 이용을 위하여 부모 타입으로 메서드를 실행해도 의도대록 실행되도록 구성해줘야 한다
> 

예를들어 Vehicle이란 코드를 상속받은 Car와 train이라는 자식 클래스가 있다 치면

```csharp
public class Vehicle
{
    public float _speed;
    public Vector3 _dir;

    public virtual void GoForward()
    {
        Debug.Log("탈것 전진");
    }

    public virtual void Reverse()
    {
        Debug.Log("탈것 후진");
    }

    public virtual void TurnRight()
    {
        Debug.Log("탈것 우회전");
    }

    public virtual void TurnLeft()
    {
        Debug.Log("탈것 좌회전");
    }
}
```

```csharp
public class Train : Vehicle
{
    public override void GoForward()
    {
        Debug.Log("기차 전진");
    }

    public override void Reverse()
    {
        Debug.Log("기차 후진");
    }
}
```

train의 경우 선로을 따라 이동하기 때문에 TurnRight()와 TurnLeft()기능이 필요하지 않다.

이는 Train은 TurnRIght()와 TurnLeft를 무효화 시키게 되고, 

리스코프 치환원칙에 위배된다.

부모 클래스의 기능을 무효화 시킨다는 것은 부모 클래스의 방향성을 따르지 않는다는 것이다.

```csharp
public interface ITurnable
{
    void TurnRight();
    void TurnLeft();
}

public interface IMovable
{
    void GoForward();
    void Reverse();
}

public class RoadVehicle : IMovable, ITurnable
{
    public float _speed = 100f;

    public virtual void GoForward() { }
    public virtual void Reverse() { }
    public virtual void TurnRight() { }
    public virtual void TurnLeft() { }
}

public class Car : RoadVehicle
{
    public override void GoForward()
    {
        base.GoForward();
    }

    public override void Reverse()
    {
        base.GoForward();
    }

    public override void TurnRight()
    {
        base.GoForward();
    }

    public override void TurnLeft()
    {
        base.GoForward();
    }
}

public class RailVehicle : IMovable
{
    public float _speed = 100f;

    public virtual void GoForward() { }
    public virtual void Reverse() { }
}

public class Train : RailVehicle
{
    public override void GoForward()
    {
        base.GoForward();
    }

    public override void Reverse()
    {
        base.GoForward();
    }
}
```

인터페이스로 나눠서 구현한다면 리스코프 치환원칙에 위배되지 않고 

깔끔하게 작성할 수 있다

또한 새로운 클래스로 비행기를 만든다고 친다면 하늘을 날아다닐 수 있는 인터페이스를 만들고

다른 인터페이스와 조합해 구현하면 확장성 있게 구현할 수 있다

요약하자면 인터페이스 하나에 모든 기능을 넣지 말고, 역할에 따라

인터페이스를 나눠 만들고, 조립하듯이 구현하면 된다는 것이다.

## ISP(Interface Segregation Principle,인터페이스 분리 원칙)

---

***인터페이스를 작게 유지하고 필요한 것만 구현하여 이동하지 않는 메서드에**

**의존하지 않아야 한다.***

반드시 객체가 자신에게 필요한 기능만을 가지도록 제한하는 원칙이다.

불필요한 상속과 구현을 방지함으로써 객체의 불필요한 책임을 제거한다.

큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리하여 꼭 필요한

메서드만 이용할 수 있게 해야한다.

> 인터페이스는 제약 없이 자유롭게 다중 상속(구현)이 가능하기에 분리할 수 있다면 분리하여
각 클래스 용도에 맞게 implement 하라는 설계 원칙이다.
> 

```csharp
public interface IUnit
{
    float HP { get; set; }
    int Defence { get; set; }
    float MoveSpeed { get; set; }
    float Acceleration { get; set; }
    int Strength { get; set; }
    void Die();
    void Attack();
    void Heal();
    void GoForward();
    void Reverse();
    void TurnLeft();
    void TurnRight();
}
```

위의 코드엔 IUnit이라는 인터페이스 안에 여러 기능들이 들어있다

이 코드를

```csharp
public interface IMovable
{
    float MoveSpeed { get; set; }
    float Acceleration { get; set; }
    void GoForward();
    void Reverse();
    void TurnLeft();
    void TurnRight();
}
```

이런식으로 IMovable이라는 인터페이스로 세분화 해서 구현하고 

필요한 부분에 조합해서 사용한다면 확장하기 좋아진다

요약하자면 인터페이스를 꼭 필요한 부분만 구현해서 세분화 해야한다는 것이다  또한 조립도 용이하기에 확장이 편하다

따라서 이렇게 하면 개방 폐쇄 원칙을 더 잘 지킬 수 있게 될것이란 생각이 든다

## DIP(Dependecy Inversion Principle, 의존 역전 원칙)
---

***의존 관계를 맺을 때 구체적인 것보다는 추상적인 것에 의존해야 한다.*** 

어떠한 class를 참조해서 사용해야하는 상황이 생긴다면 그 class를 직접 참조하는것이 아닌

그 대상의 상위요소(추상클래스 혹은 인터페이스)를 참조하라는 원칙이다.

의존관계를 맺을 때 변화하기 쉬운것 또는 자주 변화하는 것보다 변화하기 어렵거나 변화가 없는것에

의존하라는 것이다

> **의존 역전 원칙의 지향점은 각 클래스간의 결합도를 낮추는 것이다**
>