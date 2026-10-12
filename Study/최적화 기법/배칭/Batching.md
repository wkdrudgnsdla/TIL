### Batcing(배칭)이란?
Batcing(배칭)이란 여러 렌더링 작업을 하나의 묶음으로 처리하여 CPU의 렌더링 부담을 줄이는 
최적화 기법이다.

Unity에서 제공하는 대표적인 배칭 방법인 [[Static batching(정적배칭)]]과 [[Dynamic batching(동적배칭)]]은 
여러 Mesh의 작업을 결합해 [[Draw Call]]을 줄이는 것을 목표로 한다.

하지만 모든 배칭기술이 Draw Call 수를 줄이는 것을 목표로 하지만은 않는다.
일부 기술들은 Draw Call 수는 유지하면서 렌더링 상태 변경이나 명령 준비 비용을 줄인다.
**예시**
- SRP Batcher
- Static Batcing(조건과 렌더링 경로에 따라 다름)


### 배칭이 필요한 이유?
예시를 들어 화면에 큐브 100개가 존재한다고 가정했을 때,
큐브가 각각 개별적으로 렌더링된다면 CPU는 GPU에 여러번의 그리기 명령을 전달해야 한다.

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20150%20120%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2210%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M17%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M38%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M66%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M87%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M115%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M136%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%2210%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M17%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M38%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M66%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M87%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M115%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M136%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%2210%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M17%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M38%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M66%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M87%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.3%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M115%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%2394a3b8%22%20stroke%3D%22%2364748b%22%2F%3E%3Cpath%20d%3D%22M136%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.65%22%2F%3E%3C%2Fsvg%3E)
배칭 미적용 시, 각 Mesh 개별처리 
ex)Draw Call 9회

![](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20150%20120%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%223%22%20y%3D%222%22%20width%3D%22144%22%20height%3D%22114%22%20rx%3D%228%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.07%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.7%22%20stroke-dasharray%3D%225%203%22%2F%3E%3Crect%20x%3D%2210%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M17%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M38%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M66%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M87%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%228%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M115%203%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M136%208%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%2210%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M17%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M38%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M66%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M87%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%2246%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M115%2041%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M136%2046%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%2210%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M17%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M38%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%2259%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M66%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M87%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3Crect%20x%3D%22108%22%20y%3D%2284%22%20width%3D%2228%22%20height%3D%2225%22%20rx%3D%223%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.35%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M115%2079%20h28%20l-7%205%20h-28z%22%20fill%3D%22%235eead4%22%20fill-opacity%3D%22.65%22%20stroke%3D%22%230D9488%22%2F%3E%3Cpath%20d%3D%22M136%2084%20l7%20-5%20v25%20l-7%205z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.6%22%2F%3E%3C%2Fsvg%3E)
배칭 적용 시 여러 Mesh를 묶어서 처리
ex)Draw Call 1회

일반적으로 렌더링 할 때, CPU는 아래와 같은 작업을 수행한다.
1. 렌더링할 Mesh와 Material 확인
2. shader 및 렌더링 상태 설정
3. 필요한 버퍼 및 데이터 연결
4. 그래픽 API에 Draw Call제출
개별 랜더링을 수행했을 때 여러 오브젝트에 대해 유사한 작업이 반복될 수 있다.

추가적으로, 아래와 같은
- Material A에서 Material B로 변경
- Shader Variant 변경
- Texture나 GPU 버퍼 바인딩 변경
- Bland, Depth등 렌더링 상태 변경
과 같은 상황에서 상태변경이 일어났을때, 상태 설정 비용을 줄이는데에도 배칭이 사용된다.

배칭은 이러한 작업을 통합하거나 반복되는 상태 설정을 최소화 하여 CPU 비용을 줄인다.


### Unity에서의 대표적인 배칭기술

| 기술               | 처리방식                         | 주요 목적         |
| ---------------- | ---------------------------- | ------------- |
| Static Batching  | 정적 Mesh 데이터를 결합              | 렌더링 비용 감소     |
| Dynamic Batching | 작은 Mesh의 정점을 CPU에서 변환 및 결합   | Draw Call 감소  |
| GPU Instancing   | 동일 Mesh를 여러 인스턴스로 렌더링        | Draw Call 감소  |
| SRP Batcher      | Shader와 Material 데이터 설정을 효율화 | CPU 렌더링 비용 감소 |
Unity 공식 문서를 기준으로 구분하자면 Unity 기본제공 Draw Call Batching은 
Static Batching과 Dynamic Batcing이다.

GPU Instancing은 별도의 Draw Call 최적화 기법이며, 
SRP Batcher는 이름에 Batcher가 포함되어 있지만, Mesh를 하나로 합치는 기술은 아니다.