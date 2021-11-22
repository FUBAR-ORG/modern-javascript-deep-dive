# 22장 this

객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

## this

객체에서 동작을 나타내는 메서드는 프로퍼티를 참조하고 변경할 수 있어야한다. 따라서 메서드가 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야한다.

**this는 자신이 속한 객체 또는 생성할 인스턴스를 가르키는 특수한 식별자이자 자기 참조변수이다.** this를 통해 자신이 속한 객체나 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며 어디서든 참조할 수 있다. **this가 가르키는 값은 함수 호출 방식에 의해 동적으로 결정된다.**

## 함수 호출 방식과 this 바인딩

**렉시컬 스코프는 함수가 평가되어 함수 객체가 생성될 때 상위 스코프를 결정하고, this 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**

### 일반 함수 호출

일반 함수에서의 this는 전역 객체가 바인딩된다. 콜백 함수가 일반 함수로 호출되는 경우, 메서드 내에 중첩된 일반 함수 내부의 this 또한 전역 객체가 바인딩된다.

strict mode가 적용된 일반 함수 내부 this에는 `undefined`가 바인딩 된다.

```js
function outer() {
  console.log('outer ', this); // window
  function inner() {
    console.log('inner ', this); // window
  }
  inner();
}
outer();
```

### 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체가 바인딩된다.

```js
function Car(color) {
  this.color = color;
}

Car.prototype.getColor = function () {
  return this.color;
};

const myCar = new Car('Red');

// #1. getColor 메서드를 호출한 객체가 myCar이므로 this에 myCar 객체가 바인딩 된다.
console.log(myCar.getColor()); // Red

Car.prototype.color = 'Black';

// #2. getColor 메서드를 호출한 객체는 Car.prototype이다. 따라서 this는 Car.prototype를 가르킨다.
console.log(Car.prototype.getColor()); // Black
```

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수에 의해 생성되는 인스턴스가 바인딩된다. 생성자 함수를 `new` 키워드와 함께 호출하지 않으면 일반 함수로 동작한다.

```js
function Car(color) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가르킨다.
  this.color = color;
  this.getColor = function () {
    return this.color;
  };
}

const myCar = new Car('red');

console.log(myCar.getColor()); // red
```

### Function.prototype.apply / call / bind

Function.prototype.apply | call은 첫 번째 인수로 전달한 객체를 함수의 this에 바인딩한다.

```js
function returnThis() {
  return this;
}

const myObj = {
  name: 'Han',
};

console.log(returnThis()); // window, 일반 함수 내부 this는 전역객체 바인딩
console.log(returnThis.apply(myObj)); // {name: 'Han'}
console.log(returnThis.call(myObj)); // {name: 'Han'}
```

arguements 객체는 배열이 아니기 때문에 Array.prototype 메서드를 사용할 수 없다. 그러나 apply, call 메서드와 함께 사용하면 가능하다.

```js
function argsToArray() {
  // arguments 객체를 array로 변환
  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);

  // const arr = Array.prototype.slice(arguments); // []

  return arr;
}

argsToArray(1, 2, 3);
```

bind 메서드는 메서드의 this와 중첩/콜백 함수의 this가 불일치하는 문제를 해결하는데 사용된다.

```js
const person = {
  name: 'Han',
  itd(callback) {
    // #1 bind를 하지 않을 경우 일반 함수로 호출된 콜백 함수 이므로 this에 전역객체인 window가 바인딩된다.
    // setTimeout(callback, 100);

    // #2 bind 메서드를 통해 콜백 내부의 this를 콜백 외부의 this와 일치시킨다.
    setTimeout(callback.bind(this), 100);
  },
};

person.itd(function () {
  console.log('I am ', this.name);
});
// #1 -> I am // window의 name 프로퍼티는 '' 값을 가진다.
// #2 -> I am Han
```

<br>

## Summary

- this는 자신이 속한 객체 또는 생성할 인스턴스를 가르키는 특수한 식별자이자 자기 참조변수이다.
- this는 호출 방식에 따라 결정된다.
- 일반 함수로 호출된 모든 함수(중첩, 콜백 등) 내부의 this에는 전역 객체가 바인딩된다.
- 메서드 내부의 this는 메서드를 호출한 객체가 바인딩된다.
- 생성자 함수 내부의 this에는 생성자 함수에 의해 생성되는 인스턴스가 바인딩된다.
- Function.bind | apply | call 메서드를 통해 인수로 전달한 객체를 this에 바인딩 할 수 있다.
