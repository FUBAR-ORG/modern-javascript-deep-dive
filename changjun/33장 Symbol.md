# 33장. 심벌

## 심벌이란?

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입이다.  
심벌은 다른 값과 중복되지 않는 유일무이한 값이다 따라서 충돌 위험이 없는 프로퍼티의 키값으로 사용된다.

## 심벌값의 생성

- 심벌 값은 `Symbol`함수를 호출하여 생성한다.

```js
const sb = Symbol('apple');
const copy = Symbol('apple');
```

- 심벌은 원시 값이라 `new`를 이용해서 생성하면 안된다.

- 심벌은 `description` 프로퍼티로 참조가 가능하다.

```js
const sb = Symbol('apple');
console.log(sb.description); //apple
```

- 문자열 or 숫자형으로 암묵적 변환 불가능

```js
const sb = Symbol('apple');
console.log(sb + 'hello');
// TypeError: Cannot convert a Symbol value to a string
```

- 불리언으로는 암묵적 변환 가능

```js
const sb = Symbol('apple');
console.log(!sb); //false;
console.log(!!sb); //true;
```

## Symbol.for , Symbol.keyFor

- Symbol.for 메서드를 사용하면 전역 심벌 레지스트리서 해당 키와 일치하는 심벌 값을 검색한다.

```js
// 전역 심벌 레지스트리에 'test'라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const sb1 = Symbol.for('test');
// 전역 심벌 레지스트리에 'test'라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const sb2 = Symbol.for('test');
console.log(sb1 === sb2); //true
```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 키값을 추출 할 수 있다.

```js
const sb1 = Symbol.for('test');
// 전역 심벌 레지스트리에 'test'라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const sb2 = Symbol.for('test');
Symbol.keyFor(sb1); //test
```
