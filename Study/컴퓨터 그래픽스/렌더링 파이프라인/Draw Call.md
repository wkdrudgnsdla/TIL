드로우 콜(Draw Call)이란 CPU가 GPU에게 특정 그래픽 요소를 렌더링하도록 명령하는 호출을 의미한다.

Unity에선 화면에 오브젝트를 렌더링 하기 위해 CPU가 렌더링에 필요한 정보를 준비하고, 
그래픽 API를 통해 GPU에게 그리기 명령을 전달한다.
이러한 렌더링 과정에서 발생하는 명령이 Draw Call이다.

![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20340%20190%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2212%22%20y%3D%2222%22%20width%3D%2288%22%20height%3D%2266%22%20rx%3D%229%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.13%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.65%22%2F%3E%3Ctext%20x%3D%2256%22%20y%3D%2248%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20font-weight%3D%22bold%22%20fill%3D%22currentColor%22%3EUnity%20%2F%20CPU%3C%2Ftext%3E%3Ctext%20x%3D%2256%22%20y%3D%2267%22%20text-anchor%3D%22middle%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EB%A0%8C%EB%8D%94%EB%A7%81%20%EC%A4%80%EB%B9%84%3C%2Ftext%3E%3Crect%20x%3D%22126%22%20y%3D%2222%22%20width%3D%2288%22%20height%3D%2266%22%20rx%3D%229%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.13%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.65%22%2F%3E%3Ctext%20x%3D%22170%22%20y%3D%2248%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20font-weight%3D%22bold%22%20fill%3D%22currentColor%22%3EGraphics%20API%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%2267%22%20text-anchor%3D%22middle%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EA%B7%B8%EB%A6%AC%EA%B8%B0%20%EB%AA%85%EB%A0%B9%20%EC%A0%84%EB%8B%AC%3C%2Ftext%3E%3Crect%20x%3D%22240%22%20y%3D%2222%22%20width%3D%2288%22%20height%3D%2266%22%20rx%3D%229%22%20fill%3D%22%2364748b%22%20fill-opacity%3D%22.13%22%20stroke%3D%22%2364748b%22%20stroke-opacity%3D%22.65%22%2F%3E%3Ctext%20x%3D%22284%22%20y%3D%2248%22%20text-anchor%3D%22middle%22%20font-size%3D%2211%22%20font-weight%3D%22bold%22%20fill%3D%22currentColor%22%3EGPU%3C%2Ftext%3E%3Ctext%20x%3D%22284%22%20y%3D%2267%22%20text-anchor%3D%22middle%22%20font-size%3D%229%22%20fill%3D%22currentColor%22%3E%EC%8B%A4%EC%A0%9C%20%EB%A0%8C%EB%8D%94%EB%A7%81%3C%2Ftext%3E%3Cg%20stroke%3D%22%230D9488%22%20stroke-width%3D%222.2%22%20fill%3D%22none%22%20stroke-linecap%3D%22round%22%20stroke-linejoin%3D%22round%22%3E%3Cpath%20d%3D%22M102%2055%20H121%20M116%2050%20L121%2055%20L116%2060%22%2F%3E%3Cpath%20d%3D%22M216%2055%20H235%20M230%2050%20L235%2055%20L230%2060%22%2F%3E%3C%2Fg%3E%3Crect%20x%3D%2282%22%20y%3D%22113%22%20width%3D%22176%22%20height%3D%2235%22%20rx%3D%227%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.13%22%20stroke%3D%22%230D9488%22%2F%3E%3Ctext%20x%3D%22170%22%20y%3D%22135%22%20text-anchor%3D%22middle%22%20font-size%3D%2212%22%20fill%3D%22currentColor%22%20font-weight%3D%22bold%22%3EDraw%20Call%3C%2Ftext%3E%3Cpath%20d%3D%22M170%2089%20V113%22%20stroke%3D%22%230D9488%22%20stroke-dasharray%3D%223%203%22%20stroke-width%3D%221.5%22%2F%3E%3Ctext%20x%3D%22170%22%20y%3D%22174%22%20text-anchor%3D%22middle%22%20font-size%3D%2210%22%20fill%3D%22currentColor%22%3E%EB%A0%8C%EB%8D%94%EB%A7%81%20%EB%AA%85%EB%A0%B9%EC%9D%84%20%EC%A0%9C%EC%B6%9C%ED%95%98%EB%8A%94%20%EA%B3%BC%EC%A0%95%3C%2Ftext%3E%3C%2Fsvg%3E)

#### 세부적으로...
렌더링 과정은 아래와 같다.
1.CPU:렌더링할 오브젝트를 결정하고 Mesh, Material등 필요한 정보를 전달한다.
2.Graphics API:DirectX, Vulkan, Metal등을 통해 GPU에 명령을 호출한다.
3.GPU:전달받은 명령에 따라 정점처리, 레스터화, 픽셀처리등을 수행한다.
4.화면출력:렌더링된 결과가 최종적으로 화면에 표시된다.

여기서 중요한 사실은 **Draw Call이 발생할 때마다 CPU측에도 명령 준비 및 제출비용이 발생**한다는 점이다.
따라서, Draw Call이 과도하게 많으면 CPU가 렌더링 명령을 준비하고 제출하는 데에
꽤 많은 시간을 소비할 수 있다.

단, Draw Call이 많다는 이유만으로 반드시 성능이 저하되는것은 아니다.
GPU의 연산량, [[렌더링 파이프라인]], 드라이버와 플랫폼의 특성도 함께 고려해야한다.

