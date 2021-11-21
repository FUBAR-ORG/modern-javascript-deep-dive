# `strict mode`

## `strict mode`란?

```js
function foo() {
  x = 10;
}
foo();

console.log(x);
```

해당 소스에서 `JavaScript Engine`은 다음 과정으로 동작.

1. 선언하지 않은 `x` 변수에 값 10을 할당했으므로 스코프 체인을 통해 `x` 변수의 선언 위치를 찾음.
2. `foo` 함수의 스코프에서 `x` 변수 선언 검색.
3. 검색 실패 후 상위 스코프(전역)에서 `x` 변수 선언 검색.
4. 전역 스코프에도 존재하지 않기 때문에 `ReferenceError`를 발생시켜야하나,
5. `JavaScript Engine`에서 암묵적으로 전역 객체에 `x` 프로퍼티를 동적으로 생성.

> 이러한 현상을 **암묵적 전역**(`implicit global`)이라고 함.

- 개발자의 의도와 상관없이 암묵적 전역은 오류를 발생시키는 원인
  - 때문에 반드시 `var`, `let`, `const` 키워드를 사용하여 변수를 선언한 다음 사용.
- 하지만 오타나 문법 지식의 미비로 인한 실수는 언제나 발생.
- 잠재적인 오류를 발생시키기 어려운 개발 환경에서 개발하는 것이 안정적인 코드 생산에 도움.

> `ES5`부터 추가된 `strict mode`(엄격모드)는 `JavaScript`의 문법을 좀 더 엄격히 적용하여 문제를 일으킬 수 있는 코드에 명시적인 에러 발생 시킴.(`ES6`의 클래스와 모듈은 기본적으로 `strict mode` 적용)

> `ESLint` 린트 도구는 정적 분석(`static analysis`) 기능을 통해 소스코드를 실행하기 전에 소스를 스캔하여 문법적 오류 뿐만 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅 해 줄 뿐만 아니라, `strict mode`가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일의 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과 얻을 수 있음.

## `strict mode`의 적용

- 전역에 적용

```js
"use strict";

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

- 함수에 적용

```js
function foo() {
  "use strict";
  x = 10; // ReferenceError: x is not defined
}
foo();
```

> 코드의 선두에 `'use strict';`를 위치시키지 않으면 제대로 동작하지 않음.

---

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      "use strict";
    </script>
    <script>
      x = 1; // 에러가 발생하지 않음.
      console.log(x); // 1
    </script>
    <script>
      "use strict";

      y = 1; // ReferenceError: y is not defined
      console.log(y);
    </script>
  </body>
</html>
```

- 전역에 `strict mode`를 적용하는 것은 피하자.

  - 전역에 적용한 `strict mode`는 스크립트 태그 단위로 적용.
  - 때문에 `strict mode` 스크립트와 `non-strict mode` 스크립트를 혼용하는 것은 오류 발생 가능성 증가.
  - 특히 외부 서드파티 라이브러리가 `non-strcit mode`인 경우도 있을 수 있음.

    - 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프 구분 후 즉시 실행 함수의 선두에 `strict mode` 적용

    ```js
    // 즉시 실행 함수의 선두에 strict mode 적용
    (function () {
      "use strcit";

      // Do something...
    })();
    ```

---

```js
(function () {
  // non-strict moe
  var let = 10; // 에러가 발생하지 않음.
  function foo() {
    "use strict";

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
})();
```

- 함수 단위로 `strcit mode`를 적용하는 것도 피하자.

  - 어떤 함수는 `strict mode`를 적용하고 어떤 함수는 적용하지 않는 것은 바람직하지 않음.
  - 모든 함수에 일일히 `strict mode`를 적용하는 것은 번거로움.
  - `strict mode`가 적용된 함수가 참조할 외부의 컨텍스트에 `strict mode`를 적용하지 않으면 문제 발생.

> 따라서 즉시 실행 함수로 감싼 스크립트 단위로 `strict mode`를 적용하는 것이 바람직함.

---

## `strict mode`가 발생시키는 에러

1. 암묵적 전역

   - 선언하지 않은 변수를 참조하면 `ReferenceError`.

2. 변수, 함수, 매개변수의 삭제

   - `delete` 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`.

   ```js
   (function () {
       "use strict";

       var x = 1;
       delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.

       function foo(a) {
           delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
       }
       delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
   })()
   ```

3. 매개변수 이름의 중복

   - 중복된 매개변수 이름을 사용하면 `SyntaxError`.

   ```js
   (function () {
     "use strict";

     // SyntaxError: Duplicate parameter name not allowed in this context
     function foo(x, x) {
       return x + x;
     }
     console.log(foo(1, 2));
   })();
   ```

4. `with` 문의 사용

   - `with`문을 사용하면 `SyntaxError.`
     - `with`문은 전달된 객체를 스코프 체인에 추가.
     - `with`문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과.
     - 성능과 가독성이 나빠지는 문제가 있으므로 사용하지 않는 것이 좋음.

   ```js
   (function () {
     "use strict";

     // SyntaxError: Strict mode code may not include a with statement
     with ({ x: 1 }) {
       console.log(x);
     }
   })();
   ```

## `strict mode` 적용에 의한 변화

1. 일반 함수의 `this`

   - 일반 함수로서 호출하면 `this`에 `undefined`가 바인딩.
     - 생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문.
   - 에러는 발생하지 않음.

   ```js
   (function () {
     "use strict";

     function foo() {
       console.log(this); // undefined
     }
     foo();

     function Foo() {
       console.log(this); // Foo
     }
     new Foo();
   })();
   ```

2. `argumenets` 객체

   - 매개변수에 전달된 인수를 재할당 하여 변경해도 `arguments` 객체에 반영되지 않음.

   ```js
   (function (a) {
     "use strict";
     // 매개변수에 전달된 인수를 재할당하여 변경
     a = 2;

     // 변경된 인수가 arguments 객체에 반영되지 않음.
     console.log(arguments); // { 0: 1, length: 1 }
   })(1);
   ```
