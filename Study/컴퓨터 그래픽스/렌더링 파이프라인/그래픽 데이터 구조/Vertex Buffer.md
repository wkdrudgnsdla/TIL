### Vertex Buffer(정적 버퍼)란?
Vertex Buffer는 3D 오브젝트를 구성하는 정점(Vertex) 데이터를 GPU가 사용할 수 있도록 저장하는 메모리 영역이다.

Unity에서 Mesh는 여러 정점으로 구성되며, 각 정점은 위치 뿐만 아니라 다양한 정보를 가지고 있다.

대표적으로는 아래와 같은 정보를 가지고있다.

| 정점 데이터   | 설명                |
| -------- | ----------------- |
| Position | 정점의 위치 좌표         |
| Nomal    | 정점의 법선 방향         |
| UV       | 텍스쳐의 좌표           |
| Color    | 정점의 색상            |
| Tangent  | 노멀 맵핑에 사용되는 법선 정보 |

### 내부 동작원리
1. Unity에서 Mesh를 생성하거나 불러오면서 정점 데이터를 준비한다.
2. Unity가 Mesh의 정점 데이터를 GPU에서 사용할 수 있는 Vertex Buffer에 업로드한다.
3. 렌더링 할 때 GPU가 사용할 Vertex Buffer를 설정한다.
4. GPU가 Vertex Buffer의 정점 데이터를 읽고 Vertex Shader에서 처리한다.
5. Mesh의 정점 데이터가 변경되면 필요한 데이터를 GPU에 다시 업로드 할 수 있다.

![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20330%20100%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%224%22%20y%3D%2214%22%20width%3D%2282%22%20height%3D%2257%22%20rx%3D%227%22%20stroke%3D%22%230D9488%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.09%22%2F%3E%3Ctext%20x%3D%2245%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%3ECPU%3C%2Ftext%3E%3Ctext%20x%3D%2245%22%20y%3D%2253%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%229%22%3EMesh%20%EB%8D%B0%EC%9D%B4%ED%84%B0%3C%2Ftext%3E%3Crect%20x%3D%22124%22%20y%3D%2214%22%20width%3D%2294%22%20height%3D%2257%22%20rx%3D%227%22%20stroke%3D%22%230D9488%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.09%22%2F%3E%3Ctext%20x%3D%22171%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%3EVertex%20Buffer%3C%2Ftext%3E%3Ctext%20x%3D%22171%22%20y%3D%2253%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%229%22%3EGPU%20%EC%A0%91%EA%B7%BC%20%EA%B0%80%EB%8A%A5%3C%2Ftext%3E%3Crect%20x%3D%22260%22%20y%3D%2214%22%20width%3D%2266%22%20height%3D%2257%22%20rx%3D%227%22%20stroke%3D%22%230D9488%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.09%22%2F%3E%3Ctext%20x%3D%22293%22%20y%3D%2235%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%2211%22%20font-weight%3D%22600%22%3EGPU%3C%2Ftext%3E%3Ctext%20x%3D%22293%22%20y%3D%2253%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%229%22%3EVertex%20Shader%3C%2Ftext%3E%3Cg%20stroke%3D%22%230D9488%22%20fill%3D%22none%22%20stroke-width%3D%222%22%3E%3Cpath%20d%3D%22M88%2042%20H120%20m-6%20-5%20l6%205%20-6%205%22%2F%3E%3Cpath%20d%3D%22M220%2042%20H256%20m-6%20-5%20l6%205%20-6%205%22%2F%3E%3C%2Fg%3E%3Ctext%20x%3D%22104%22%20y%3D%2285%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%229%22%3E%EC%97%85%EB%A1%9C%EB%93%9C%3C%2Ftext%3E%3Ctext%20x%3D%22239%22%20y%3D%2285%22%20text-anchor%3D%22middle%22%20fill%3D%22currentColor%22%20font-size%3D%229%22%3E%EC%9D%BD%EA%B8%B0%3C%2Ftext%3E%3C%2Fsvg%3E)
한번 GPU에 업로드 된 정점 데이터는 변경되지 않는 한 일반적으로 매 [[Draw Call]]마다 다시 업로드 할 필요가 없다.

이 때, 중요한 점은 오브젝트의 Transform이 변경된다고 해서 반드시 Vertex Buffer의 정점 데이터를 수정해야 하는 것은 아니라는 점이다.

일반적인 Mesh 렌더링에서는 기존 정점 데이터를 유지하며 GPU가 Transform 행렬을 이용해 좌표를 변환할 수 있다.


### 장점과 단점

| 장점                          | 단점                         |
| --------------------------- | -------------------------- |
| GPU가 정점 데이터에 효율적으로 접근 가능    | 정점 데이터 저장을 위한 메모리 필요       |
| 저장된 정점 데이터를 반복적으로 재사용 가능    | 정점 데이터 변경 시 갱신 비용 발생       |
| 정점 데이터의 반복적인 CPU->GPU 전송 감소 | 빈번한 갱신 시 전송 및 동기화 비용 증가 가능 |
| 정적 데이터의 구조적 관리 가능           | 필요한 정점 속성 저장 시 메모리/대역폭 낭비  |
위의 장점에서 서술한 장점은 정점 데이터를 개별적으로 반복 전달하는 방식과 비교했을 때의 이점이며, 
Vertex Buffer 자체가 항상 성능을 개선시킨다는 의미는 아니다.


### 실제 사용
Unity에서는 일반적으로 Mesh의 Vertex Buffer를 내부적으로 관리하므로 개발자가 직접 생성할 필요는 없지만, 아래와 같은 API를 활용해 직접 제어할 수 있다.

| API                          | 역할                         |
| ---------------------------- | -------------------------- |
| Mesh.SetVertexBufferParams() | 정점 버퍼의 크기와 데이터 구조 설정       |
| Mesh.SetVertexBufferData()   | 정점 데이터 설정                  |
| Mesh.GetVertexBuffer()       | GPU의 Vertex Buffer에 접근     |
| Mesh.MarkDynamic()           | 자주 변경되는 Mesh에 대한 최적화 힌트 제공 |
