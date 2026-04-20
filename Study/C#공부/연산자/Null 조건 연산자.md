
##  Null-conditional operator?
C#에서 기본 제공하는 연산자중 하나인  null-conditional operator(널 조건 연산자)는
참조하려는 정보가 null인지 미리 체크한 후, 이에 맞는 값을 반환한다.

## 가장 많은 사용 예시
```csharp
private void PlayAnimLocal(string animName)
{
    if (string.IsNullOrEmpty(animName)) return;
    Anim?.Play(animName);
}
```
가장 많이 사용하는 예시를 하나 들자면
위와같은 메서드가 있다고 쳤을때  Anim이 null이라면 Anim의 Play 메서드를 실행하지 않습니다.

## ??연산자
?? 연산자는 a ?? b 이런 식으로 작성하며, 왼쪽 값(a)이 null이면 오른쪽 값(b)를 반환하는 연산이다
```csharp
private Image image;

private void Awake()
{
	image = GetComponent<Image>() ?? GameObject.Find("Obj").GetComponent<Image>();
}
```
이 연산자는 쓴적이 없어 예시를 직접 만들어봤어용
해당 코드로 설명을 하자면 image를 GetComponent 해봅니다.
해당 오브젝트에 이미지 컴포넌트가 없다면? => Obj라는 게임오브젝트에서 이미지를 찾아오죠

하지만 Find는 무거운 동작이란 문제가 있습니다. 
아래와 같은 방법도 사용 가능합니다.
```csharp
private GameObject obj;
private Image image;

private void Awake()
{
	image = GetComponent<Image>() ?? obj?.GetComponent<Image>(); //<= obj가 null인지 한번 더 확인
}
```

굳이 오브젝트 컴포넌트를 연결해줄 필요 없이 바로 이미지를 꼽아주는거도 가능합니다
```csharp
private GameObject obj;
private Image image;
private Image image1;

private void Awake()
{
	image = GetComponent<Image>() ?? image1; //해당 오브젝트에서 image가 null이라면 image1을 넣음
}
```
이 오브젝트에 Image가 있으면 그거 가져다 쓰고 없으면 image1 쓰겠다 라는 뜻입니다.

## ??= 연산자
??= 연산자는 a ??= b 이런 식으로 작성하며, 왼쪽 값(a)이 null이면 오른쪽 값(b)를 대입하는 연산이다.
이때, 왼쪽에 오는 값은 반드시 **값을 넣을수 있는 대상**이여야만 합니다.

이건 쉽게 이해하기 위해 풀어쓴 코드를 보여드리겠습니다.
```csharp
if(a == null)
{
	a = b;
}
```
위의 코드가 `a ??= b` 입니다.

이용 예시는 
```csharp
image ??= image1
```
image가 null이라면 image1의 값을 image에 넣겠다라는 뜻과 같습니다.

##### 아니 둘다 null이면요?
=> 예.... 당연히 null이 반환됩니다.