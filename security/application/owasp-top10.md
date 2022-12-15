`OWASP(Open Web Application Security Project)`에서는 3~4년 주기로 10가지 보안이슈를 발표한다.

인젝션
---

SQL, OS, XXE, LDAP 인젝션은 `신뢰할 수 없는 데이터`가 명령어나 쿼리문의 일부분으로써, 인터프리터로 보내질 때 발생한다. 

- 공격자의 악의적인 데이터는 예기치 않은 명령을 실행
- 올바른 권한 없이 데이터에 접근하도록 인터프리터를 속임

### SQL 인젝션

DB와 연동되어 있는 애플리케이션 `입력값을 조작`하여 DBMS가 의도하지 않은 결과를 반환하도록 하는 기법

> 필터링 기능이 없는 애플리케이션의 경우 사용자의 입력값의 적정성의 검사를 하지 않는다.

#### 공격 피해

- DB 정보 조회/변조/삭제
- 인증절차 우회
- 시스템 명령어 실행
- 주요시스템 파일정보 노출

#### 취약점 판단

입력 폼에 `큰따옴표`, `작은따옴표`, `세미콜론` 등을 입력하여 DB 에러를 출력하는지 확인

#### 대응 방법

- 개발단계부터 모든 입력값에 대해 `적절한 검증절차`를 설계 구현
- SQL 서버의 `에러 메시지`를 표시 X
- 일반 사용자 권한으로 `시스템 저장 프로시저` 접근을 불허
- 동적 SQL 완성방식 사용을 지양하고 `저장 프로시저`를 사용
- `선처리 질의문`을 이용

인증 취약점
---

인증 및 세션 관리와 관련된 애플리케이션 기능의 잘못된 구현으로 발생한다.

- 공격자에게 `암호`, `키`, `세션 토큰`을 위험에 노출시킴
- 일시적/영구적으로 다른 사용자의 권한을 획득하여 결함을 악용하는 것을 허용

### 웹 방화벽(WAF, Web Application Firewall)

웹 애플리케이션을 대상으로 시도되는 해킹을 차단하는 솔루션

#### 기능

- 사용자 요청 검사
  - 접근제어, Web DoS, 업로드 파일 및 요청 형식 검사, SQL 인젝션 및 XSS 등의 차단
- 콘텐츠 보호
  - 정보 유출 차단, 웹 변조 방지, 코드 노출 진단
- 위장
  - URL 정보 위장, 서버 정보 위장

#### 기존 방화벽과 차이

| 구분 | 웹 방화벽 | 기존 방화벽 |
|------|-----------|-------------|
| 목적 | 유해 HTTP 차단 | 유해 포트 차단 |
| 동작레벨 | 7계층 | 3/4계층 |
| 동작방식 | 규칙 + 애플리케이션 로직 | 일련의 규칙에 의해 동작 |

민감 데이터 노출
---

다수의 웹 애플리케이션과 API는 금융 정보, 건강 정보, 개인 식별 정보와 같은 중요한 정보를 제대로 보호하지 않는다.

- 공격자는 신용카드 사기, 신분 도용 또는 다른 범죄를 수행하기 위해 보호가 취약한 데이터를 훔치거나 수정
- 중요한 데이터는 저장 또는 전송할 때 암호화 같은 추가 보호 조치가 없으면 탈취 가능
- 브라우저에서 주고 받을 때 각별한 주의가 필요

XML 외부 객체(XXE)
---

`XXE`는 악의적인 자바스크립트를 막기 위한 필터장치를 우회하는 취약점

- `XML 문서`에서 동적으로 외부 URI의 리소스를 포함시킬 수 있는 `외부 엔티티(Entity)`를 사용할 때 발생
- 오래된 XML 프로세서들은 XML 문서 내에서 외부 개체 참조를 평가
- 외부 개체는 파일 URI 처리기, 내부 파일 공유, 내부 포트 스캔, 원격 코드 실행과 DoS 공격을 사용하여 내부 파일을 공개하는데 사용 가능

접근제어 취약점
---

인증된 사용자가 수행할 수 있는 작업에 대한 제한이 제대로 적용되어 있지 않은 경우 발생하는 취약점

- 공격자는 다른 사용자의 계정 접근, 중요 데이터에 접근/수정, 접근 권한 수정 등을 할 수 있음

### 직접 객체 참조

파일, 디렉터리, DB 키와 같이 내부적으로 구현된 객체에 대한 참조가 노출될 때 발생

> 2013년도 이전에는 OWASP TOP10의 별도 항목이었다가 2017년도부터 접근통제 취약점으로 편입되었음.

#### 디렉터리 탐색 공격(파일 다운로드 취약점)

브라우저에서 확인 가능한 경로의 상위로 탐색하여 시스템 파일을 다운로드 하는 방법

- 자료실에 올라간 파일을 다운로드할 때 전용 다운로드 프로그램이 파일을 가져옴
- 이때 파일 이름을 필터링하지 않아서 발생하는 취약점
-  `..`과 `/` 문자를 필터링 하도록 해야함

#### 파일 업로드 제한 부재

클라이언트에서 서버 측으로 임의의 파일을 보낼 수 있는 취약점은 웹 서버가 가지는 가장 치명적인 취약점

- 공격자는 웹 서버에 악의적인 파일을 전송
- 원격지에서 해당 파일을 실행하여 웹 서버를 장악
- 추가적인 내부 침투 공격을 수행 가능
– `리버스 텔넷`과 같은 웹 서버의 통제권을 얻기 위해 반드시 성공해야 하는 공격

#### 리버스 텔넷

웹 해킹을 통해 시스템 권한을 얻은 후 텔넷과 같이 명령 가능한 쉘을 띄우기 위한 공격

- 방화벽이 존재하는 시스템을 공격시 주로 사용
- 일반적인 웹 서버는 방화벽 내부에 존재하며 `TCP/80`만을 허용
  - 텔넷이 열려있어도 방화벽으로 인해 공격자가 외부에서 접근 불가
- `아웃바운드 정책`을 설정하지 않은 경우 취약점을 노림
  - 공격자는 서버가 자신에게 텔넷 연결을 요청하도록 유도

> 아웃바운드 정책: 내부에서 방화벽 외부로 나가는 패킷에 대한 정책 (인바운드는 이와 반대)

보안 설정 오류
---

취약한 기본 설정 사용, 임시 설정, 개방된 클라우드 스토리지, 잘못 구성된 HTTP 헤더 메시지 등의 결과

- 운영체제, 프레임워크, 라이브러리와 애플리케이션을 안전하게 설정
- S/W는 시기 적절하게 패치/ 업그레이드를 진행

크로스사이트 스크립팅(XSS)
---

애플리케이션이 신뢰할 수 없는 데이터를 적절한 검증 없이 웹 브라우저로 전송하는 경우 발생

- 클라이언트의 브라우저에서 `공격자의 스크립트` 실행
- 사용자 세션을 탈취, 웹 사이트를 변조, 악성 사이트로 리다이렉션 등의 공격

### 공격 과정

1. 공격자는 게시판 등에다가 XSS를 심어 둠
2. 클라이언트는 공격자가 작성해 놓은 XSS에 접근
3. 게시판의 글을 통해 XSS코드가 클라이언트에게 전달됨
4. 클라이언트 브라우저에서 XSS 실행

### 공격 유형

#### Stored XSS

가장 일반적인 XSS 공격으로 게시판과 같은 곳에 공격자가 정상적인 평문이 아닌 `스크립트 코드`를 입력하는 기법

- 사용자가 게시판을 열람하는 순간 악성 스크립트가 실행됨
- 사용자의 쿠키정보가 유출되거나 악성 스크립트가 기획한 공격에 당하게 됨

#### Reflected XSS

악성 스크립트 코드가 `인자` 형태로 포함된 URL을 클릭 시 악성 스크립트 코드가 서버 사이트에 의해 HTML 문서로 반사되어 웹 브라우저에서 실행됨

- 공격 스크립트 부분을 주로 인코딩하여 전달해 사용자가 눈치채지 못하게 함
- URL의 변수 부분처럼 스크립트 코드를 입력하는 동시에 결과가 바로 전해지는 기법
- 서버 상에 악성 스크립트 코드를 남지 않는 장점(흔적X)

#### DOM based XSS

피해자의 브라우저가 HTML 페이지 구분 분석할 때 마다 공격 스크립트가 `DOM 생성`의 일부로 실행되며 공격

- 페이지 자체는 변경되지 않음
- 페이지에 포함되어 있는 브라우저측 코드가 DOM 환경에서 악성코드로 변경
- DOM 기반 XSS는 서버와 관계없이 브라우저에서 발생하는 것이다.

### 보안 대책

- `<`, `>`, `&`, `"`등 문자열은 `&lt`, `&gt`, `&amp`, `&quot`로 치환한다.
- 사용자 입력값에 대한 검증은 서버단에서 반드시 해야함
- 게시판에서 HTML 태그의 리스트를 선정, 해당 태그만 허용하는 방식 이용

### CSRF와 비교

사이트 간 요청 위조(CSRF)는 사용자의 의도와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 요청하게 하는 공격기법

| 비교 | XSS | CSRF |
|------|-----|------|
| 공격 대상 | 클라이언트 | 서버 |
| 공격 시점 | 악성 스크립트가 심어진 게시물 등을 열었을 때 | 이메일 등을 통해 전달된 주소를 열었을 때 |
| 공격 방법 | 클라이언트의 브라우저에서 실행 | 정상적인 사용자가 서버에 요청(서버에서 실행) |
| 이용 허점 | 클라이언트가 웹사이트를 신뢰하는 상태 | 특정 웹사이트가 사용자를 신뢰하는 상태 |

#### CSRF 보안대책

- 폼 전송방식으로 `GET` 대신 `POST` 사용
- `Referrer` 검증
  - 요청된 도메인이 어디인지 확인
  - 이메일 등을 통해서 악의적인 스크립트가 담긴 주소를 열면 차단
- CSRF 토큰
  - 임의의 난수를 `세션`에 저장하여 정상적인 사용자의 요청인지 확인

안전하지 않은 역직렬화
---

원격 코드 실행으로 이어질 수 있으며, 권한 상승 공격, 인젝션 공격과 재생 공격을 포함한 다양한 공격 수행에 사용

알려진 취약점이 있는 컴포넌트 사용
---

- 라이브러리, 프레임워크 및 다른 소프트웨어 모듈 같은 구성요소는 애플리케이션과 같은 권한으로 실행됨
- 만약에 취약한 구성요소가 악용된 경우 심각한 데이터 손실을 일으키거나 서버가 장악될 수 있음

불충분한 로깅과 모니터링
---

사고 대응의 비효율적인 통합 또는 누락이 공격자들로부터 시스템을 지속적으로 공격하거나 더 많은 시스템을 공격할 수 있게 하고, 데이터 변조/유출/삭제를 할 수 있게 한다