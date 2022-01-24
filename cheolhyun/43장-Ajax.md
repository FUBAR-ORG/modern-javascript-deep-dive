# `Ajax`

## `Ajax`란?

- `Ajax`(`Asynchronous JavaScript and XML`)란 `JavaScript`를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식.
- 브라우저에서 제공하는 `Web API`인 `XMLHttpRequest` 객체를 기반으로 동작.
- `XMLHttpRequest`는 `HTTP` 비동기 통신을 위한 메서드와 프로퍼티 제공.
- 이전의 웹페이지는 `<html></html>` 태그로 시작과 끝을 표현하는 완전한 `HTML`을 서버로부터 전송받아 웹페이지를 처음부터 다시 렌더링하는 방식.

  > 따라서 화면이 전환되면 서버로부터 새로운 `HTML`을 전송받아 웹페이지 전체를 다시 렌더링.

  - 이 방식의 단점
    1. 이전과 차이가 없어서 변경할 필요가 없는 부분까지 포함된 `HTML`을 통신하므로 불필요한 데이터 통신 발생
    2. 변경할 필요가 없는 부분까지 새로 렌더링하여, 화면이 전환되면 순간적으로 깜빡이는 현상 발생
    3. 클라이언트와 서버의 통신이 동기 방식이므로, 응답이 있을 때까지 다음 처리는 블로킹

> `Ajax`는 웹페이지 변경에 필요한 데이터만 비동기 방식으로 전송받아 새로운 렌더링을 최소화 하여 빠른 퍼포먼스와 부드러운 화면 전환 가능

- `Ajax`의 장점

  1. 필요한 데이터만 통신하여 불필요한 데이터 통신이 발생하지 않음
  2. 변경할 필요가 없는 부분을 다시 렌더링 하지 않아, 깜빡이는 현상 제거
  3. 비동기 방식으로 통신하기 때문에, 요청을 보낸 이후 블로킹이 발생하지 않음

## `JSON`

- `JSON`(`JavaScript Object Notation`)은 클라이언트와 서버 간의 `HTTP` 통신을 위한 텍스트 데이터 포맷

### `JSON` 표기 방식

- `JavaScript`의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트.
- `JSON`의 키는 반드시 큰 따옴표(작은 따옴표 사용 불가)로 묶어야 함.
- 값은 객체 리터럴과 같은 표기법 사용 가능하지만, 문자열은 반드시 큰 따옴표.

### `JSON.stringify`

- 객체를 `JSON` 포맷의 문자열로 변환.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데, 이를 직렬화(`serializing`)이라고 함.
- 객체 뿐만 아니라 배열에도 사용 가능.

```js
const obj = {
  name: "Lee",
  age: 20,
  alive: true,
  hobby: ["traveling", "tennis"],
};

const json = JSON.stringify(obj);
const prettyJson = JSON.stringify(obj, null, 2); // 문자열로 변환하며 들여쓰기

function filter(key, value) {
  return typeof value === "number" ? undefined : value;
} // 숫자 타입은 반환하지 않도록 하는 필터 함수

const strFilterObject = JSON.stringify(obj, filter, 2);
// {
//     "name": "Lee",
//     "alive": true,
//     "hobby": ["traveling", "tennis"],
// }
```

### `JSON.parse`

- `JSON` 포맷의 문자열을 객체로 변환.
- 서버로부터 클라이언트에게 전송된 `JSON` 데이터는 문자열인데, 이를 객체화하는 것을 역직렬화(`deserializing`)이라고 함.
- 배열이 `JSON` 포맷의 문자열로 변환되어 있는 경우 문자열을 배열 객체로 변환.
  - 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환.

## `XMLHttpRequest`

- 브라우저는 `HTML`의 `form`, `a` 태그를 통해 `HTTP` 요청 전송 기능을 기본으로 제공
- `JavaScript`를 사용하여 `HTTP` 요청을 전송하려면 `XMLHttpRequest` 객체를 사용.

### `XMLHttpRequest` 객체 생성

- `XMLHttpRequest` 생성자 함수를 호출하여 생성.
- `Web API`이므로 브라우저 환경에서만 정상적으로 실행.

```js
const xhr = new XMLHttpRequest();
```

### `XMLHttpRequest` 객체의 프로퍼티와 메서드

> 중요한 것만 나열.

#### 프로토타입 프로퍼티

- `readyState`: `HTTP` 요청의 현재 상태를 나타내는 정수
- `status`: `HTTP` 요청에 대한 응답 상태(`HTTP` 상태 코드)를 나타내는 정수
- `responseType`: `HTTP` 응답 타입(`document`, `json`, `text`, `blob`, `arraybuffer`)
- `response`: `HTTP` 요청에 대한 응답 몸체(`response body`). `responseType`에 따라 타입이 다름.

#### 이벤트 핸들러 프로퍼티

- `onreadystatechange`: `readyState` 프로퍼티 값이 변경된 경우
- `onerror`: `HTTP` 요청에 에러가 발생한 경우
- `onload`: `HTTP` 요청이 성공적으로 완료한 경우

#### 메서드

- `open`: `HTTP` 요청 초기화
- `send`: `HTTP` 요청 전송
- `setRequestHeader`: 특정 `HTTP` 요청의 헤더의 값을 설정

#### 정적 프로퍼티

- `DONE`: `4`, 서버 응답 완료

### `HTTP` 요청 전송

- `HTTP` 요청 순서

  1. `XMLHttpRequest.prototype.open` 메서드로 `HTTP` 요청을 초기화
  2. 필요에 따라 `XMLHttpRequest.prototype.setRequestHeader` 메서드로 특정 `HTTP` 요청의 헤더 값을 설정
  3. `XMLHttpRequest.prototype.send` 메서드로 `HTTP` 요청 전송

```js
const xhr = new XMLHttpRequest();

xhr.open("GET", "/users");
xhr.setRequestHeader("content-type", "application/json");
xhr.send();
```

#### `XMLHttpRequest.prototype.open`

```js
xhr.open(method, url[, async]);
```

- `method`: `HTTP` 요청 메서드(`"GET"`, `"POST"`, `"PUT"`, `"DELETE"` 등)
- `url`: `HTTP` 요청을 전송할 `URL`
- `async`: 비동기 요청 여부. 옵션으로 기본값은 `true`

| `HTTP` 요청 메서드 |       종류       |         목적          | 페이로드 |
| :----------------: | :--------------: | :-------------------: | :------: |
|       `GET`        | `index/retrieve` | 모든/특정 리소스 취득 |    X     |
|       `POST`       |     `create`     |      리소스 생성      |    O     |
|       `PUT`        |    `replace`     |  리소스의 전체 교체   |    O     |
|      `PATCH`       |     `modify`     |  리소스의 일부 수정   |    O     |
|      `DELETE`      |     `delete`     | 모든/특정 리소스 삭제 |    X     |

#### `XMLHttpRequest.prototype.send`

```js
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- 매개변수: 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달.

  - 객체인 경우 반드시 `JSON.stringify`를 통해 직렬화.
  - `GET` 요청 메서드인 경우 전달한 인수는 무시되고 요청 몸체는 `null`로 설정.

- `GET` 요청 메서드의 경우 데이터를 `URL`의 일부인 쿼리 문자열(`query string`)로 서버에 전송
- `POST` 요청 메서드의 경우 데이터(페이로드, `payload`)를 요청 몸체(`request body`)에 담아 전송

#### `XMLHttpRequest.prototype.setRequestHeader`

- 반드시 `open` 메서드를 호출한 이후에 호출해야 함.
- `Content-type`은 요청 몸체에 담아 전송할 데이터의 `MIME` 타입의 정보를 표현

  ```js
  const xhr = new XMLHttpRequest();

  xhr.open("POST", "/users");

  xhr.setRequestHeader("content-type", "application/json");
  ```

- `Accept`은 서버가 응답할 데이터의 `MIME` 타입을 지정

  - 설정하지 않으면 `*/*`로 설정

  ```js
  const xhr = new XMLHttpRequest();
  xhr.setRequestHeader("accept", "application/json");
  ```

- `MIME` 타입

  - `text`: `text/plain`, `text/html`, `text/css`, `text/javascript`
  - `application`: `application/json`, `application/x-www-form-urlencode`
  - `multipart`: `multipart/formed-data`

### `HTTP` 응답 처리

- 서버가 전송한 응답을 처리하려면 `XMLHttpRequest` 객체가 발생시키는 이벤트를 캐치.
- `onreadystatechange` 이벤트를 캐치하여 `HTTP` 응답 처리.
- `send` 메서드를 통해 `HTTP` 요청을 서버에 전송하면 서버는 응답을 반환.

  - 하지만 언제 응답이 올지 모르므로 `HTTP` 요청의 현재 상태를 확인해야 함.
  - `onreadystatechange` 이벤트는 `HTTP` 요청의 현재 상태를 나타내는 `readyState` 프로퍼티가 변경될 때마다 발생.
  - `onreadystatechange` 이벤트 핸들러는 `xhr.readyState`가 `XMLHttpRequest.DONE`인지 확인하여 응답 여부 판단.
  - 판단 후 `xhr.status`로 응답 코드를 확인하여 정상 처리와 에러 처리 구분.
  - 정상 도착 시 `xhr.response`에서 서버가 전송한 데이터를 취득.

- `onload` 이벤트는 `HTTP` 요청이 성공적으로 완료된 경우 발생하므로 `xhr.readyState`를 확인할 필요가 없음.

```js
const xhr = new XMLHttpRequest();

xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

xhr.send();

xhr.onreadystatechange = () => {
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};
```
