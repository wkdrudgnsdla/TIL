### Index Buffer(인덱스 버퍼)란?
Index Buffer는 [[Vertex Buffer]]에 저장된 정점들을 어떤 순서로 참조하여 도형을 구성할지 지정하는 인덱스 데이터를 저장하는 버퍼이다.

각 정점의 데이터를 직접 저장하는 것이 아닌, 정점을 참조하는 인덱스 번호를 저장하여 동일한 정점을 여러번 재사용 할 수 있도록 한다.

이를 통해 중복되는 정점 데이터의 저장을 줄이고, GPU가 정점 데이터를 효율적으로 처리하도록 도울 수 있다.


#### Index Buffer에 저장되는 데이터
예를들어 4개의 정점으로 이루어진 사각형을 렌더링한다고 가정하였을 때,
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20320%20170%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20d%3D%22M82%2025%20H238%20V145%20H82Z%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.12%22%20stroke%3D%22%2364748b%22%20stroke-width%3D%221.8%22%2F%3E%3Cpath%20d%3D%22M82%2025%20L238%20145%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%222%22%20stroke-dasharray%3D%225%204%22%2F%3E%3Ccircle%20cx%3D%2282%22%20cy%3D%2225%22%20r%3D%225%22%20fill%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%2265%22%20y%3D%2218%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2212%22%20font-weight%3D%22600%22%3EV0%3C%2Ftext%3E%3Ccircle%20cx%3D%22238%22%20cy%3D%2225%22%20r%3D%225%22%20fill%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%22256%22%20y%3D%2218%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2212%22%20font-weight%3D%22600%22%3EV1%3C%2Ftext%3E%3Ccircle%20cx%3D%22238%22%20cy%3D%22145%22%20r%3D%225%22%20fill%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%22256%22%20y%3D%22157%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2212%22%20font-weight%3D%22600%22%3EV2%3C%2Ftext%3E%3Ccircle%20cx%3D%2282%22%20cy%3D%22145%22%20r%3D%225%22%20fill%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%2265%22%20y%3D%22157%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2212%22%20font-weight%3D%22600%22%3EV3%3C%2Ftext%3E%3Ctext%20x%3D%22176%22%20y%3D%2258%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20fill%3D%22currentColor%22%3E%EC%82%BC%EA%B0%81%ED%98%95%20A%3C%2Ftext%3E%3Ctext%20x%3D%22145%22%20y%3D%22113%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20fill%3D%22currentColor%22%3E%EC%82%BC%EA%B0%81%ED%98%95%20B%3C%2Ftext%3E%3C%2Fsvg%3E)
```
Vertex Buffer
V0, V1, V2, V3

Index Buffer
삼각형 A : [0, 1, 2]
삼각형 B : [0, 2, 3]
```
사각형을 구성하기 위해선 삼각형 2개가 필요하며, 인덱스를 사용하지 않고 각 삼각형의 정점을 별도로 저장한다면 총 6개의 정점데이터가 필요하다.

하지만 Index Buffer를 사용하였을 때 4개의 정점 데이터를 저장하고 6개의 인덱스로 정점을 참조하여 동일한 사각형을 구성할 수 있다.
(UV나 Nomal등 정점 속성이 같은 동일한 위치의 정점데이터 기준)


### 내부 동작원리
1. Mesh의 정점을 어떤 순서로 참조할지 인덱스 데이터를 구성한다
2. 생성된 인덱스 데이터를 GPU에서 사용할 수 있는 Index Buffer에 업로드 한다.
3. 렌더링 할 때 사용할 Vertex Buffer와 Index Buffer를 설정한다
4. GPU가 Index Buffer의 인덱스를 읽어 Vertex Buffer의 해당 정점 데이터를 참조한다.
5. 참조한 정점을 처리하고 지정된 순서에 따라 삼각형등 도형을 구성하여 렌더링한다.
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20320%20154%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%228%22%20y%3D%228%22%20width%3D%22135%22%20height%3D%22124%22%20rx%3D%227%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%2364748b%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%2228%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3EIndex%20Buffer%3C%2Ftext%3E%3Crect%20x%3D%2220%22%20y%3D%2239%22%20width%3D%22110%22%20height%3D%2227%22%20rx%3D%224%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.2%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%2257%22%20fill%3D%22currentColor%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0%2C%201%2C%202%3C%2Ftext%3E%3Crect%20x%3D%2220%22%20y%3D%2276%22%20width%3D%22110%22%20height%3D%2227%22%20rx%3D%224%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.2%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%2294%22%20fill%3D%22currentColor%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%3E0%2C%202%2C%203%3C%2Ftext%3E%3Cpath%20d%3D%22M146%2069%20H176%20M170%2063%20L176%2069%20L170%2075%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%222%22%20fill%3D%22none%22%2F%3E%3Crect%20x%3D%22180%22%20y%3D%228%22%20width%3D%22132%22%20height%3D%22124%22%20rx%3D%227%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%22246%22%20y%3D%2228%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3EVertex%20Buffer%3C%2Ftext%3E%3Ctext%20x%3D%22193%22%20y%3D%2247%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3EV0%20%3A%20%EC%A0%95%EC%A0%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22193%22%20y%3D%2269%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3EV1%20%3A%20%EC%A0%95%EC%A0%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22193%22%20y%3D%2291%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3EV2%20%3A%20%EC%A0%95%EC%A0%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22193%22%20y%3D%22113%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3EV3%20%3A%20%EC%A0%95%EC%A0%90%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Ctext%20x%3D%22160%22%20y%3D%22149%22%20fill%3D%22currentColor%22%20font-size%3D%2210%22%20text-anchor%3D%22middle%22%3E%EC%9D%B8%EB%8D%B1%EC%8A%A4%EB%A5%BC%20%EC%9D%B4%EC%9A%A9%ED%95%9C%20%EC%A0%95%EC%A0%90%20%EC%B0%B8%EC%A1%B0%3C%2Ftext%3E%3C%2Fsvg%3E)
Index Buffer를 사용하면 여러 삼각형에서 동일한 정점을 참조할 수 있으며, GPU의 정점 캐시를 활용하여 중복된 정점 처리 비용을 줄일 수도 있다.


### 장점과 단점

| 장점                        | 단점                        |
| ------------------------- | ------------------------- |
| 동일한 정점 데이터 재사용 가능         | 인덱스 데이터 저장을 위한 추가 메모리 필요  |
| 중복 정점 데이터 감소로 메모리 절약 가능   | 정점 재사용이 적으면 메모리 이점 감소     |
| GPU 정점 캐시를 통한 중복 연산 감소 가능 | 비효율적인 인덱스 순서로 캐시 효율 감소 가능 |
| 인덱스를 통해 도형 구성 관리 가능       | 정점 구성 변경 시 인덱스 데이터 갱신 필요  |
