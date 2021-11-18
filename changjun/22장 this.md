# 22장. this

## 1. this란?

> `this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다.

## 2. this의 이해

this가 가리키는 값, 즉 this 바인딩은 상황에 따라 동적으로 변한다.

- 일반 함수 내부
- 메소드 내부
- 생성자 내부
- call, apply, bind를 통한 호출

### 2-1 일반 함수 내부

```js
console.log(this === window); //true

var a = 30;
console.log(this.a); //30

function foo() {
  var b = 5;
  console.log(this.b); //undefined
  return this; //일반 함수 내부에서의 this는 전역객체이다.
}

foo() === window; //true;
```

### 2-2 메소드 내부

객체 리터럴의 메소드 내부에서의 `this`는 호출한 객체를 가리킨다.

```js
const person = {
  firstName: 'Changjun',
  lastName: 'Park',

  getFullName() {
    console.log(this.firstName + this.lastName);
  },
};

person.getFullName(); // ChagjunPark
```

but 메소드 내부에 콜백함수가 일반함수로 호출되면 `this`는 전역객체에 바인딩된다. (이를 해결하기 위해 arrow function을 사용)

```js
const person = {
  firstName: 'Changjun',
  lastName: 'Park',

  getFullName() {
    console.log(this.firstName + this.lastName);
    setTimeout(function () {
      console.log(this); //window;
    }, 1000);
  },
};

person.getFullName(); // ChagjunPark
```

### 2-3 생성자 내부

생성자로 호출 시 생성자 함수가 생성할 객체에 바인딩 된다.
바인딩된 this를 반환한다.

```js
function Circle(radius) {
  this.radius = radius;
}

const circle = new Circle(5);
console.log(circle.radius); //5;
```

### 2-4 call, apply, bind 함수에 의한 바인딩

```js
var value = 1;
const obj = {
  value: 100,
  foo() {
    setTimeout(
      function () {
        console.log(this.value);
      }.bind(this),
      100
    );
  },
};
obj.foo(); //100
```
