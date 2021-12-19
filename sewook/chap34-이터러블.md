# 이터러블

## 이터레이션 프로토콜

이터레이션 프로토콜은 순회 가능한(iterable) 자료 구조를 만들기 위해 ECMAScript 사양에 정의한 규칙이다.

이터레이션 프로토콜에는 2가지 종류가 있다.

- 이터러블 프로토콜
  - Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나
  - 프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
- 이터레이터 프로토콜
  - 이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.

### 이터러블

이터러블은 이터러블 프로토콜을 준수한 객체이자 순회가능한 자료구조이다.

```js
// 배열, Map, Set, 문자열은 이터러블이다.

// [Symbol.iterator]가 function이면 이터러블 객체이다.
const isIterable = (v) =>
  v !== null && typeof v[Symbol.iterator] === 'function';

isIterable([]); // true
isIterable(''); // true
isIterable({}); // false
```

일반 객체는 Symbol.iterator를 구현하거나 상속받지 않으므로 이터러블이 아니다.

### 이터레이터

- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
- 이터레이터의 next 메서드는 이터러블 요소를 탐색하기 위한 포인터이다.

```js
const array = [1, 2];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
// 이터레이터는 next 메서드를 반환한다.
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터레이터 리절트 객체를 반환한다.
// 이터레이터 리절트 객체는 이터러블을 순회한 결과인 value, done 프로퍼티를 갖는다.
console.log(iterator.next()); // { value: 1, done: false}
console.log(iterator.next()); // { value: 2, done: false}
console.log(iterator.next()); // { value: undefined, done: true}
```

<br>

## 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 표준 빌트인 이터러블 객체를 제공한다.

- Array, String, TypedArray, arguments, DOM 컬렉션(NodeList, HTMLCollection)

> TypedArray란?
>
> - ArrayBuffer: 개발자가 지정한 메모리 크기만큼의 이진 데이터를 저장하는 객체
> - ArrayBufferView: ArrayBuffer의 이진 데이터에 접근할 수 있는 객체들의 통칭
>   - **TypedArray**
>     - Int8Array, Unit8Array, Unit8ClampledArray, Float32Array 등
>   - DataView

<br>

## for ... of 문

for of 문은 어떻게 동작하는가?

- for ... of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회한다.
- next 메서드가 반환한 이터레이터 리절트 객체의 value 값을 for ... of 문의 변수에 할당한다.
- done 프로퍼티가 true가 되면 순회를 중단한다.

```js
// for of 문을 for 문으로 작성하기

const iterable = [1, 2, 3];

const iterator = iterable[Symbol.iterator]();

for (;;) {
  const res = iterator.next(); // 이터레이터 리절트 객체
  if (res.done) break; // done 프로퍼티가 true면 순회 중단

  const item = res.value;
  console.log(item); // 1 2 3
}
```

<br>

## 이터러블과 유사 배열 객체

유사 배열 객체란 다음과 같은 특징을 가지는 객체이다.

- length 프로퍼티를 가지므로 순회 가능하다.
- 배열처럼 인덱스로 접근할 수 있다.
- Symbol.iterator 메서드가 없으므로 이터러블은 아니다.
- arguments, NodeList, HTMLCollection은 유사 배열 객체이자 이터러블이다.

```js
const obj = {
  0: 1,
  1: 2,
  2: 3,
  legnth: 3,
};

obj[1]; // 2

// length 프로퍼티를 가지기 때문에 순회 가능하다.
for(let i = 0 ; i < obj.length ; i++){ ... }

// Symbol.iterator 메서드가 없기 때문에 이터러블은 아니다.
for(const item of obj) { ... } // TypedError

// Array.from은 유사 배열 객체나 이터러블을 배열로 변환한다.
const arr = Array.from(obj); // [1,2,3]
```

<br>

## 이터레이션 프로토콜의 필요성

- ES6 이전의 순회 가능한 데이터 컬렉션은 통일된 규약 없이 다양한 방법으로 순회할 수 있었다.
- ES6에서 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for of 문, 스프레드 문법, 디스트럭처링 할당의 대상으로 사용할 수 있게 했다.
- 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정해 데이터 소비자가 효율적으로 데이터를 사용할 수 있게하는 인터페이스 역할이다.

<br>

## 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

```js
// 사용자 정의 함수로 피보나치 수열을 구현하기

const fibonacci = {
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    const max = 10; // 인수가 아닌 명시적으로 작성할 수도 있다.

    return {
      next() {
        pre = cur;
        cur += pre;
        return { value: cur, done: cur >= max };
      },
    };
  },
};

for (const num of fibonacci) {
  console.log(num);
}
```

### 이터러블을 생성하는 함수

```js
const fibonacci = function (max) {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return {
        next() {
          pre = cur;
          cur += pre;
          return { value: cur, done: cur >= max };
        },
      };
    },
  };
};

for (const num of fibonacci(10)) {
  console.log(num);
}
```

### 무한 이터러블과 지연 평가

```js
const fibonacci = function (max) {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      pre = cur;
      cur += pre;
      return { value: cur }; // done 프로퍼티 생략
    },
  };
};

for (const num of fibonacci()) {
  if (num > 500) break;
  console.log(num); // 2 4 8 16 32 64 128 256
}
```

지연 평가란 데이터를 생성하지 않다가 데이터가 필요한 시점이되면 그 때 데이터를 생성하는 기법이다.

- 위 예제는 for of 문 이전에 데이터를 생성하지 않는다.
- 이러한 지연 평가는 빠른 실행속도, 메모리 효율, 무한 표현 등의 장점이 있다.

<br>

---

### Summary

- 이터레이션 프로토콜은 iterable한 자료 구조를 만들기 위한 프로토콜이다.
- 이터러블 프로토콜은 직접 구현하거나 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환하는 규약이다.
- 이터레이터 프로토콜은 이터레이터의 next 메서드를 호출하면 이터러블을 순회하며 이터레이터 리절트 객체를 반환하는 규약이다.
- 이터러블은 이터러블 프로토콜을 준수하는 순회 가능한 객체이다.
- 이터레이터는 이터레이터 프로토콜을 준수하며 이터러블 요소를 탐색하기 위한 포인터이다.
- for of 문은 내부적으로 이터레이터의 next 메서드를 호출해 반환한 이터레이터 리절트 객체와 함께 이터러블을 순회한다.
- 이터레이션 프로토콜은 ES6에서 정의되었으며 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정해 데이터 소비자가 효율적으로 데이터를 사용할 수 있게하는 인터페이스 역할을 한다.
