유니티에서 오브젝트를 생성할때 Instantiate를 사용하고 삭제할때는 Destroy를 사용하는데
**Instantiate와 Distroy는 상당한 비용**을 먹는다.

Instantiate는 **메모리를 할당**하고 **리소스를 로드**하는등의 초기화 과정이 필요하고
Destroy는 **파괴 이후 가비지 컬렉팅**으로 인한 프레임드랍이 발생할 수 있다.

여기에서 문제가 발생하게 되는데 총알같이 자주 발생하고 삭제되어야하는 오브젝트의 경우
치명적인 결과를 초래할 수 있다.

이때 사용하는것이 바로 오브젝트 풀링인데, 자주 사용할 오브젝트를 **미리 생성**하여 놓고 **사용할때 오브젝트 풀에 빌려서 사용**하고 **삭제할 때는 오브젝트 풀한테 돌려줌**으로써 단순히 **오브젝트를 활성화 비활성화** 하는 개념이다

## 구현
---

**풀링 담당 코드**

```csharp
public class Objectpooling : MonoBehaviour
{
    public GameObject bulletPrefab;
    public int initialSize = 20;
    public float bulletSpeed = 10f;

    private Queue<GameObject> pool = new Queue<GameObject>();
    public void Enqueue(GameObject obj) => pool.Enqueue(obj);//다 쓴 총알을 Queue의 뒷쪽에 넣어주기

    private void Awake()
    {
        for (int i = 0; i < initialSize; i++)
        {
            var b = Instantiate(bulletPrefab, transform);
            b.SetActive(false);
            pool.Enqueue(b);
        }
    }

    private void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            GetBullet(gameObject.transform.position, gameObject.transform.rotation);
        }
    }

    public GameObject GetBullet(Vector3 position, Quaternion rotation)
    {
        if (pool.Count == 0)
        {
            var extra = Instantiate(bulletPrefab, transform);
            extra.SetActive(false);
            pool.Enqueue(extra);
        }

        var bullet = pool.Dequeue();
        bullet.transform.SetPositionAndRotation(position, rotation);
        bullet.SetActive(true); 
        return bullet;
    }
}

```

미리 총알을 20개 생성해 놓고, 마우스를 클릭하면 `GetBullet()`이 실행됩니다.

1. 큐에 비활성화 된 총알이 남아있지 않다면 

 `var extra = Instantiate(bulletPrefab, transform);`

로 새 총알을 생성한 뒤,  Active를 false로 하여 큐에 넣습니다.

이러한 기능을 통해 풀이 부족해도 에러가 일어나지 않고 총알을 꺼낼 수 있습니다.

2. `var bullet = pool.Dequeue();` 부분에선 `Dequeue()` 를 통해

큐에 가장 오래 대기중이던 총알을 꺼냅니다. 꺼낸 시접에서 총알은 비활성화 상태 입니다.

3.메서드의 인자로 받은 position과 rotation을 적용하여, 총구 위치와 방향에 맞게 총알을 조정합니다.

4.`bullet.SetActive(true);` 를 통해 총알을 활성화 시킵니다.

이때, 비활성화 상태이던 총알이 활성화 되었으므로 Bullet의 `OnEnable()` 이 호출됩니다.

**총알부분**

```csharp
public class Bullet : MonoBehaviour
{
    public float speed;
    public float activeTime;

    public bool Active = false;

    public Objectpooling pool;

    private void Awake()
    {
        pool = FindObjectOfType<Objectpooling>();
    }

    private void Start()
    {
        activeTime = 0;
        speed = 20f;
    }

    private void OnEnable()
    {
        Active = true;
    }

    private void Update()
    {
        transform.Translate(Vector3.left * speed * Time.deltaTime);

        if (Active)
        {
            activeTime += Time.deltaTime;
        }

        if (activeTime >= 3)
        {
            gameObject.SetActive(false);
        }

    }

    private void OnDisable()
    {
        Active = false;
        activeTime = 0;

        if (pool != null)
        {
            pool.Enqueue(this.gameObject);
        }
    }

    private void OnTriggerEnter(Collider other)
    {
        if (!other.gameObject.CompareTag("Gun"))
        {
            gameObject.SetActive(false);
        }
    }
}
```

1.총알은 생성되어있는 상태이고 위 `Objectpolling` 코드를 통해 활성화 된 총알은

`OnEnable()` 이 먼저 실행되어 `Active` 를 활성화 시키고 이는 3초뒤에 자동으로 오브젝트가 비활성화 되게 합니다.

2.오브젝트가 총이 아닌 물체에 닿았을 때 오브젝트를 비활성화 합니다. 

1. OnDisable을 통해 오브젝트가 비활성화가 되면 위 `Objectpooling` 코드의 Enqueue를 통해 해당 게임오브젝트를 다시 큐에 넣습니다.

## 정리

---

이 코드는 총알이 무한히 반복, 삭제하는것 대신, 오브젝트를 활성화, 비활성화 하며

눈에 보이는 부분은 똑같이 보이지만 메모리 측면에서 매우 유리한 측면이 존재합니다.

큐에 있는 사용 가능한 오브젝트가 없을땐, 새로 오브젝트를 만들어 큐에 넣고, 이는 큐의 크기를 변경하여, 매우 효율적으로 오브젝트를 관리할 수 있습니다.

또한 사용한 오브젝트는 삭제하는것이 아닌 비활성화하고 큐에 다시 넣는 과정을 통하여 

생성한 일정 오브젝트를 가지고 무한한 사용이 가능합니다.

이는 유니티에서 생성, 디스트로이 과정에서 발생하는 상당한 비용을 효율적으로 줄여주는데 효과적입니다.

![화면 녹화 중 2025-06-11 160154.gif](%ED%99%94%EB%A9%B4_%EB%85%B9%ED%99%94_%EC%A4%91_2025-06-11_160154.gif)