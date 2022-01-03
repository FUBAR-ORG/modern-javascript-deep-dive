# 16장 프로퍼티 어트리뷰트

### 들어가기 전에

- 내부 슬롯과 내부 메서드는 ECMAScript 사양에서 js 엔진의 구현 알고리즘을 설명하기 위해 사용되는 슈도 프로퍼티, 슈도 메소드이다.
- 모든 객체는 `[[prototype]]`이라는 내부 슬롯을 가진다. `__proto__`를 통해 간접적 접근이 가능하다.

## 프로퍼티 어트리뷰트

프로퍼티 어트리뷰트는 프로퍼티의 상태를 나타내는 내부 슬롯 `[[Value]]`, `[[Writable]]`(갱신 가능), `[[Enumerable]]`, `[[Configurable]]`(재정의 가능) 이다.

`Object.getOwnPropertyDescriptor` 메서드는 프로퍼티 어트리뷰트 정보를 포함하는 **프로퍼티 디스크립터 객체**를 반환한다.

```js
const obj = {
  author: 'Han',
};

Object.getOwnPropertyDescriptors(obj);
// 프로퍼티 디스크립터 객체
// author: {value: 'Han', writable: true, enumerable: true, configurable: true}
```

## 프로퍼티

일반적인 프로퍼티를 데이터 프로퍼티, 접근자 함수로 구성된 프로퍼티를 접근자 프로퍼티라고 한다.

데이터 프로퍼티는 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]` 프로퍼티 어트리뷰트를 가지며 js가 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

접근자 프로퍼티는 `[[Get]]`, `[[Set]]`, `[[Enumerable]]`, `[[Configurable]]` 프로퍼티 어트리뷰트를 가진다. `[[Get]]`, `[[Set]]`의 프로퍼티 디스크립터 객체의 프로퍼티에 get, set 프로퍼티를 통해 getter, setter 함수를 사용할 수 있다.

`Object.defineProperties` 메서드를 통해 데이터 프로퍼티의 접근자 프로퍼티의 프로퍼티 어트리뷰트를 정의할 수 있다.

## 객체 변경

객체의 변경을 제한하는 메서드는 다음과 같다. 단, 프로퍼티의 값이 객체인 경우 중첩된 객체에 대해서는 변경이 가능하다.

- Object.preventExtensions(obj): 프로퍼티 추가를 제한한다.
- Object.seal(obj): 프로퍼티 추가와 삭제, 프로퍼티 어트리뷰트의 재정의를 제한한다.
- Object.freeze(obj): 프로퍼티 값 읽기를 제외한 모든 변경 사항을 제한한다.
