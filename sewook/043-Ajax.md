# 43장 Ajax

## Ajax란?

Ajax란 자바스크립트를 사용해 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.

- 브라우저 WebAPI인 XMLHttpRequest 객체를 기반으로 동작한다.
- XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

### 전통적인 웹페이지 모델

- 이전 웹페이지와 차이가 없어 변경할 필요 없는 부분까지 서버에서 전송 받아 불필요한 데이터 통신이 발생한다.
- 변경할 필요 없는 부분까지 처음부터 다시 렌더링해 깜빡이는 현상이 발생한다.
- 클라이언트와 서버가 동기 방식으로 동작해 서버로 부터 응답이 있을 때가지 다음 처리가 블로킹된다.

### Ajax 모델

- 변경할 부분을 갱신하는데 필요한 데이터만 전송받는다.
- 변경할 필요 없는 부분은 렌더링하지 않는다.
- 비동기 방식으로 동작하기 때문에 블로킹이 발생하지 않는다.

<br>

## JSON

JSON(javascript object notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.

- 키와 문자열은 반드시 큰따옴표로 묶어야 한다.
- 클라이언트가 서버로 객체를 전송하려면 JSON.stringify 메서드를 통해 JSON 포맷의 문자열화, 직렬화해야 한다.
- 서버로부터 전송된 JSON 데이터를 객체로 사용하려면 JSON.parse 메서드를 통해 객체로 변환해야 한다.

<br>

## XMLHttpRequest

자바스크립트를 사용해 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

### XMLHttpRequest 객체 생성

```js
const xhr = new XMLHttpRequest();
```

- XMLHttpRequest 객체는 브라우저에서 제공하는 WebAPI이므로 브라우저 환경에서만 정상적으로 작동한다.

### XMLHttpRequest 객체의 프로퍼티와 메서드

|            분류            | 이름               | 설명                                                                                                                                                                                |
| :------------------------: | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  **프로토타입 프로퍼티**   | readyState         | HTTP 요청의 현재 상태를 나타내는 정수. 다음과 같은 XMLHttpRequest의 정적 프로퍼티를 값으로 갖는다.<br />\- UNSENT : 0 \- OPENED : 1 \- HEADERS_RECEIVED: 2 \- LOADING :3 \- DONE: 4 |
|                            | status             | HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수 ex) 200                                                                                                                  |
|                            | statusText         | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 ex) "OK"                                                                                                                             |
|                            | responseType       | HTTP 응답 타입 ex) document, json, text, blob, arraybuffer                                                                                                                          |
|                            | response           | HTTP 요청에 대한 응답 몸체. responseType에 따라 타입이 다르다.                                                                                                                      |
| **이벤트 핸들러 프로퍼티** | onreadystatechange | readyState 프로퍼티 값이 변경된 경우                                                                                                                                                |
|                            | onerror            | HTTP 요청에 에러가 발생한 경우                                                                                                                                                      |
|                            | onload             | HTTP 요청이 성공적으로 완료한 경우                                                                                                                                                  |
|         **메서드**         | open               | HTTP 요청 초기화                                                                                                                                                                    |
|                            | send               | HTTP 요청 전송                                                                                                                                                                      |
|                            | abort              | 이미 전송된 HTTP 요청 중단                                                                                                                                                          |
|                            | setRequestHeader   | 특정 HTTP 요청 헤더의 값을 설정                                                                                                                                                     |

### HTTP 요청 전송

```js
// 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

XMLHttpRequest.prototype.send는 HTTP 요청을 서버에 전송하며 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.

- GET: 데이터를 URL의 일부분인 쿼리 문자열로 전송한다.
- POST: 데이터(payload)를 요청 몸체에 담아 전송한다.
- 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달한 인수는 무시된다.

### HTTP 응답 처리

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.

```js
//  readyState 프로퍼티 값이 변경된 경우 발생하는 reaadystatechange 이벤트를 캐치해 다음과 같이 HTTP 응답을 처리하는 예제

const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', ' ... ');

// HTTP 요청 전송
xhr.send();

xhr.onreadystatechange = () => {
  // readyState 프로퍼티 값이 4가 아니면 서버 응답이 완료되지 않은 상태다.

  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.log('Error', xhr.status, xhr.statusText);
  }
};

// load 이벤트로도 캐치할 수 있다.
// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.load = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.log('Error', xhr.status, xhr.statusText);
  }
};
```

---

### Summary

- Ajax란 자바스크립트를 사용해 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.
- Ajax는 브라우저 WebAPI인 XMLHttpRequest 객체를 기반으로 동작하며 XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
- JSON은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.

---

### Deep dive

- [Ajax와 Axios 그리고 fetch](https://velog.io/@kysung95/%EA%B0%9C%EB%B0%9C%EC%83%81%EC%8B%9D-Ajax%EC%99%80-Axios-%EA%B7%B8%EB%A6%AC%EA%B3%A0-fetch)
