# 스프레드 문법

ES6에서 도입된 스프레드 문법 `...`은 뭉쳐있는 여러 값들의 집합을 펼쳐 개별적인 값들의 목록으로 만든다.

- 이터러블에만 한정적으로 적용할 수 있다.
- 스프레드 문법의 결과는 값으로 사용할 수 없으므로 변수에 할당할 수 없다.

스프레드 문법의 결과물은 다음 과같은 문맥에서만 사용할 수 있다.

## 함수 호출문의 인수 목록

```js
const arr = [1, 2, 3];

const max = Math.max(arr); // NaN
const maxWithSpread = Math.max(...arr); // 3
```

Rest 파라미터는 전달된 인수들을 하나의 배열로 만들고, 스프레드 문법은 하나의 이터러블의 값을 개별적으로 분리하는 것이다. 즉, 스프레드 문법과 Rest 파라미터는 반대 개념이다.

```js
function foo(...rest) {
  console.log(rest);
}

foo(...[1, 2, 3]);
```

## 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 사용하면 ES6 이전에서 배열 리터럴을 사용하는 방식보다 더 효율적으로 코드를 작성할 수 있다.

```js
// concat
var c1 = [1, 2].concat([3, 4]); // es5, [1,2,3,4]
const c2 = [...[1, 2], ...[3, 4]]; // es6, [1,2,3,4]

// splice
var arr1 = [1, 4];
Array.prototype.splice.apply(arr, [1, 0].concat([2, 3]));
console.log(arr1); // es5, [1,2,3,4]

const arr2 = [1, 4];
arr2.splice(1, 0, ...[2, 3]);
console.log(arr2); // es6, [1,2,3,4]

// 배열 복사
const og = [1, 2];
var c1 = og.slice(); // es5 [1, 2]
const c2 = [...og]; // es6 [1,2]
```

## 객체 리터럴 내부에서 사용하는 경우

스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

> _cf_) 스프레드 프로퍼티는 2021년 TC39 stage 4단계에 제안되었음.

```js
// 객체 얕은 복사
const obj = { x: 1, y: 2 };
const copy = { ...obj }; // { x: 1, y: 2 }

// 객체 합치기
const merged1 = Object.assign({}, { x: 1, y: 2 }, { a: 3, b: 4 }); // // Object.assign (ES6)
const merged2 = { x: 1, y: 2, ...{ a: 3, b: 4 } }; // 스프레드 프로퍼티
console.log(merged1, merged2); // {x: 1, y: 2, a: 3, b: 4}, {x: 1, y: 2, a: 3, b: 4}
```

<br>

---

### Summary

- 스프레드 문법 `...`은 이터러블에 적용할 수 있는 전개연산자이다.
- 스프레드 문법의 결과는 값으로 사용할 수 없다.
- 스프레드 문법은 함수 호출문의 인수, 배열 리터럴 내부, 객체 리터럴 내부에서 사용할 수 있다.
