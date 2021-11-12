# 19장 프로토타입

## 객체지향 프로그래밍

객체지향에서 **객체**란 상태를 나타내는 프로퍼티와 프로퍼티를 조작할 수 있는 메서드를 논리적인 단위로 묶은 자료구조이다. 객체지향 프로그래밍은 이러한 객체의 집합으로 프로그램을 표현하려는 패러다임이다. 자바스크립트는 프로토타입 기반의 객체지향 프로그래밍 언어이다.

또한 객체지향에서 추상화란 객체의 다양한 속성 중 필요한 프로그램에 필수적인 속성만 간추리는 것 또는 공통된 속성이나 기능을 하나의 객체로 묶는 것을 말한다.

### 추상화 예제 ..?

<br>

## 상속, 프로토타입

**상속은 객체의 프로퍼티와 메서드를 그대로 사용하는 것**이다. js는 프로토타입 기반으로 상속을 구현하며 상속을 통해 코드를 재사용할 수 있다.

**프로토타입은 객체의 부모 객체 역할을 하는 객체**이며 상속받은 자식 객체는 프로토타입의 프로퍼티와 메서드를 사용할 수 있다.

프로토타입은 객체의 `[[Prototype]]` 내부 슬롯에 의해 참조된다. 객체는 `__proto__` 접근자 프로퍼티로 이 내부 슬롯 즉, 자신의 프로토타입에 접근할 수 있다.

모든 객체는 하나의 프로토타입을 갖는다. 프로토타입은 생성자 함수와 연결되어 있다.

![img](https://www.howdy-mj.me/static/a88859306abea59f9f7b0277f510fe8c/891d5/connect.png)

<br>

## 프로토타입의 생성

**프로토타입과 생성자 함수는 언제나 pair로 존재하며 프로토타입은 생성자 함수가 생성되는 시점에 생성된다.**

### 사용자 정의 생성자 함수

사용자가 정의한 생성자 함수로 생성되는 객체의 프로토타입은 생성자 함수가 평가되어 함수 객체를 생성하는 시점에 생성된다. 생성자 함수는 `prototype` 프로퍼티를 가지고 있는데 이 프로퍼티는 생성된 프로토타입을 가르킨다. 또한 생성된 프로토타입은 생성자 함수를 가르키는 `constructor` 프로퍼티만을 가지고 있다.

### 사용자 정의 생성자 함수로 생성된 객체

생성자 함수로 생성되는 객체의 프로토타입은 생성자 함수의 `prototype`에 바인딩되어있는 객체다.

```js
function Car(color) {
  this.color = color;
}

const myCar1 = new Car('red');
const myCar2 = new Car('blue');

console.log(Car.prototype === myCar1.__proto__); // true
console.log(myCar1.__proto__ === myCar2.__proto__); // true

Car.prototype.printColor = function () {
  console.log(this.color);
};

// Car 생성자 함수로 생성된 객체는 프로토타입에 추가된 메서드를 상속받는다.

myCar1.printColor(); // red
myCar2.printColor(); // blue
```

### built in 생성자 함수

빌트인 생성자 함수로 생성되는 객체의 프로토타입은 전역 객체가 생성되는 시점에 생성된다. 생성자 함수는 생성자 함수 객체가 생성되는 시점에 프로토타입이 생성되지만 빌트인 생성자 함수는 전역객체가 생성될 때 생성되므로 이미 프로토타입이 존재해있다. 또한 빌트인 생성자 함수 프로토타입는 다양한 빌트인 메서드를 가지고 있다.

### 리터럴 표기법으로 생성된 객체

리터럴 표기법으로 생성된 객체는 가상적인 빌트인 생성자 함수로 생성된다고 생각하면 된다. 따라서, 리터럴 표기법으로 생성된 객체도 빌트인 생성자 함수로 생성된 객체와 같은 프로토타입을 가진다고 생각하면 된다.

```js
const obj1 = {};

const obj2 = new Object();

// obj1은 객체 리터럴로 생성된 객체지만 new Object로 생성된 객체와 동일한 프로토타입을 가진다.
console.log(obj1.__proto__ === obj2.__proto__); // true
console.log(obj1.__proto__ === Object.prototype); // true
```

<br>

## 프로토타입 체인

프로토타입 체인의 최상위 객체는 항상 `Object.prototype`이다. 즉, 프로토타입의 프로토타입은 `Object.prototype`이다. 또한 `Object.prototype`의 프로토타입은 `null`이다.

객체의 프로퍼티에 접근할려고 할 때 객체에 해당 프로퍼티가 없을 경우, `[[Prototype]]` 내부 슬롯의 참조를 따라 부모 객체 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 체인의 최상위인 `Object.prototype`에서도 프로퍼티를 찾지 못하면 에러가 아닌 `undefined`를 반환한다.

```js
function Car(color) {
  this.color = color;
}

Car.prototype.printColor = function () {
  console.log(this.color);
};

const car = new Car('red');

console.log(car.hasOwnProperty('color')); // true
```

위 예제에서 `car` 객체의 프로토타입인 `Car.prototype`은 `hasOwnProperty` 프로퍼티를 가지고 있지 않다. 그러나 프로토타입 체인상 `Car.prototype`의 프로토타입인 `Object.prototype`의 `hasOwnProperty` 프로퍼티에 접근이 가능하므로 콘솔에 `true`가 출력된다.

<br>

## 프로토타입 변경

프로토타입은 생성자 함수 또는 인스턴스를 통해 변경할 수 있다.

```js
const Car = (function () {
  function Car(color) {
    this.color = color;
  }

  // Car 생성자 함수의 프로토타입에 객체 리터럴 할당
  // constructor 프로퍼티가 존재하지 않는다.
  Car.prototype = {
    printColor() {
      console.log(this.color);
    },
  };

  return Car;
})();

const myCar = new Car('red');
console.log(myCar.__proto__.constructor); // Object
```

생성자 함수를 통해 프로토타입을 변경하면 생성자 함수의 프로토타입의 `constructor`가 파괴된다. 따라서 생성된 객체의 생성자 함수에 접근하면 생성자 함수가 아닌 프로토타입체인 최상위에 위치한 `Object` 생성자 함수가 나온다.

```js
function Car(color) {
  this.color = color;
}

const myCar = new Car('red');

myCar.__proto__.constructor; // Car

const car = {
  printColor() {
    console.log(this.color);
  },
};

Object.setPrototypeOf(myCar, car);

myCar.printColor(); // red
myCar.__proto__.constructor; // Object
```

인스턴스의 경우 `Object.setPropertyOf(인스턴스, 교체할 프로토타입 객체)`를 통해 프로토타입을 변경할 수 있다.

<br>

## 기타 연산자, 메서드

### instanceOf

`객체 instanceOf 생성자 함수`

```js
myCar instanceOf Car
```

생성자 함수의 `prototype`에 바인딩된 프로토타입이 객체의 프로토타입 체인상 존재하면 `true`로 평가된다.

프로토타입이 변경되어 생성자 함수와의 연결이 파괴되면 프로토타입 체인상 존재하지않기 때문에 `false`로 평가된다.

### Object.create()

`Object.create(프로토타입 객체, 프로퍼티 키와 디스크립터 객체로 이루어진 객체)`

```js
const obj = Object.create(Object.prototype, {
  x: { value: 1, writable: .... }; // 프로퍼티 키와 디스크립터 객체
});

console.log(obj.x); // 1
```

명시적 프로토타입을 지정을 통해 새로운 객체를 생성하는 직접 상속을 위한 메서드이다.
