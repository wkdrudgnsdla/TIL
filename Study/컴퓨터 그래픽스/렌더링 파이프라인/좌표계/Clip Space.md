### Clip Space(클립 좌표계)란?
Clip Space란 **3D 공간의 정점을 화면에 렌더링하기 위해 투영 변환을 적용한 좌표계**이다.

[[Vertex Shader]]에서 정점의 위치를 변환한 결과로 사용되며, GPU는 해당 좌표를 기준으로 카메라의 
가시영역(View Frustum) 밖에 있는 도형을 잘라내는 [[Clipping]](클리핑) 작업을 수행한다.

또한 Clip Space는 일반적인 3차원 좌표(x, y, z)가 아닌 [[동차 좌표]](x, y, z, w)를 사용한다.



Clip Space의 좌표 구성

| 좌표  | 역할                         |
| --- | -------------------------- |
| x   | 화면의 가로 방향에 대응하는 좌표         |
| y   | 화면의 세로 방향에 대응하는 좌표         |
| z   | 깊이와 클리핑에 사용되는 좌표           |
| w   | 원근 나눗셈 및 클리핑 범위 결정에 사용되는 값 |
Clip Space의 좌표는 아직 화면의 픽셀 좌표가 아니며, [[원근 나눗셈]]을 거쳐 [[NDC]]로 변환된다.


### 내부 동작 원리
1. Vertex Shader에서 정점의 로컬 좌표에 모델, 뷰, 투영 행렬을 적용하여 Clip Space로 변환한다
2. GPU가 Clip Space의 좌표를 기준으로 가시 영역 밖의 도형을 잘라낸다.
3. 각 좌표의 x,y,z 값을 w로 나누어 NDC로 변환한다.
4. NDC의 좌표를 실제 화면의 뷰포트 좌표로 변환한다.
5. 화면 공간의 도형을 프래그먼트로 변환하여 후속 렌더링을 진행한다(레스터화)


이미지 변환 과정
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20330%20208%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2254%22%20y%3D%224%22%20width%3D%22222%22%20height%3D%2233%22%20rx%3D%226%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.6%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%2218%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3ELocal%20Space%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%2230%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8A%B8%20%EA%B8%B0%EC%A4%80%20%EC%A2%8C%ED%91%9C%3C%2Ftext%3E%3Cpath%20d%3D%22M165%2038%20v7%20m-3%20-3%20l3%203%203%20-3%22%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%221.5%22%2F%3E%3Crect%20x%3D%2254%22%20y%3D%2246%22%20width%3D%22222%22%20height%3D%2233%22%20rx%3D%226%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.6%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%2260%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3EWorld%20Space%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%2272%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EC%9B%94%EB%93%9C%20%EA%B8%B0%EC%A4%80%20%EC%A2%8C%ED%91%9C%3C%2Ftext%3E%3Cpath%20d%3D%22M165%2080%20v7%20m-3%20-3%20l3%203%203%20-3%22%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%221.5%22%2F%3E%3Crect%20x%3D%2254%22%20y%3D%2288%22%20width%3D%22222%22%20height%3D%2233%22%20rx%3D%226%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.6%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%22102%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3EView%20Space%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%22114%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EC%B9%B4%EB%A9%94%EB%9D%BC%20%EA%B8%B0%EC%A4%80%20%EC%A2%8C%ED%91%9C%3C%2Ftext%3E%3Cpath%20d%3D%22M165%20122%20v7%20m-3%20-3%20l3%203%203%20-3%22%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%221.5%22%2F%3E%3Crect%20x%3D%2254%22%20y%3D%22130%22%20width%3D%22222%22%20height%3D%2233%22%20rx%3D%226%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.16%22%20stroke%3D%22%230D9488%22%20stroke-opacity%3D%22.6%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%22144%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3EClip%20Space%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%22156%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%ED%88%AC%EC%98%81%20%EB%B3%80%ED%99%98%EB%90%9C%20%EB%8F%99%EC%B0%A8%20%EC%A2%8C%ED%91%9C%3C%2Ftext%3E%3Cpath%20d%3D%22M165%20164%20v7%20m-3%20-3%20l3%203%203%20-3%22%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%221.5%22%2F%3E%3Crect%20x%3D%2254%22%20y%3D%22172%22%20width%3D%22222%22%20height%3D%2233%22%20rx%3D%226%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.09%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.6%22%2F%3E%3Ctext%20x%3D%2275%22%20y%3D%22186%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%20fill%3D%22currentColor%22%3ENDC%3C%2Ftext%3E%3Ctext%20x%3D%2275%22%20y%3D%22198%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3EW%EB%A1%9C%20%EB%82%98%EB%88%88%20%EC%A0%95%EA%B7%9C%ED%99%94%20%EC%A2%8C%ED%91%9C%3C%2Ftext%3E%3C%2Fsvg%3E)


### 특징과 제약

| 특징                     | 제약                           |
| ---------------------- | ---------------------------- |
| 4차원 동차 좌표 사용           | 화면의 픽셀 좌표로 직접 사용할 수 없음       |
| 카메라 가시 영역을 기준으로 클래핑 가능 | 그래픽 API에 따라 깊이 좌표 범위가 다름     |
| 원근 투영에 필요한 w 값 유지      | NDC 변환을 위한 원근 나눗셈 필요         |
| 가시 영역 밖 도형을 잘라낼 수 있음   | 클리핑만으로 불필요한 렌더링 연산을 제거할 수 없음 |
| 원근 투영과 직교 투영 모두 지원     | 좌표값의 해석이 투영 방식에 따라 달라짐       |
