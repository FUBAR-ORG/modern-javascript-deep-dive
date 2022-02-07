# 제너레이터와 `async`/`await`

## 제너레이터란?

- `ES6`에서 도입된 제너레이터(`generator`)는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수.
- 제너레이터와 일반 함수의 차이

  1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권 양도 가능.
     - 함수 호출자가 함수의 실행을 일시 중지시키거나 재개 가능.
     - 함수의 제어권을 함수가 독점하는 것이 아닌, 함수 호출자에게 양도(`yield`) 가능.
  2. 제너레이터 함수는 함수 호출자와 함수 상태를 주고받을 수 있음.
     - 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있음.
  3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환.
     - 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체 반환.

---

## 제너레이터 함수의 정의

- `function*` 키워드로 선언.
- 하나 이상의 `yield` 표현식을 포함.
- 두 가지를 제외하면 일반 함수를 정의하는 방법과 같음.

```js
function* genDecFunc() {
  yield 1;
}

const genExpFunc = function* () {
  yield 1;
};

const obj = {
  *genObjMethod() {
    yield 1;
  },
};

class MyClass {
  *genClsMethod() {
    yield 1;
  }
}

const genArrowFunc = * () => {
  yield 1;
} // SyntaxError: Unexpected token '*'

new genDecFunc(); // TypeError: genDecFunc is not a constructor
```

- 애스터리스크(`*`)의 위치는 `function` 키워드와 함수 이름 사이라면 어디든지 상관 없으나, 일관성을 유지하는 것이 좋음.
- 제너레이터 함수는 화살표 함수로 정의할 수 없음.
- 제너레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없음.

---

## 제너레이터 객체

> 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환.

> 제너레이터 함수가 반환한 제너레이터 객체는 이터러블(`iterable`)이면서 동시에 이터레이터(`iterator`).

- 제너레이터 객체는 `Symbol.iterator` 메서드를 상속받는 이터러블이면서 `value`, `done` 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 `next` 메서드를 소유하는 이터레이터.
- 제너레이터 객체는 `next` 메서드를 소유하므로 `Symbol.iterator` 메서드를 호출해서 별도로 이터레이터를 생성할 필요가 없음.
- 제너레이터는 이터레이터에는 없는 `return`, `throw` 메서드를 가짐.
  - `next` 메서드: 호출 시 제너레이터 함수의 `yield` 표현식까지 코드 블록을 실행하고 `yield`된 값을 `value` 프로퍼티 값으로, `false`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환.
  - `return` 메서드: 호출 시 인수로 전달받은 값을 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환.
  - `throw` 메서드: 호출 시 인수로 전달받은 에러를 발생시키고 `undefined`를 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환.

```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: "End!", done: true}
console.log(generator.throw("Error!")); // {value: undefined, done: true}
```

---

## 제너레이터의 일시 중지와 재개

- 제너레이터는 `yield` 키워드와 `next` 메서드를 통해 실행을 일시 중지 했다가 필요한 시점에 다시 재개 가능.
- `next` 메서드를 호출하면 `yield` 표현식까지만 함수의 코드 블록 실행.
- `yield` 키워드는 제너레이터 함수의 실행을 일시 중지 시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환.

```js
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2이 할당
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당
console.log(generator.next()); // {value: 2, done: false}

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당
console.log(generator.next()); // {value: 3, done: false}

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행
// value 프로퍼티에는 제너레이터 함수의 암묵적 반환값 undefined가 할당
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 ftrue가 할당
console.log(generator.next()); // {value: undefined, done: true}
```

- 제너레이터 객체의 `next` 메서드를 호출하면 `yield` 표현식까지 실행되고 일시 중지(`suspend`).
  - 이때 함수의 제어권이 호출자로 양도(`yield`).
- 이후 필요한 시점에 호출자가 또다시 `next` 메서드를 호출하면 일시 중지된 코드부터 실행을 재개(`resume`)하기 시작하여 다음 `yield` 표현식까지 실행되고 또 일시 중지.
- `next` 메서드는 `value`, `done` 프로퍼티를 갖는 이터레이터 리절트 객체를 반환.
  - `value` 프로퍼티에는 `yield` 표현식에서 `yield`된 값(`yield` 키워드 뒤의 값)이 할당.
  - `done` 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 `boolean` 값이 할당.
  - 제너레이터 함수가 끝까지 실행되면 이터레이터 리절트 객체의 `value` 프로퍼티에는 제너레이터 함수의 반환값이 할당.
- 제네레이터 객체의 `next` 메서드에서는 인수 전달 가능.
- `next` 메서드에 전달한 인수는 제너레이터 함수의 `yield` 표현식을 할당받는 변수에 할당.

> `yield` 표현식을 할당받는 변수에 `yield` 표현식의 평가 결과가 할당되지 않는 것에 주의.

```js
function* genFunc() {
  // yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당
  // x 변수에는 아직 아무 것도 할당되지 않고, next 메서드가 두 번째 호출될 때 결정
  const x = yield 1;
  // 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당
  // const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료

  // yield된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당
  const y = yield x + 10;
  // 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당
  // const y = yield x + 10;은 세 번째 next 메서드를 호출했을 때 완료

  // 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당
  // 일반적으로 제네레이터의 반환값은 의미가 없음
  // return은 종료의 의미로만 사용
  return x + y;
}

const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않음
// 처음 호출하는 next 메서드에 인수를 전달할 경우 무시
let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10);
console.log(res); // {value: 20, done: false}

res = generator.next(20); // {value: 30, done: true}
console.log(res);
```

- 이처럼 제너레이터 함수는 `next` 메서드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있음.
- 함수 호출자는 `next` 메서드를 통해 `yield` 표현식까지 함수를 실행시켜 제너레이터 객체가 관리하는 상태(`yield`된 값)을 꺼내올 수 있고, `next` 메서드에 인수를 전달해서 제너레이터 객체에 상태(`yield` 표현식을 할당받는 변수)를 밀어넣을 수 있음.

---

## 제너레이터의 활용

### 이터러블의 구현

- 제네레이터를 사용한 무한 피보나치 수열 예제

  ```js
  const infiniteFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
      [pre, cur] = [cur, pre + cur];
      yield cur;
    }
  })();

  for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);
  }
  ```

### 비동기 처리

- 프로미스를 사용한 비동기 처리를 후속 처리 메서드 없이 동기 처리처럼 구현하는 예제

  ```js
  // Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지
  const fetch = require("node-fetch");

  const async = (generatorFunc) => {
    const generator = generatorFunc();

    const onResolved = (arg) => {
      // 두 번째부터는 fetch의 결과인 Response 객체를 다음 yield 할당문에 밀어넣음
      const result = generator.next(arg);

      return result.done // 함수가 끝날 떄까지 반복 실행하기 위함
        ? result.value // 함수가 끝나면 제너레이터 함수의 반환 값(undefined)를 반환함
        : result.value.then((res) => onResolved(res)); // yield 는 모두 Promise이므로 후속 처리 함수가 실행될 때까지 기다린 후 다음 next 메서드 실행
    };
    return onResolved; // 클로저 반환
  };

  async(function* fetchTodo() {
    const url = "https://jsonplaceholder.typicode.com/todos/1";

    const response = yield fetch(url); // 첫 next 함수가 여기까지 실행 됨
    const todo = yield response.json();
    console.log(todo);
  })();
  // 첫 next 메서드를 즉시 호출함
  ```

---

## `async`/`await`

- `ES8`(`ECMAScirpt 2017`)에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 `async`/`await`가 도입.
- `async`/`await`는 프로미스를 기반으로 동작.
- `async`/`await`를 사용하면 프로미스의 `then`/`catch`/`finally` 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요가 없음.

> 프로미스의 후속 처리 메서드 없이 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능.

### `async` 함수

- `await` 키워드는 반드시 `async` 함수 내부에서 사용.
- `async` 함수는 `async` 키워드를 사용해 정의하며 언제나 프로미스를 반환.
- `async` 함수가 프로미스를 반환하지 않더라도 `async` 함수는 암묵적으로 반환값을 `resolve`하는 프로미스를 반환.

```js
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

const bar = async function (n) {
  return n;
};

const baz = async (n) => n;

const obj = {
  async foo(n) {
    return n;
  },
};

class MyClass {
  async bar(n) {
    return n;
  }
}
```

- 클래스의 `constructor` 메서드는 `async` 메서드가 될 수 없음.

### `await` 키워드

- `await` 키워드는 반드시 프로미스 앞에서 사용.
- `await` 키워드는 프로미스가 `settled` 상태가 될 때까지 대기하다가 `settled` 상태가 되면 프로미스가 `resolve`한 처리 결과를 반환.
- `await` 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 `settled` 상태가 되면 다시 재개.

- 일반 사용 예제

  ```js
  async function foo() {
    const a = await new Promise((resolve) =>
      setTimeout(() => resolve(1), 3000)
    );
    const b = await new Promise((resolve) =>
      setTimeout(() => resolve(2), 2000)
    );
    const c = await new Promise((resolve) =>
      setTimeout(() => resolve(3), 1000)
    );

    console.log([a, b, c]); // [1, 2, 3]
  }

  foo(); // 약 6초 소요.
  ```

- 서로 연관이 없이 개별적으로 수행되는 비동기 처리 예제

  ```js
  async function foo() {
    const res = await Promise.all([
      new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
      new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
      new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
    ]);

    console.log(res); // [1, 2, 3]
  }

  foo(); // 약 3초 소요.
  ```

- 처리 순서가 보장되어야 하는, 순차적으로 처리할 수 밖에 없는 예제

  ```js
  async function bar(n) {
    const a = await new Promise((resolve) =>
      setTimeout(() => resolve(n), 3000)
    );
    const b = await new Promise((resolve) =>
      setTimeout(() => resolve(a + 1), 2000)
    );
    const c = await new Promise((resolve) =>
      setTimeout(() => resolve(b + 1), 1000)
    );

    console.log([a, b, c]); // [1, 2, 3]
  }

  bar(1); // 약 6초 소요.
  ```

### 에러 처리

- 비동기 처리를 위한 콜백 패턴은 비동기 함수가 콜백 함수를 호출하지 않기 때문에 에러 처리가 어렵다는 단점.

  - 따라서 `try ... catch` 문을 사용해 에러를 캐치 불가.

- `async`/`await` 에서는 `try ... catch` 문을 사용 가능.
- 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확.

```js
const fetch = require("node-fetch");

const foo = async () => {
  try {
    const wrongUrl = "https://wrong.url";
    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (e) {
    console.error(e); // TypeError: Failed to fetch
  }
};

foo();
```

- `HTTP` 통신에서 발생한 네트워크 에러뿐 아니라 `try` 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치 가능.
- `async` 함수 내에서 `catch` 문을 사용해서 에러 처리를 하지 않으면 `async` 함수는 발생한 에러를 `reject`하는 프로미스를 반환.

> 따라서 `async` 함수를 호출하고 `Promise.prototype.catch` 후속 처리 메서드를 사용해 에러 캐치 가능.

```js
const fetch = require("node-fetch");

const foo = async () => {
  const wrongUrl = "https://wrong.url";
  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
```
