# 26장 ES6 함수의 추가 기능

ES6 이전의 모든 함수는 callable이면서 constructor였다. 그러나 ES6에서 함수 사용 목적에 따라 세 가지 종류로 구분되었다.

- 일반 함수: constructor, prototype, arguments
- 메서드: super(부모 오브젝트의 함수 호출), arguments
- 화살표 함수

## 메서드

ES6에서 메서드를 메서드 축약 표현으로 정의된 함수라고 명확히 정의했다. 또한 메서드는 non-constructor이며 prototype도 생성하지 않는다.

메서드는 [[HomeObject]]라는 자신을 바인딩한 객체를 가리키는 내부슬롯을 가진다. 그렇기 때문에 super 키워드를 사용할 수 있다.

## 화살표 함수

화살표 함수를 사용하면 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결할 수 있다.

화살표 함수의 특징은 다음과 같다.

- non-constructor이다.
- 중복된 매개변수 이름 선언이 가능하다.
- 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 상위 스코프의 this, arguments, super, new.target를 참조한다.

### this

콜백 함수를 일반 함수로서 호출하면 콜백 함수 내부와 외부의 this가 다르게 되는 문제가 발생한다.

**화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.** 즉, 화살표 함수의 this는 함수가 정의된 위치에 의해 결정된다.(lexical this)

화살표 함수는 자체적인 this 바인딩을 가지지 않기 때문에 Function.prototype.bind | call | apply 메서드를 사용해 내부의 this를 바꿀 수 없으며 메서드를 사용하더라도 적용되지 않고 상위 스코프의 this를 가리킨다.

```js
class Students {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // #1. 일반 함수 콜백: 콜백 함수 내부 this는 undefined 가리킴
    // return arr.map(function (item) {
    // return this.prefix + item;
    // }) -> TypeError

    // #2. 화살표 함수 콜백: 상위 스코프 this 바인딩
    return arr.map((item) => this.prefix + item);
  }
}

const students = new Students('Student Name: ');
console.log(students.add(['Kim', 'Han', 'Seo']));
// ['Student Name: Kim', 'Student Name: Han', 'Student Name: Seo']
```

## super, arguments

화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `My name is ${this.name}.`;
  }
}

class Han extends Person {
  // 화살표 함수의 super는 상위 스코프의 constructor의 super를 가르킨다.
  introduce = () => `${super.introduce()}`;
}

const me = new Han('Han');
console.log(me.introduce());
// My name is Han.
```

super와 마찬가지로 화살표 함수 내부에서 arguments를 참조하면 상위 스코프의 arguments를 참조한다.

## Rest 파라미터

Rest 파라미터는 `...` 키워드로 정의한 매개변수를 의미하며 함수에 전달된 인수들의 목록을 배열로 전달받는다. Rest 파라미터는 함수 정의시 하나만 선언할 수 있으며 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

```js
function sum(...args) {
  return args.reduce((acc, cur) => acc + cur, 0);
}

sum(1, 2, 3, 4, 5); // 15
```

<br>

## Summary

- ES6에서 함수 사용 목적에 따라 일반 함수, 메서드, 화살표 함수 3가지로 구분되었다.
- 메서드는 메서드 축약 표현으로 정의된 함수이며 super 키워드를 사용할 수 있다.
- 화살표 함수 내부의 this, super, arguments는 상위 스코프의 this, super, arguments를 참조한다.
- `...` 키워드로 정의한 매개변수인 rest 파라미터는 전달된 인수들을 배열로 전달한다.
