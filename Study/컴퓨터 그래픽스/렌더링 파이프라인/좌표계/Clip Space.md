### Clip Space(클립 좌표계)란?
Clip Space란 **3D 공간의 정점을 화면에 렌더링하기 위해 투영 변환을 적용한 좌표계**이다.

[[Vertex Shader]]에서 정점의 위치를 변환한 결과로 사용되며, GPU는 해당 좌표를 기준으로 카메라의 
가시영역(View Frustum) 밖에 있는 도형을 잘라내는 Clipping(클리핑) 작업을 수행한다.

또한 Clip Space는 일반적인 3차원 좌표(x, y, z)가 아닌 동차좌표(x, y, z, w)를 사용한다.



Clip Space의 좌표 구성

| 좌표  | 역할                         |
| --- | -------------------------- |
| x   | 화면의 가로 방향에 대응하는 좌표         |
| y   | 화면의 세로 방향에 대응하는 좌표         |
| z   | 깊이와 클리핑에 사용되는 좌표           |
| w   | 원근 나눗셈 및 클리핑 범위 결정에 사용되는 값 |
Clip Space의 좌표는 아직 화면의 픽셀 좌표가 아니며, 원근 나눗셈(Perspective Division)을 거쳐 NDC(Normalized Device Coordinates)로 변환된다.


### 내부 동작 원리
