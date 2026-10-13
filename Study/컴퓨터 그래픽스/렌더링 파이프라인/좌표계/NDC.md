### NDC(Normalized Device Coordinates)란?
NDC는 [[원근 나눗셈]]을 통해 [[Clip Space]] 좌표를 정규화한 좌표계이다.

NDC는 화면 해상도와 독립적인 정규화 좌표를 사용하며, 이후 Viewport Transform을 통해 실제 화면영역에 대응되는 좌표로 변환된다.

###### NCD의 좌표범위

| 축   | 일반적인 범위                      |
| --- | --------------------------- |
| X   | -1 ~                         |
| Y   | -1                           |
| Z 그래픽 API에 따라 0 ~ 1 or -1 ~ 1  or  |
이때, 일반적으로 X, Y좌표는 화면 중심을 (0, 0)으로 표현한다.
![720](data:image/svg+xml;charset=utf-8,%3Csvg%20font-family%3D%22-apple-system-body%2C%20ui-sans-serif%2C%20-apple-system%2C%20system-ui%2C%20%26quot%3BSegoe%20UI%26quot%3B%2C%20Helvetica%2C%20%26quot%3BApple%20Color%20Emoji%26quot%3B%2C%20Arial%2C%20sans-serif%2C%20%26quot%3BSegoe%20UI%20Emoji%26quot%3B%2C%20%26quot%3BSegoe%20UI%20Symbol%26quot%3B%22%20font-weight%3D%22400%22%20data-d-component%3D%22svg%22%20fill%3D%22currentColor%22%20style%3D%22color%3Argb\(237%2C%20237%2C%20237\)%22%20viewBox%3D%220%200%20320%20190%22%20width%3D%22100%25%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Crect%20x%3D%2262%22%20y%3D%2225%22%20width%3D%22196%22%20height%3D%22136%22%20fill%3D%22%230D9488%22%20fill-opacity%3D%22.07%22%20stroke%3D%22%2364748b%22%20stroke-width%3D%221.3%22%2F%3E%3Cpath%20d%3D%22M160%2013%20V174%20M46%2093%20H274%22%20stroke%3D%22currentColor%22%20stroke-opacity%3D%22.45%22%20stroke-width%3D%221.3%22%2F%3E%3Cpath%20d%3D%22M156%2020%20L160%2013%20L164%2020%20M268%2089%20L275%2093%20L268%2097%22%20fill%3D%22none%22%20stroke%3D%22currentColor%22%20stroke-opacity%3D%22.65%22%2F%3E%3Ccircle%20cx%3D%22160%22%20cy%3D%2293%22%20r%3D%224%22%20fill%3D%22%230D9488%22%2F%3E%3Cg%20fill%3D%22currentColor%22%20font-size%3D%2211%22%3E%3Ctext%20x%3D%2257%22%20y%3D%2221%22%20text-anchor%3D%22middle%22%3E\(-1%2C%201\)%3C%2Ftext%3E%3Ctext%20x%3D%22262%22%20y%3D%2221%22%20text-anchor%3D%22middle%22%3E\(1%2C%201\)%3C%2Ftext%3E%3Ctext%20x%3D%2257%22%20y%3D%22176%22%20text-anchor%3D%22middle%22%3E\(-1%2C%20-1\)%3C%2Ftext%3E%3Ctext%20x%3D%22264%22%20y%3D%22176%22%20text-anchor%3D%22middle%22%3E\(1%2C%20-1\)%3C%2Ftext%3E%3Ctext%20x%3D%22168%22%20y%3D%2286%22%3E\(0%2C%200\)%3C%2Ftext%3E%3Ctext%20x%3D%22278%22%20y%3D%2288%22%3EX%3C%2Ftext%3E%3Ctext%20x%3D%22170%22%20y%3D%2214%22%3EY%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)




### 내부 동작 원리
1. [[Clipping]]을 거친 정점의 [[동차 좌표]]를 전달받는다.
2. X,Y,Z를 W로 나누어 NDC를 계산한다.
3. 투영된 가시영역의 좌표가 정해진 NDC 범위에 대응하도록 표현된다.
4. NDC의 X,Y좌표를 실제 화면영역의 좌표로 변환한다.