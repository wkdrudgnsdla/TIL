### Static batching이란?
정적배칭(Static batching)은 움직이지 않는 여러 GameObject의 Mesh 데이터를 결합하여 
렌더링 비용을 줄이는 기법이다.

해당 기법은 건물, 벽, 바닥, 고정된 장식물등 위치가 변하지 않는 오브젝트에 적합하다.

### 내부 동작 원리
예를들어 각각 다른 위치에 존재하는 3개의 큐브가 있다고 가정하였을 때,
각 큐브는 자신의 로컬좌표를 기준으로 Mesh의 정점 정보를 가지고 있다.

유니티는 정적 배칭 과정에서 아래와 같은 과정을 수행한다.
1. 배칭할 Mesh의 정점을 월드좌표로 변환한다.
2. 변환된 정점 데이터를 하나의 [[Vertex Buffer]]에 구성한다.
3. 각 Mesh의 Index 정보를 공유 Vertex Buffer에 구성한다.
4. 렌더링 시 해당 공유 버퍼를 이용하여 배칭 가능한 오브젝트를 처리한다.
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20320%20166%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2216%22%20y%3D%228%22%20width%3D%2278%22%20height%3D%2246%22%20rx%3D%226%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.15%22%2F%3E%3Ctext%20x%3D%2255%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%20fill%3D%22currentColor%22%3EMesh%20A%3C%2Ftext%3E%3Cpath%20d%3D%22M55%2056%20V77%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.8%22%2F%3E%3Crect%20x%3D%22121%22%20y%3D%228%22%20width%3D%2278%22%20height%3D%2246%22%20rx%3D%226%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.15%22%2F%3E%3Ctext%20x%3D%22160%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%20fill%3D%22currentColor%22%3EMesh%20B%3C%2Ftext%3E%3Cpath%20d%3D%22M160%2056%20V77%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.8%22%2F%3E%3Crect%20x%3D%22226%22%20y%3D%228%22%20width%3D%2278%22%20height%3D%2246%22%20rx%3D%226%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.15%22%2F%3E%3Ctext%20x%3D%22265%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%20fill%3D%22currentColor%22%3EMesh%20C%3C%2Ftext%3E%3Cpath%20d%3D%22M265%2056%20V77%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.8%22%2F%3E%3Cpath%20d%3D%22M55%2077%20H265%20M160%2077%20V99%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.8%22%20fill%3D%22none%22%2F%3E%3Cpath%20d%3D%22M155%2094%20L160%20100%20L165%2094%22%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%221.8%22%2F%3E%3Crect%20x%3D%2229%22%20y%3D%22102%22%20width%3D%22262%22%20height%3D%2251%22%20rx%3D%227%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.1%22%20stroke%3D%22%230D9488%22%2F%3E%3Crect%20x%3D%2240%22%20y%3D%22112%22%20width%3D%2278%22%20height%3D%2230%22%20rx%3D%224%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.3%22%2F%3E%3Crect%20x%3D%22121%22%20y%3D%22112%22%20width%3D%2278%22%20height%3D%2230%22%20rx%3D%224%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.3%22%2F%3E%3Crect%20x%3D%22202%22%20y%3D%22112%22%20width%3D%2278%22%20height%3D%2230%22%20rx%3D%224%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.3%22%2F%3E%3Cg%20font-size%3D%2210%22%20fill%3D%22currentColor%22%20text-anchor%3D%22middle%22%3E%3Ctext%20x%3D%2279%22%20y%3D%22130%22%3EA%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22160%22%20y%3D%22130%22%3EB%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22241%22%20y%3D%22130%22%3EC%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)


### 왜 성능이 개선되는지?
별도의 Mesh를 렌더링 할 때 발생할 수 있는 버퍼 연결과 렌더링 상태 설정을 줄일 수 있기 때문이다.
또한 정적배칭은 런타임 매 프레임마다 각 Mesh의 정점을 CPU에서 월드좌표로 변환할 필요가 없다.

여기서 중요한점은 Static batching을 사용했다고 해서 여러개의 Mesh가 반드시 [[Draw Call]]이 1회로
렌더링된다는 보장은 없다는 것이다.

Unity 버전과 렌더링 경로에 따라 Draw Call을 통합하거나, 공유 버퍼를 사용하며 개별 Draw Call의 
상태 변경 비용을 줄이는 방식으로 처리될 수 있다.

위와 같은 이유로 인해 정적배칭의 효과는 Draw Call의 수 뿐만이 아닌, CPU 렌더링 시간으로도 판단해야 한다.


### 장점과 단점

| 장점                | 단점                        |
| ----------------- | ------------------------- |
| CPU 렌더링 비용 감소     | 결합된 Mesh 데이터로 메모리 증가      |
| 작은 Mesh에만 제한되지 않음 | 개별 오브젝트 이동에 부적합           |
| 개별 Randerer 컬링 가능 | 배칭 조건이 맞지 않다면 분리          |
| 빌드시 사전처리 가능       | 동일 Mesh 반복 사용 시 데이터 복제 가능 |
