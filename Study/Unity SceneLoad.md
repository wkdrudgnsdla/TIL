유니티에서 씬을 로드하려면 SceneManager를 사용한다

종류는 동기 Single방식, 동기 Additive방식, 비동기 방식이 있다

## Single방식

Single방식은 현재의 씬을 언로드하고 새로운 씬을 로드하는 방식이다

Single방식은 현재의 씬을 언로드 즉 닫고 새로운 씬을 로드하는 방식이다

따라서 씬을 닫았다 다시 열게되면 초기의 씬 상태가 불러와지게 된다

사용방식은 아래와 같다

```csharp
SceneManager.LoadScene("Villige");
```

## Additive방식

(에디티브)

Additive방식은 현재 씬을 유지한 채 추가적인 씬을 로드한다

Additive방식은 씬 모드라는 것을 사용해야 하는데 방식은 아래와 같다

```csharp
SceneManager.LoadScene("Villige",LoadSceneMode.Additive);
```

이렇게 사용하면 원래 있던 씬을 유지시킨채로 다른 씬을 불러올 수 있다.

Additive방식으로 씬을 불러오게 되면 원래 있었던 씬이 Active Scene이 된다 

따라서 Instatiate와 같은 동적인 생성을 하게되면 생성된 오브젝트들은

Active Scene에 귀속되게 된다.

이는 Active Scene을 직접 코드로 바꿔줘야하는데

```csharp
SceneManager.SetActiveScene(SceneManager.GetSceneByName("Villige");
```

위와 같은 식으로 바꿔주어야 한다

---

## 비동기 방식

(에이싱크)

유니티는 비동기 방식으로 씬을 로드할 수 있어, 로드작업을 실행하는 동안

다른 작업을 실행할 수 있다

또한 LoadScene은 중지,지연과 같은문제가 발생할 수 있지만

LoadSceneAsync()는 AsyncOperation클래스를 리턴값으로 전달한다

AsyncOperation프로퍼티로 현재 씬의 호출 상태를 알수 있다

아래는 AsyncOperation의 각 프로퍼티에 대한 설명이다

- allowSceneActivation : 씬을 활성화 시킨다.
- isDone : 씬의 호출이 완료 되었는지를 확인한다. 완료되면 isDone은 true 값을 가진다.
- progress : 씬의 호출이 얼마나 이루어 졌는지 알려주는 값이다. 0~1.0사이의 값을 가진다.
    
    1.0이 되면, isDone은 True로 값을 가진다.
    
- priority : 멀티씬을 로드할 때 씬을 호출하는 순서를 나타낸다.

사용방법

```csharp
AsyncOperation asyncOper = SceneManager.LoadSceneAsync("Loading");
```

LoadScene()방식은 동기방식으로 불러올 씬을 한꺼번에 불러오고 기다리는 방식으로

불러오는 도중에 다른 작업을 할 수 없게 되지만 

LoadSceneAsync()의 방식은 비동기 방식으로 씬을 불러오는 도중에도 다른 작업이 가능하다.

---