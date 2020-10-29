HTTP(Hypertext Transfer Protocol)
===

서버-클라이언트가 인터넷 상에서 데이터를 주고받기 위한 프로토콜

Contents
---

- [영속성](#영속성)
- [트랜잭션](#트랜잭션)
- [요청 메시지](#요청-메시지)
  - [요청-라인](#요청-라인)
  - [요청 헤더](#요청-헤더)
  - [바디](#바디)
- [응답 메시지](#응답-메시지)
  - [상태 코드](#상태-코드)
  - [응답 헤더](#응답-헤더)
  - [바디](#바디-1)

영속성
---

- 영속적 연결
  - 서버는 연결 이후 클라이언트의 요청을 대기
  - HTTP 1.1에서 Keep-Alive 옵션이 추가됨
    - 연결 상태 지속시간 설정 기능
- 비영속적 연결
  - 각 요구/응답에 대해 하나의 TCP 연결 생성

트랜잭션
---

![HTTP 트랜잭션](images/2020-06-04-15-20-24.png)

웹 브라우저가 웹 서버에 요청을 보내고 웹 서버가 이를 처리한 응답을 전송하는 한번의 과정

요청 메시지
---

클라이언트가 서버에게 요청을 보낼 때 전송하는 메시지

### 요청 라인

요청 메시지의 첫 줄로 공백 문자로 구분된 세가지 필드를 가진다.

- Method
- URL
- Version

| 메소드 | 역할 | 설명 |
|--------|------|------|
| GET | 리소스 취득 | 리소스 취득 인수를 URL에 포함시켜 전송, 최소한의 보안도 없음 |
| HEAD | 문서 정보 취득 | GET과 비슷하나, 실제 문서를 요청하는 것이 아니라, `문서 정보`를 요청 |
| POST | 데이터 전송 | 데이터를 HTTP 바디에 담아 웹서버로 전송, 최소한의 보안 |
| PUT | 데이터 전송(갱신 용도) | POST와 유사하나 `리소스 갱신`을 위해 사용 |
| DELETE | 파일 제거 | 웹 리소스를 제거 |
| OPTIONS | 제공 메소드 옵션 | 시스템에서 지원되는 `메소드 종류`를 확인 |
| TRACE | 요청 리소스 수신 경로 | 디버깅에 사용, 서버가 요청을 받고 있는지 여부 확인 |
| CONNECT | 프락시 기능 요청 | 원하는 웹서버와의 중계 연결 요청 |

> TRACE와 CONNECT는 현재는 거의 사용하지 않는다.

### 요청 헤더

바디 본문을 통해 전달하는 데이터(일반 문서) 이외의 정보를 교환하기 위해 사용

- `host`, `User-Agent`, `Referer`, `콘텐츠 길이`, `쿠키 정보` 등
- 헤더 이후에는 `빈 라인`: 헤더의 끝을 의미

### 바디

- PUT, POST일 때 송신될 주석, 웹사이트에 게시될 파일을 담고 있음
- GET 방식의 경우 요청 데이터가 없기 때문에 본체(Body)가 없음

응답 메시지
---

서버가 클라이언트에게 응답을 보낼 때 전송하는 메시지

### 상태 코드

세 자리 숫자로 요청의 상태를 정의

- 1xx (조건부 응답): 요청을 받았으며 프로세스를 계속한다
- 2xx (성공): 요청을 성공적으로 수신하여 처리하였다
- 3xx (리다이렉션): 요청 완료를 위해 추가 동작을 취해야 한다
- 4xx (클라이언트 오류): 요청의 문법이 잘못되었거나 요청을 처리할 수 없다
- 5xx (서버 오류): 서버가 명백히 유효한 요청에 대한 수행을 실패했다

<details><summary>자세히 보기</summary>

| 상태 코드 | 설명 |
|-----------|------|
| 100(continue) | 요청의 첫 부분이 서버 도착했으며 나머지를 기다리고 있는 상태. 요청자는 요청을 계속해야한다. |
| 101(Switching Protocols) | 요청자가 서버에 프로토콜 전환을 요청했으며 서버는 이를 승인하는 중이다. |
| 200(Ok) | 서버가 요청을 제대로 처리했다. |
| 201(Created) | 성공적으로 요청되었으며 서버가 새 리소스를 작성했다. |
| 202(Accepted) | 서버가 요청을 접수했지만 아직 처리하지 않았다. |
| 204(No Content) | 성공적으로 처리했지만 컨텐츠를 제공하지는 않는다. |
| 301(Moved Permanently) | 요청한 페이지를 새 위치로 영구적으로 이동했다.
| 302(Found) | 현재 서버가 다른 위치의 페이지로 요청에 응답하고 있지만 요청자는 향후 요청 시 원래 위치를 계속 사용해야 한다. |
| 304(Not Modified) | 마지막 요청 이후 요청한 페이지는 수정되지 않았다. 클라이언트 내 캐시 사용 |
| 400(Bad Request) | 요청 자체가 잘못되었을때 사용하는 코드이다. |
| 401(Unauthorized) | 인증이 필요한 리소스에 인증 없이 접근할 경우 발생한다. |
| 403(Forbidden) | 서버가 요청을 거부할 때 발생한다. |
| 404(Not Found) | 찾는 리소스가 없다는 뜻이다. |
| 405(Method Not Allowed) | 서버에서 허용되지 않은 메소드로 요청시 사용하는 코드이다. |
| 500(Internal Server Error) | 서버에 오류가 발생해 작업을 수행할 수 없을 때 사용된다. |
| 501(Not Implemented) | 서버가 요청을 수행하는데 필요한 기능을 지원하지 않는 경우 사용된다. |
| 502(Bad Gateway) |  게이트웨이가 연결된 서버로부터 잘못된 응답을 받았을 때 사용된다. |
| 504(Gateway Timeout) | 게이트웨이가 연결된 서버로부터 응답을 받을 수 없었을 때 사용된다. |

</details>

### 응답 헤더

추가적인 정보를 서버에서 클라이언트로 보냄

- `Contents-Type`, `Contents-Length` 등

### 바디

- 서버에서 클라이언트로 전송되는 문서를 포함
- 응답이 오류 메시지가 아니라면 본체가 존재