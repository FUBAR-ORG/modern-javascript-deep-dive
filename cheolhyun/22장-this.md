# `this`

## `this` 키워드

자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수(`self-referencing variable`).**  
`this`를 통해 자신이 속한 객체 또는 사진이 생성할 인스턴스의 프로퍼티나 메소드 참조 가능.

- `this`는 `JavaScript Engine`에 의해 암묵적으로 생성하여 코드 어디서든 참조.
- 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달.
- 함수 내부에서 지역 변수처럼 `arguments`와 `this` 사용 가능.
- **단, `this`가 가리키는 값, 즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정.**(`strict mode` 역시 영향을 줌.)

#### `this` 바인딩(`this binding`)

변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩 하는 것.

> `this`(키워드로 분류되지만 식별자의 역할)와 `this`가 가리킬 객체를 바인딩.

---

## 함수 호출 방식과 `this` 바인딩

#### 렉시컬 스코프와 `this` 바인딩은 결정 시기가 다름.

- 렉시컬 스코프: 함수의 상위 스코프를 결정하는 방식, 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정.
- `this` 바인딩: 함수 호출 시점에 결정.

#### 함수 호출 방식

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. `Function.prototype.apply` / `call` / `bind` 메서드에 의한 간접 호출

### 일반 함수 호출

> 기본적으로 `this`에는 전역 객체가 바인딩.

- 전역 함수, 중첩 함수, 콜백 함수를 일반 함수로 호출하면 `this`에는 전역 객체가 바인딩.
- 일반 함수에서의 `this`는 의미가 없음.
- `strict mode`가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩.

하지만 메서드 내에서 정의한 중첩 함수 또는 메서드에게 전달한 콜백 함수(보조 함수)가 일반 함수로 호출될 때 메서드 내의 중첩 함수 또는 콜백 함수의 `this`가 전역 객체를 바인딩 하는 것은 문제.

외부 함수인 메서드와 중첩 함수, 콜백 함수의 `this`가 일치하지 않는다는 것은 헬퍼 함수로 동작하기 어렵게 만듬.

- 일치시키기 위한 방법

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);

    // 혹은 명시적으로 this를 바인딩한다.
    setTimeout(
      function () {
        console.log(this.value);
      }.bind(this),
      100
    );

    // 또는 화살표 함수를 사용.
    setTimeout(() => console.log(this.value), 100);
  },
};

obj.foo();
```

### 메서드 호출

> `this`를 호출한 객체. 즉, 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩.  
> 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩.

```js
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩.
    return this.name;
  },
};

console.log(person.getName()); // Lee

const anotherPerson = {
  name: "Kim",
};

anotherPerson.getName = person.getName;
console.log(anotherPerson.getName()); // Kim

const getName = person.getName;

console.log(getName());
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같음.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''.
// Node.js 환경에서 this.name은 undefined.
```

> 프로토타입 메서드 내부에서 사용된 `this`도 해당 메서드를 호출한 객체에 바인딩.

```js
function Person(name) {
    this.name = name;
}

Person.prototype.getName = funciton () {
    return this.name;
}

const me = new Person('Lee');
cosole.log(me.getName()); // Lee

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName()); // Kim
```

### 생성자 함수 호출

> 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩.

생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수.  
일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면 생성자 함수로 동작.  
`new` 연산자를 사용하지 않으면 일반 함수로 동작.

- `new` 연산자를 사용한 생성자 함수

```js
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

- `new` 연산자를 사용하지 않은 일반 함수

```js
// 일반적인 함수의 호출.
const circle3 = Circle(15);

console.log(circle3); // undefined

console.log(radius); // 15
```

### `Function.prototype.apply` / `call` / `bind` 메서드에 의한 간접 호출

#### `apply` / `call`

> `apply`와 `call`의 본질적인 기능은 함수를 호출하는 것.

- `Function.prototype.apply`, `Function.prototype.call` 메서드는 `this`와 사용할 객체와 인수 리스트를 인수로 전달 받아 함수를 호출.
- `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우에 매우 유용.

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수 호출.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수로 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray]);

/**
 * 주어진 this 바인딩과 , 로 구분된 인수 리스트를 사용하여 함수 호출.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수로 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]]);
```

#### `bind`

> 함수를 호출하지 않고 `this`로 사용할 객체만 전달.

- `Function.prototype.bind` 메서드는 `this`로 사용할 객체만 전달 받아 함수 생성.
- 메서드의 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하는 경우에 매우 유용.

|           함수 호출 방식           |             `this` 바인딩              |
| :--------------------------------: | :------------------------------------: |
|           일반 함수 호출           |               전역 객체                |
|            메서드 호출             |          메서드를 호출한 객체          |
|          생성자 함수 호출          | 생성자 함수가 (미래에) 생성할 인스턴스 |
| `apply` 메서드 등에 의한 간접 호출 |       첫 번째 인수로 전달한 객체       |
