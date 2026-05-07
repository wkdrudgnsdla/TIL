### HTTP
HyperText Transfer Protocol의 약자
- 하이퍼텍스트 문서를 교환하기 위해 만들어진 통신 규약
- TCP/IP기반
- 요청(Request)과 응답(Response) 구조로 되어있음
- 클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어짐

### Request Message
- start line
- header
- body

#### Start line
HTTP Request Message의 시작 라인
HTTP request의 start line 3가지 부분으로 구성
- [[HTTP메서드]]
- Request Target
- HTTP version
###### HTTP 메서드
HTTP메서드는 요청의 의도를 담고있는 Get Post Put Delete등이 있다.
Get은 존재하는 자원에 대한 요청, Post는 새로운 자원을 생성, Put은 존재하는 자원에 대한 변경, Delete는 존재하는 자원에 대한 삭제와 같은 기능을 가지고있음

###### Request target
Request Target은 HTTP Request가 전송되는 목표 주소

###### HTTP version
HTTP version은 version에 따라 Request 메세지 구조나 데이터가 다를 수 있어서 version을 명시합니다.

#### Headers
해당 Request에 대한 추가 정보를 담고있는 부분

#### Body
HTTP Reqeust가 전송하는 데이터를 담고있는 부분
전송하는 데이터가 없다면 body부분은 비어있음
Post요청일 경우 보통 HTML 폼 데이터가 포함되어있음

### Response Message
HTTP Response Message는 request와 동일하게 공백을 제외하고 3가지 부분으로 나누어진다
- status line
- Headers
- Body

#### Status Line
HTTP Response의 상태를 간략하게 나타내주는 부분
HTTP Response의 status line또한 3가지 부분으로 구성
- HTTP version
- Status Code
- Status Text

#### Headers
Request의 headers와 동일함
하지만 response에서만 사용되는 headers값이 있다
예를들어 User-Agent 대신 Server 헤더가 사용된다.

#### Body
Request의 Body와 일반적으로 동일하다.
Request와 마찬가지로 모든 response가 body가 있지는 않음
데이터를 전송할 필요가 없을 시 body가 비어있음
