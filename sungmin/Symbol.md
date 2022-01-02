# Symbol

## 심벌이란?
심벌(Symbol)은 ES6에서 추가된 원시타입으로 모든 심벌 타입의 값은 다른 값과 **중복되지 않는 유일한 값**

## 심벌 값의 생성

1. Symbol 함수를 통한 생성
    - Symbol 함수로 생성
       ```js
       Symbol([description])
       // description: string
       ```
    - 다른 원시값과는 다르게 심벌 값을 정의하는 리터럴은 존재하지 않는다.
    - new 키워드를 통해 호출할 수 없다.
    - 문자열 인수를 선택적으로 전달 할 수 있다. 
      - 전달한 문자열은 심벌 값에 대한 설명이며 디버깅 용도로 사용
      - 심벌 값 자체에는 영향을 주지 않음
      - 같은 문자열을 인수로 전달해 생성한 심벌 값들이라도 각각 유일
    - 생성 시 인수로 전달한 설명은 description 프로퍼티로 참조 가능 
      - 다른 원시타입처럼 래퍼객체를 생성하여 Symbol.prototype의 프로퍼티를 참조한다.
         ```js
         const mySymbol = Symbol("mySymbol");
         console.log(mySymbol.description); // mySymbol
         ```
    - 심벌은 문자열이나 숫자로 암묵적 형변환되지 않는다. 다만 Symbol.prototype에 toString 메서드를 가지고 있다.
    - 불리언 타입으로는 암묵적으로 형변환 될 수 있다.
      ```js
      const mySymbol = Symbol("mySymbol");
      console.log(!!mySymbol); // true
      ```

2. Symbol.for / Symbol.keyFor 메서드
   - Symbol.for과 Symbol.keyFor을 통해 전역 심벌 레지스트리에 접근
   1. Symbol.for(key: string)
      - Symbol.for에는 인수로 문자열을 전달
      - 인수로 전달한 문자열을 키로 가지는 심벌 값이 전역 심벌 레지스트리에 있으면 해당 값을 반환
      - 키와 일치하는 값이 없으면 해당 문자열을 키로 가지는 새로운 심벌 값을 생성하고 이를 반환
      - 전역 심벌 레지스트리는 문자열 키와 심벌 값의 쌍을 저장하는 객체
      - 이를 통해 유일한 심벌 값을 전역에서 공유
      ```js
      const s1 = Symbol.for('mySymbol');
      const s2 = Symbol.for('mySymbol');
      console.log(s1 === s2); // true
      ```
   2. Symbol.keyFor(value: Symbol)
       - Symbol.keyFor에는 인수로 심벌을 전달
       - 인수로 받은 심벌이 전역 심벌 레지스트리에 저장되어 있으면 해당하는 심벌의 키를 반환
      ```js
      const s1 = Symbol.for('mySymbol');
      Symbol.keyFor(s1); // mySymbol
      ```


## 심벌과 프로퍼티
- 객체의 프로퍼티 키로 심벌을 사용할 수 있다.
- 심벌은 유일한 값이므로 프로퍼티의 키로 사용하면 다른 프로퍼티 키와 충돌하지 않는다.
- 키가 심벌인 프로퍼티는 for ...in 문이나 Object.keys, Object.getOwnPropertyNames로 찾을 수 없다. 따라서 심벌을 이용해 프로퍼티를 은닉할 수 있다.
- 단, ES6에 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 키가 심벌인 프로퍼티를 조회할 수 있다.
- 표준 빌트인 객체에 메서드를 추가할 때 심벌을 키로 사용하면 보다 안전하게 활용할 수 있다.

## Well-known Symbol
- 자바스크립트에는 기본적으로 제공하는 빌트인 심벌 값이 있다.
- Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.
- 예를 들어 Symbol.iterator는 이터레이터를 반환하는 메서드의 키로 사용되는 심벌이다.
- 순회 가능한 빌트인 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가지며 이 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.

```js
const iterable = {
   [Symbol.iterator](){
    let cur = 1;
    const max = 5;
    return {
       next(){
          return {value: cur++, done: cur > max + 1};
       }
    }  
   }
}
for(const num of iterable){
   console.log(num); // 1 2 3 4 5
}
```