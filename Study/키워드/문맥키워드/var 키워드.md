var 키워드

일반적으로 변수를 선언할 때 선언시 형식을 직접 선언한 것처럼, var 키워드를 사용하면

컴파일러가 형식을 결정해준다.

## 예시

---

> 정수형 HP에 100할당

int HP = 100;
var HP = 100;
> 

> 실수형 DMG에 10할당

float DMG = 10f;
var DMG = 10f;
> 

> 문자열형 NAME에 장경훈 할당

string NAME = “장경훈”;
var NAME = “장경훈”;
> 

## 주의사항

---

int HP;

float DMG;

string NAME;

처럼 값을 할당하지 않는것은 위의 예시처럼 형식을 선언해 주어야 한다.

var HP;

var DMG;

var NAME;

와 같이 정의해선 안되며 반드시 값을 할당하여야 한다

[[[문맥키워드 설명]]]