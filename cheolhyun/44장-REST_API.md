# `REST API`

- `REST`(`REpresentational State Transfer`)는 `HTTP`의 장점을 최대한 활용할 수 있는 아키텍처로 소개되어 `HTTP` 프로토콜을 의도에 맞게 디자인하도록 유도.
- `REST`의 기본 원칙을 성실히 지킨 서비스 디자인을 `RESTful`이라고 표현.

> `REST`는 `HTTP`를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처.  
> `REST API`는 `REST`를 기반으로 서비스 `API`를 구현한 것.

## `REST API`의 구성

- `REST` 자체 표현 구조(`self descriptiveness`)로 구성되어 `REST API` 만으로 `HTTP` 요청의 내용 이해 가능.

|        구성 요소        |              내용              |     표현 방법      |
| :---------------------: | :----------------------------: | :----------------: |
|    자원(`resource`)     |              자원              | `URI`(엔드 포인트) |
|      행위(`verb`)       |        자원에 대한 행위        | `HTTP` 요청 메서드 |
| 표현(`representations`) | 자원에 대한 행위의 구체적 내용 |      페이로드      |

## `REST API`의 설계 원칙

### 1. `URI`는 리소스를 표현해야 한다.

- 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용.

> 따라서 이름에 행위에 대한 표현 사용 금지.

```shell
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

### 2. 리소스에 대한 행위는 `HTTP` 요청 메서드로 표현한다.

- `HTTP` 요청 메서드는 클라이언트가 서버에 요청의 종류와 목적(리소스에 대한 행위)를 알리는 방법.
- 5가지 요청 메서드를 사용하여 `CRUD`를 구현.
- 리소스에 대한 행위는 `URI`에 표현하지 않고, `HTTP` 요청 메서드로 표현.

```shell
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```