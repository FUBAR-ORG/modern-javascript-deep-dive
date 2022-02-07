# 46장. 제너레이터와 async/await

## 46.1 제너레이터란?

코드 블록의 실행을 실시 중지 했다가 필요한 시점에 재개할 수 있는 특수한 함수.

제너레이터와 일반 함수의 차이는 다음과 같다.

### 1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.

#### 일반 함수

- 일반 함수는 호출시 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다.
- 함수 호출자는 함수를 호출한 이후 함수를 실행을 제어할 수 없다.

#### 제너레이터 함수

- 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
- 함수 호출자가 함수 실행을 일시 중지 시키거나 재개 시킬 수 있다.
- 이는 **함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다.**

### 2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.

#### 일반 함수

- 일반 함수를 호출하면 매개 변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
- 즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.

#### 제너레이터 함수

- 제너레이터 함수는 함수 호출자와 양뱡향으로 함수의 상태를 주고받을 수 있다.
- 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.

### 3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

#### 일반 함수

- 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.

#### 제너레이터 함수

- 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수 정의

제너레이터 함수는 `function*` 키워드로 선언한다. 그리고 하나 이상의 `yield`를 포함한다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

- 제너레이터는 화살표 함수로 정의할 수 없다.

```js
//화살표 함수로 정의할 수 없음
const genArrowFunc = * () => {
  yield 1;
}; // SyntaxError: Unexpected token '*'

```

- 제너레이터는 new 연산자와 함께 생성자 함수로 호출할 수 없다.

```js
//new 연산자와 함께 생성자 함수로 호출 불가
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

## 46.3 제너레이터 객체

**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블 이면서 동시에 이터레이터이다.**

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```

제너레이터 객체는 **next 메서드**를 갖는 이터레이터이지만 이터레이터에 없는 return, throw 메서드를 갖는다. 제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

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

//next 메서드 호출시
console.log(generator.next()); // {value: 1, done: false}

//return 메서드 호출시
console.log(generator.return('End!')); // {value: "End!", done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

## 46.4 제너레이터의 일시 중지와 재개

- 제너레이터는 **yield 키워드와 next 메서드**를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
- 제너레이터 객체의 **next** 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행한다.
- 단, 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 **yield 표현식** 까지만 실행한다.
- **yield** 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 2, done: false}

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 3, done: false}

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
console.log(generator.next()); // {value: undefined, done: true}
```

1. **제너레이터 객체의 next 메서드를 호출하면 yield 표현식 까지 실행되고 일시중지 된다. 이때 함수의 제어권이 호출자로 양도된다.**

2. 이후 필요한 시점에 호출자가 또다시 next메서드를 호출하면 일시 중지된 코드부터 실행을 재개 하기 시작하여 다음 yield표현식 까지 실행되고 또 다시 일시 중지 된다.

3. 이때, 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.

4. next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값이 들어가고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당된다.

5. next를 반복 호출하여 yield 표현식까지 실행과 일시 중지를 반복하다가 제너레이터 함수가 끝까지 실행되면 value 프로퍼티에는 제너레이터 함수의 반환값이 할당되고 done 프로퍼티에는 함수가 끝까지 실행되었으니 true가 할당된다.

6. 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 해당된다.

## 46.5 제너레이터의 활용

### 46.5.1 이터러블의 구현

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

### 이터레이션 프로토콜을 준수하여 무한 피보나치 수열을 생성하는 함수

```js
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티를 생략한다.
      return { value: cur };
    },
  };
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

### 제너레이터를 사용하여 무한 피보나치 수열을 생성하는 함수 구현

```js
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...2584 4181 6765
}
```

### 46.5.2 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다. 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기처럼 구현할 수 있다.

```js
// node-fetch는 node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
// https://github.com/node-fetch/node-fetch
const fetch = require('node-fetch');

// 제너레이터 실행기
const async = generatorFunc => {
  const generator = generatorFunc(); // (2)  제너레이터 객체를 생성

  const onResolved = arg => {
    const result = generator.next(arg); // (5) next 메서드를 호출.

    return result.done
      ? result.value // (9) done 프로퍼티가 true일 경우 value 프로퍼티를 반환.
      : result.value.then(res => onResolved(res)); // (7) false일 경우 onResolved 함수를 재귀호출
  };

  return onResolved; // (3) onResolved함수를 반환. onResolved는 상위 스코프의 generator 변수를 기억하는 클로저임
};

async(function* fetchTodo() {
  // (1) async함수가 호출되면 인수로 전달받은 제너레이터 함수 fetchTodo를 호출함
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = yield fetch(url); // (6)  fethTodo의 첫번째 yield문
  const todo = yield response.json(); // (8) 두번째 yield문
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})(); // (4)
```

위 코드는 다음과 같이 동작한다.
