# 43장. Ajax

## 43.1 Ajax란?

Ajax(Asynchronos JavaScript and XML)란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

Ajax는 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작하며 XMLHttpRequest는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

## 43.2 JSON

JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포멧이다.

### 43.2.1 JSON의 표시 방식

JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트이다.

```json
{
  "name": "Lee",
  "age": 20
}
```

### 43.2.2 JSON.stringify

JSON.stringify 메서드는 객체를 JSON 포멧의 문제열로 반환한다. 클라이언트가 서버로 객쳋를 전송하려면 객체를 문자열화 해야하는데 이를 `직렬화`라고 한다.

### 43.2.3 JSON.parse

JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 `역직렬화`라 한다.

## 43.3 XMLHttpRequest

브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

### 43.3.1 XMLHttpRequest 객체 생성

XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다. XMLHttpRequest 객체는 브라우저가 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.

```js
const xhr = new XMLHttpRequest()
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드

### 43.3.3 HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

1. XMLHttpRequest.prototype.open 메서드로 HTTP요청을 초기화한다.

2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.

3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```js
const xhr = new XMLHttpRequest()
xhr.open('GET', '/users')
xhr.setRequestHeader('context-type', 'appliction/json')

xhr.send()
```

#### XMLHttpRequest.prototype.open

open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다.

> xhr.open(method, url[, async])

#### XMLHttpRequest.prototype.send

send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.

#### XMLHttpRequest.prototype.setRequestHeader

setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야 한다. 자주 사용하는 HTTP 요청 헤더인 Content-type 과 Accept에 대해 살펴보자

Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.

### 43.3.4 HTTP 응답처리

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다. XMLHttpRequest 객체는 onreadystatechange, onload, onerror 같은 이벤트 핸들러 프로퍼티를 갖는다. 이 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP응답을 처리할 수 있다.
