### Vertex Shader(버텍스 셰이더)란?
Vertex Shader는 GPU에서 실행되며, 각 Mesh의 정점 데이터를 입력받아 위치를 변환하거나 정점 속성을 처리하는 프로그램이다.

대표적으로 정점의 로컬좌표를 화면 렌더링에 필요한 클립좌표로 변환하며, Nomal, UV, Color등의 데이터를 처리하여 다음 렌더링 단계에 전달한다.

Vertex Shader는 GPU의 렌더링 파이프라인 중 하나이며, 일반적으로 레스터화 이전에 실행된다.

##### Vertex Shader 에서 처리하는 대표적인 데이터

| 데이터      | 역할                    |
| -------- | --------------------- |
| Position | 정점의 좌표 변환 및 위치 변경     |
| Nomal    | 법선 방향 변환 및 조명 계산에 활용  |
| UV       | 텍스쳐 좌표 변환 및 전달        |
| Color    | 정점 색상 처리 및 전달         |
| Tangent  | 탄젠트 방향 변환 및 노멀 매핑에 활용 |
단, 모든 Vertex Shader가 위의 데이터를 전부 처리하는 것은 아니며, Shader의 구현에 따라 필요한
데이터만 사용한다.

추가적으로, Vertex Shader는 정점마다 실행되기에 정점의 위치를 변환하여 물결, 흔들리는 풀 등의 효과를 구현할 수 있다.


### 내부 동작 원리
1. GPU가 [[Vertex Buffer]]의 정점 데이터를 읽어 Vertex Shader에 전달한다
2. 정점의 로컬 좌표에 변환 행렬을 적용하여 최종적으로 클립 좌표로 변환한다.
3. 필요에 따라 Nomal, UV, Color등의 정점 속성을 변환하거나 가공한다.
4. 변환된 정점 위치와 필요한 데이터를 다음 렌더링 단계에 전달한다.
5. GPU가 정점으로 도형을 구성하고, 레스터화를 통해 생성된 프레그먼트를 처리한다.


아래의 이미지는 Vertex Shader에서 기본적으로 수행하는 작업인 좌표 변환에 대한 예시이다
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20330%2098%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%222%22%20y%3D%2213%22%20width%3D%2271%22%20height%3D%2258%22%20rx%3D%227%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.12%22%2F%3E%3Ctext%20x%3D%2237.5%22%20y%3D%2236%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-weight%3D%22600%22%20font-size%3D%2212%22%3ELocal%3C%2Ftext%3E%3Ctext%20x%3D%2237.5%22%20y%3D%2252%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%3ESpace%3C%2Ftext%3E%3Cpath%20d%3D%22M75%2042%20h11%20m-5%20-4%20l5%204%20-5%204%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.5%22%20fill%3D%22none%22%2F%3E%3Crect%20x%3D%2287%22%20y%3D%2213%22%20width%3D%2271%22%20height%3D%2258%22%20rx%3D%227%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.12%22%2F%3E%3Ctext%20x%3D%22122.5%22%20y%3D%2236%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-weight%3D%22600%22%20font-size%3D%2212%22%3EWorld%3C%2Ftext%3E%3Ctext%20x%3D%22122.5%22%20y%3D%2252%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%3ESpace%3C%2Ftext%3E%3Cpath%20d%3D%22M160%2042%20h11%20m-5%20-4%20l5%204%20-5%204%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.5%22%20fill%3D%22none%22%2F%3E%3Crect%20x%3D%22172%22%20y%3D%2213%22%20width%3D%2271%22%20height%3D%2258%22%20rx%3D%227%22%20stroke%3D%22%2364748b%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.12%22%2F%3E%3Ctext%20x%3D%22207.5%22%20y%3D%2236%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-weight%3D%22600%22%20font-size%3D%2212%22%3EView%3C%2Ftext%3E%3Ctext%20x%3D%22207.5%22%20y%3D%2252%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%3ESpace%3C%2Ftext%3E%3Cpath%20d%3D%22M245%2042%20h11%20m-5%20-4%20l5%204%20-5%204%22%20stroke%3D%22%230D9488%22%20stroke-width%3D%221.5%22%20fill%3D%22none%22%2F%3E%3Crect%20x%3D%22257%22%20y%3D%2213%22%20width%3D%2271%22%20height%3D%2258%22%20rx%3D%227%22%20stroke%3D%22%230D9488%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.12%22%2F%3E%3Ctext%20x%3D%22292.5%22%20y%3D%2236%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-weight%3D%22600%22%20font-size%3D%2212%22%3EClip%3C%2Ftext%3E%3Ctext%20x%3D%22292.5%22%20y%3D%2252%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%3ESpace%3C%2Ftext%3E%3Ctext%20x%3D%22165%22%20y%3D%2291%22%20text-anchor%3D%22middle%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3E%EB%AA%A8%EB%8D%B8%20%EB%B3%80%ED%99%98%20%E2%86%92%20%EB%B7%B0%20%EB%B3%80%ED%99%98%20%E2%86%92%20%ED%88%AC%EC%98%81%20%EB%B3%80%ED%99%98%3C%2Ftext%3E%3C%2Fsvg%3E)

| 좌표계            | 설명                   |
| -------------- | -------------------- |
| Local Space    | 오브젝트 자체를 기준으로 한 좌표   |
| World Space    | 게임 월드의 원점을 기준으로 한 좌표 |
| View Space     | 카메라를 기준으로 한 좌표       |
| [[Clip Space]] | 투영 변환이 적용된 좌표        |
중요한점은, Vertex Shader가 최종적으로 출력하는 위치가 일반적으로 World Space가 아닌 Clip Space 라는 것 이다.


### 장점과 단점

| 장점                       | 단점                             |
| ------------------------ | ------------------------------ |
| GPU 병렬처리를 통한 효율적인 정점 연산  | 정점 수와 연산 복잡도에 따라 GPU 부하 증가     |
| GPU의 정점 변환 연산 부담 감소      | 복잡한 Shader 연산 시 성능 저하 가능       |
| 정점 변환을 통한 다양한 시각적 효과 구현  | 정점 밀도가 낮으면 세밀한 변형에 한계          |
| 정점 데이터 가공 및 후속 단계로 전달 가능 | 정점 단위의 처리이기에 픽셀 단위의 세밀한 표현에 한계 |


##### 실제 게임에서의 활용 예시
- 풀과 나무의 흔들림
- 물결 및 파도 높이 구현
- Mesh 변형으로 인한 오브젝트 찌그러짐 효과 구현
단, Vertex Shader에서 정점 위치를 변화시키더라도 CPU가 관리하는 원본 Mesh와 Colider의 형상이 자동으로 변경되는 것은 아니다.