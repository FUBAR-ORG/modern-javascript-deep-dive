# REST

- 로이 필딩(Roy Fielding)의 2000년 논문에서 처음 소개
- 웹이 HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 REST를 소개했고 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다.
- REST의 기본 원칙을 성실히 지킨 서비스 디자인을 RESTful이라고 표현한다.

## REST API의 구성

REST API는 자원(resource), 행위(verb), 표현(representations)로 구성된다.
REST는 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조(self descriptiveness)로 구성되어 REST API만으로 HTTP 요청을 이해할 수 있다.

| 구성요소               | 내용                | 표현방법          |
|--------------------|-------------------|---------------|
| 자원(recouce)        | 자원                | URI(엔드포인트)    |
| 행위(verb)           | 자원에 대한 행위         | HTTP 요청 메서드   |
| 표현(representation) | 자원에 대한 행위의 구체적 내용 | Payload(페이로드) |


## REST API 설계 원칙

RESTful API를 설계하는 중심 원칙은 **URI는 리소스를 표현하는데 집중**하고** 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것**이다.


| HTTP 요청 메서드       | 종류             | 목적           | 페이로드 |
|-------------------|----------------|--------------|------|
| GET | index/retrieve | 모든/특정 리소스 취득 | x    |
| POST | create         | 리소스 생성       | o    |
| PUT | replace        | 리소스의 전체 교체   | o    |
| PATCH | modify         | 리소스의 일부 수정   | o    |
| DELETE | delete         | 모든/특정 리소스 삭제 | x    |

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