# 7번째 데이터 타입 `Symbol`

## 심벌이란?

- `ES6`에서 도입된 `string`, `number`, `boolean`, `undefined`, `null`, `object`에 이은 7번째 데이터 타입.
- 변경 불가능한 원시 타입의 값.
- 다른 값과 중복되지 않는 유일무이한 값.
- 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

## 심벌 값의 생성

### `Symbol` 함수

- 다른 원시타입과 다르게 심벌 값은 `Symbol` 함수를 호출하여 생성.
- 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값.

```js
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

console.log(mySymbol); // Symbol()
```

- 생성자 함수로 객체를 생성하는 것과 달리 `new` 연산자를 호출하지 않음.

```js
new Symbol(); // TypeError: Symbol is not a constructor
```

- `Symbol` 함수에는 선택적으로 문자열 인수 전달 가능하지만, 이는 심벌 값의 대한 설명으로 디버깅 용도로만 사용되며, 실제 값에 어떠한 영향을 주지 않음.

```js
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값도 `number`, `string`, `boolean`과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체 생성.

```js
const mySymbol = Symbol("mySymbol");

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 `string`이나 `number` 타입으로 변환되지 않음.
- 단, `boolean` 타입으로는 암묵적으로 타입 변환 가능.

### `Symbol.for` / `Symbol.keyFor` 메서드

- `Symbol.for` 메서드

  - 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리(`global symbol registry`)에서 해당 키와 일치하는 심벌 값을 검색.

    - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환.
    - 검색에 실패하면 새로운 심벌 값을 생성하여 `Symbol.for` 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환.

    ```js
    const s1 = Symbol.for("mySymbol");
    const s2 = Symbol.for("mySymbol");

    console.log(s1 === s2); // true
    ```

  - `Symbol` 함수는 호출될 때마다 유일무이한 심벌 값을 생성.
  - `JavaScript Engine`이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 검색할 수 있는 키를 지정할 수 없으므로 관리되지 않음.
  - `Symbol.for` 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있음.

- `Symbol.keyFor` 메서드

  - 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출.

    ```js
    const s1 = Symbol.for("mySymbol");
    Symbol.keyFor(s1); // mySymbol

    const s2 = Symbol("foo");
    Symbol.keyFor(s2); // undefined
    ```

## 심벌과 상수

```js
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going up.");
}
```

- 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우, 상수 값 `1`, `2`, `3`, `4`가 변경될 수 있으며, 다른 변수 값과 중복될 수 있다는 단점 존재.
- 이때 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있음.

```js
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going up.");
}
```

- `enum`

  - `enum`은 명명된 숫자 상수(`named numeric constant`)의 집합으로 열거형(`enumerated type`)이라고 부름.
  - `JavaScript`는 `enum`을 지원하지 않지만 흉내 내어 사용할 수 있음

  ```js
  const Direction = Object.freeze({
    UP: Symbol("up"),
    DOWN: Symbol("down"),
    LEFT: Symbol("left"),
    RIGHT: Symbol("right"),
  });
  ```

## 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성할 수도 있음.

```js
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

console.log(obj[Symbol.for("mySymbol")]); // 1
```

> 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않음.

## 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for ... in`문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없음.

```js
const obj = {
  [Symbol.for("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); //
}

console.log(Object.keys(obj)); // []
console.log(Object.getOWnPropertyNames(obj)); // []
```

> 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉

- 하지만 `ES6`에 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 찾을 수 있음.

```js
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않음.
- 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋음.

  > 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문.

- 하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 안전.

```js
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for("sum")](); // 3
```

## `Well-known Symbol`

- `JavaScript`가 기본 제공하는 빌트인 심벌 값.
- 빌트인 심벌 값은 `Symbol` 함수의 프로퍼티에 할당.
- 이는 `JavaScript Engine` 내부 알고리즘에 사용.
- `Array`, `String`, `Map`, `Set`, `TypedArray`, `arguments`, `NodeList`, `HTMLCollection`과 같이 `for ... of` 문으로 순회 가능한 빌트인 이터러블은 `Well-known Symbol`인 `Symbol.iterator`를 키로 갖는 메서드를 가지며, `Symbol.iterator`를 호출하면 이터레이터를 반환하도록 `ECMAScript` 사양에 규정.
  > 빌트인 이터러블은 이 규정. 즉, 이터레이션 프로토콜을 준수.
- 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 됨.
- `Symbol.iterator`를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블.

```js
const iterable = {
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;

    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

- 이때 이터레이션 프로토콜을 준수하기 위해 일반 객체에 추가해야 하는 메서드의 키 `Symbol.iterator`는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않을 것.

> 이처럼 심벌은 중복되지 않는 상수 값을 생성하는 것은 물론, 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉. 하위 호환성을 보장하기 위해 도입.
