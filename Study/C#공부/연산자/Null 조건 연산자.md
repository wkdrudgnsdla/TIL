
##  Null-conditional operator?
C#에서 기본 제공하는 연산자중 하나인  null-conditional operator(널 조건 연산자)는
참조하려는 정보가 null인지 미리 체크한 후, 이에 맞는 값을 반환한다.


```csharp
private void PlayAnimLocal(string animName)
{
    if (string.IsNullOrEmpty(animName)) return;
    Anim?.Play(animName);
}
```
가장 많이 사용하는 예시를 하나 들자면
위와같은 메서드가 있다고 쳤을때  `Anim` 이 null이라면 Play메서드를 실행하지 않습니다