# 7장 연산자

## 7.3 비교 연산자

```javascript
-0 === 0; // true
Object.is(-0, +0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

## 7.8 typeof 연산자

```javascript
const foo = null;
typeof foo; // object
foo === null; // true

// 선언하지 않은 연산자 bar
typeof bar; // undefined
```

## 7.9 지수 연산자

지수 연산자는 ES7에서 도입 되었으며 좌항을 밑(base)으로, 우항의 피연산자를 지수(exponent)로 거듭 제곱해 숫자 값 반환하는 연산자이다.

```javascript
2**2 === Math.pow(2,2); // true

-5 ** 2 // SyntaxError
(-5) ** 2 // 25
```

## 7.10 그 외의 연산자

`delete`는 프로퍼티를 삭제하는 연산자이다.

```javascript
const obj = { a: 'a' };

delete obj.a;
obj; // {}
```
