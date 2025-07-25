### is, as?
`is`와 `as`는 각각 타입의 검사와 변환을 위한 연산자이다.


#### is연산자
`is`연산자는 객체가 특정 타입인지를 검사하여 `bool`값을 반환한다.

###### 예시
```csharp
int num = 10;
if(num is int n)
{
	Debug.Log(n); //패턴매칭으로 인해 선언된 n도 num의 10값을 가지게된다.
}
```


#### as 연산자
캐스팅 할 때 형변환이 가능하다면 형변환을 수행하고, 불가능하다면 `null`을 리턴하는 연산자이다.

###### 예시
```csharp
GameObject obj = "안녕"; //오브젝트에 문자열이 들어가 오류 발생
String str = obj as String; //형변환 실행

if (s != null)
{
    Debug.Log(s);  //반환에 성공하였다면 실행
}
else
{
	Debug.Log("문자열이 아님");
}
```
이렇게 `as` 연산자는 예외 없이 타입 캐스트를 시도하고, 캐스트 성공 여부는 `null` 체크로 판단할 수 있다.