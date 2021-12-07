# 7번째 데이터 타입 Symbol

심벌은 ES6에서 도입된 변경 불가능한 원시 타입의 값이다. 심벌 값은 중복되지 않는 유일한 값이며 이름 충돌 위험이 없는 프로퍼티 키를 만들기 위해 사용한다.

## 심벌 값의 생성

- Symbol()

  ```js
  // #1. 심벌 값에 대한 설명이 같더라도 유일한 심벌 값이 생성된다.
  const s1 = Symbol('s');
  const s2 = Symbol('s');
  console.log(typeof s1, s1); // symbol, Symbol(s)
  console.log(s1 === s2); // false

  // #2. 심벌 값은 원시 값이므로 new 연산자와 함께 호출하면 안된다.
  new Symbol(); // TypeError

  // #3. 심벌 값도 객체처럼 접근하면 래퍼 객체를 생성한다.
  console.log(s1.toString()); // Symbol(s)
  ```

- Symbol.for / Symbol.keyFor 메서드

  ```js
  // #1. Symbol.for
  // 인수로 전달받은 문자열을 키로 사용해 일치하는 심벌 값을 검색한다.
  // 검색에 실패하면 전역 심벌 레지스트리에 새로운 심벌 값을 생성한다.

  const s1 = Symbol.for('s'); // 새로운 심벌 값 생성
  const s2 = Symbol.for('s'); // s라는 키로 저장된 심벌 값 반환
  console.log(s1 === s2); // true

  // #2. Symbol.keyFor
  // 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출한다.
  // Symbol() 함수로 생성된 심벌 값은 전역 심벌 레지스트리에 존재하지 않기 때문에 undefined가 반환된다.

  Symbol.keyFor(s1); // 's'
  ```

## 심벌과 상수

프로퍼티를 상수가 아닌 중복될 가능성이 없는 심벌 값을 사용해 4 방향을 정의하는 예제이다.

```js
const Direction = {
  UP: Symbol('UP'), // UP: 1
  DOWN: Symbol('DOWN'), // UP: 2
  LEFT: Symbol('LEFT'), // LEFT: 3
  RIGHT: Symbol('RIGHT'), // RIGHT: 4
};

const myDirection = Direction.UP;
console.log(myDirection === Direction.UP); // true
```

## 심벌과 프로퍼티

심벌 값으로 프로퍼티 키를 동적으로 생성할 수 있다. 심벌 값으로 프로퍼티를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

```js
const obj = {
  [Symbol.for('symbolProperty')]: 1,
  stringProperty: 2,
};
obj[Symbol.for('symbolProperty')]; // 1
obj.stringProperty; // 2
```

심벌 값을 프로퍼티 키로 사용해 프로퍼티를 생성하면 프로퍼티를 은닉할 수 있다.

```js
console.log(Object.keys(obj)); // ['stringProperty']
console.log(Object.values(obj)); // [2]

// Object.getOwnPropertySymbols 메서드를 사용하면 찾을 수 있다.
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(symbolProperty)]
```

## 심벌과 표준 빌트인 객체 확장

표준 빌트인 객체에 사용자 정의 메서드를 추가해 확장하는 것은 바람직하지 않다. 그러나 심벌 값으로 프로퍼티 키를 생성해 표준 빌트인 객체를 확장하면 안전하게 표준 빌트인 객체를 확장할 수 있다.

```js
// X
Array.prototype.average = function () {
  return this.reduce((acc, cur) => acc + cur, 0) / this.length;
};
[1, 2, 3].average(); // 2

// O
Array.prototype[Symbol.for('average')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0) / this.length;
};
[1, 2, 3][Symbol.for('average')](); // 2
```

## Well-known Symbol

자바스크립트 빌트인 심벌 값은 ECMAScript 사양에서 Well-known Symbol이라고 부른다. Well-known Symbol은 Symbol.iterator를 키로 갖는 메서드를 가진다.

```js
// 일반 객체를 이터러블 처럼 동작하도록 구현

const iterable = {
  // Symbol.iterator를 구현해 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const value of iterable) {
  console.log(value);
}

const obj = {
  a: 1,
  b: 2,
  c: 3,
};
// 1 2 3 4 5
```

<br>

---

### Summary

- 심벌은 원시 타입의 값이며 중복되지 않는 유일한 값을 가진다.
- Symbol.for 메서드로 심벌 값을 생성하면 전역 심벌 레지스트리에 존재하는 심벌 값 혹은 새로운 심벌 값을 생성한다.
- 심벌 값으로 프로퍼티를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.
- 심벌 값을 가지는 프로퍼티 키를 사용해 표준 빌트인 객체를 확장하면 안전하게 확장할 수 있다.
- 일반 객체에 Symbol.iterator를 명시적으로 작성해 객체를 이터러블하게 만들 수 있다.
