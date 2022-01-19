# 44장 REST API

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정하는 아키텍처다.

- 2000년 로이 필딩의 논문에서 처음 소개 되었다.
- REST는 HTTP 프로토콜을 의도에 맞게 디자인 하도록 유도하며 HTTP의 장점을 최대한 활용할 수 있는 아키텍쳐다.
- REST의 기본원칙을 성실히 지킨 디자인을 _RESTful_ 하다고 표현한다.
- REST API는 REST를 기반으로 서비스 API를 구현한 것이다.

<br>

## REST API의 구성

REST API는 자원(resource), 행위(verb), 표현(representations)로 구성된다.

- 자원: 자원에 대한 내용이며 URI(엔드포인트)로 표현한다.
- 행위: 자원에 대한 행위이며 HTTP 요청 메서드로 표현한다.
- 표현: 행위의 구체적 내용이며 페이로드로 표현한다.

REST는 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조(self descriptiveness)로 구성되어 REST API만으로 HTTP 요청을 이해할 수 있다.

<br>

## REST API 설계 원칙

RESTful API를 설계하는 중심 원칙은 **URI는 리소스를 표현하는데 집중**하고** 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것**이다.

### URI는 리소스를 표현해야 한다.

URI는 리소스를 표현하는데 중점을 두어야 하며 리소스 이름은 명사를 사용한다.

```
# bad
GET /getTodos/1

# good
GET/todos/1
```

### 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다.

- 주로 GET, POST, PUT, PATCH, DELETE 메서드를 사용한다.
- HTTP 요청 메서드를 통해 표현하며 URI에 표현하지 않는다.

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
