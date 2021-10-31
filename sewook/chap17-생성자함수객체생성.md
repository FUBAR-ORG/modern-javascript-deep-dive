# 17장 생성자 함수에 의한 객체 생성

## 생성자 함수

이전 정리 포스트 참고: [new, Constructor](https://github.com/SewookHan/TIL/blob/main/JavaScript/33-js-concepts/new-constructor-instanceof-and-instance.md)

\+ 생성자 함수 내부에서 return 문 작성은 생성자 함수의 동작에 영향을 줄 수 있기 때문에 명시적 return 작성은 지양해야한다.

<br>

함수는 객체다. 그러나 함수는 `[[Construct]]` 내부 메서드와 추가적으로 `[[Call]]` 내부 메서드를 가지고 있다. 일반적인 함수 호출에 경우 `[[Call]]` 내부 메서드가 호출되고 `new` 연산자와 같이 생성자 함수로서 호출되면 `[[Construct]]` 메서드가 호출된다.

모든 함수 객체는 `[[Call]]` 내부 메서드를 포함하는 caller이다. 그러나 함수 선언문과 함수 표현식으로 정의된 **일반 함수**는 constructor이고 화살표 함수, 메서드(함수를 프로퍼티 값으로 사용)로 정의된 함수는 non-constructor이다.

`new` 연산자와 함께 호출되는 함수는 constructor 함수이어야한다.

빌트인 생성자 함수(`Object`, `Function`, `Array`)는 `new` 연산자를 생략하더라도 정상적으로 동작한다.
