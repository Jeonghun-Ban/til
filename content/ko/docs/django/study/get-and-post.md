---
title: "GET/POST"
linkTitle: "GET/POST"
date: 2020-11-16
type: docs
draft: false
weight: 11
---


HTTP 통신 프로토콜을 통해서 정보를 주고받는 방식에는 여러가지가 있지만, 보통 GET과 POST 두가지를 많이 사용한다.

GET과 POST 차이
---

GET방식은 데이터를 전송할 때, 주소에 담아서 전송한다. 이전에 로또 프로젝트를 했을 때, 숫자를 전송하는 과정이 있었는데 이때 사용되었던 방식이다. 만약에 1부터 6까지 입력해서 전송하면 아래와 같이 데이터가 전송된다.

    127.0.0.1:8000/?number1=1&number2=2&number3=3&number4=4&number5=5&number6=6

이러한 방식으로 인해 어떤 데이터가 전송되는지 알수 있다는 점에서 정보 탈취에 취약하다.

반면 POST방식은 GET과 달리 주소를 통해서 어떤 정보가 전달되었는지 알 수 없다. 왜냐하면 헤더의 BODY 안에 담겨서 전송이 되기 때문이다.

POST 전송 타입 설정
---

POST는 데이터 전송에 대한 컨텐츠 타입 설정이 존재한다.

그 종류는 아래와 같이 3가지로 나누어진다.

    1. application/x-www-form-urlencoded
    2. text/plain
    3. multipart/form-data

일반적인 경우 1번 방식으로 전송되는데, 이때 전송되는 데이터는 GET과 같은 방식으로 key와 value 쌍으로 구성된다. 그리고 2번은 단순 txt로 전송하는 방법이다. 

3번은 파일 전송을 위해서 사용하는 것으로 바이너리 데이터를 사용한다는 것을 명시해주는 것이다.

보통 이러한 설정은 파일을 전송하기 위해서 해주는 경우가 많은데, 이럴 때 form태그 속성으로 `enctype="multipart/form-data"`를 써주면 된다.

2번은 거의 사용할 일이 없으며, 1번의 경우는 자동적으로 설정되므로 위 설정만 기억해두면 될 것이다.

적절한 HTTP 프로토콜 사용 예
---

- GET: 데이터를 조회할 때! ex) 게시글 조회

- POST: 데이터를 전송하는데, 정보가 밖으로 들어나서는 안되는 경우 <br>
ex) 회원가입, 로그인, 글 작성/수정