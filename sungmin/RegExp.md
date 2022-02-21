# RegExp

## 정규 표현식이란?

- 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식언어(formal language)
- 자바스크립트 고유 문법이 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있음
- 자바스크립트는 펄의 정규 표현식 문법을 ES3부터 도입
- 문자열을 대상으로 **패턴 매칭 기능**을 제공

```ts
const tel = '010-2040-6695';
const regExp = /^\d{3}-\d{4}-\d{4}$/;

console.log(regExp.test(tel)); // false
```

## 정규 표현식의 생성

- 정규 표현식 리터럴을 사용
```ts
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regExp = /is/i;

console.log(regExp.test(target)); // true
```
- RegExp 생성자 함수를 사용
```ts
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색한다.
const regExp = new RegExp(/is/i); // ES6
// const regExp = new RegExp(/is/m 'i'); 
// const regExp = new RegExp('is', 'i'); 

console.log(regExp.test(target)); // true
```

## RegExp 메서드
- RegExp.prototype
    - **exec**: 대응되는 문자열을 찾는다. 정보를 가지고 있는 배열을 반환하며 대응되는 문자열을 찾지 못했다면 null을 반환한다.
    - **test**: 대응되는 문자열 여부에 따라 true | false를 반환한다.
- String.prototype
    - **match**: exec와 같은 동작을 한다. g 플래그 지정 시 exec는 첫 번째 매칭만 반환하지만 match는 모든 매칭 결과를 반환한다.
    - **replace**: 대응되는 문자열을 찾아 다른 문자열로 치환한다.
    - **search**: 대응되는 문자열이 있는지 검사해 대응된 부분의 인덱스를 반환한다. 대응되는 문자열을 찾지 못했다면 -1을 반환한다.
    - **split**: 대응되는 문자열을 찾아 전체 문자열을 배열로 분할해 반환한다.

```ts
const target = 'Is this all there is?';
const regExp = /is/;

// RegExp 메서드
regExp.exec(target); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
regExp.test(target); // true

// String 메서드
target.match(regExp); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
target.replace(regExp, ''); // 'Is th all there is?'
target.search(regExp); // 5
target.split(regExp); // ['Is th', ' all there ', '?']
```

## 플래그
- 정규 표현식의 검색 방식을 설정하기 위해 사용
- 선택적으로 사용할 수 가능
- 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능 

| 플래그 | 의미          | 설명                                   |
|-----|-------------|--------------------------------------|
| i   | ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.              |
| g   | Global      | 대상 문자열 내에서 패턴과 일지하는 모든 문자열을 전역 검색한다. |
| m   | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.           |

```ts
const target = 'Is this all there is?';

target.match(/is/); // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
target.match(/is/i); // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
target.match(/is/g); // ['is', 'is']
target.match(/is/gi); // ['Is', 'is', 'is']
target.match(/is/m); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

## 패턴
- 정규 표현식의 일정한 규칙을 표현하기 위해 사용
- /로 열고 닫으며 문자열의 따옴표는 생략한다.
- 따옴표를 포한한다면 패턴에 포함되어 검색된다.
- 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.

### 임의의 문자열 검색

- `.`은 문자 한 개를 의미한다. `.`이 3개 연속된 `...`은 3자리 문자열과 매치한다.
- []는 범위를 지정할 때 사용한다.

```ts
const target = 'Is this all there is?';
target.match(/.../g); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
target.match(/[a-z]/g); // ['s', 't', 'h', 'i', 's', 'a', 'l', 'l', 't', 'h', 'e', 'r', 'e', 'i', 's']
```

### 반복 검색

- {m, n}은 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
- {n}은 {n, n}이 축약된 패턴이다.
- {n,}은 n번 이상 반복되는 문자열을 의미한다.
- +는 최소 한번 이상 반복되는 문자열을 의미한다. -> {1,}
- ?는 0~1번 반복되는 문자열을 의미한다. -> {0,1}
- 콤마 뒤에 공백이 있으면 정상 동작하지 않는다.

```ts
const regExp = /A{1,2}/g // A가 최소 1번, 최대 2번 반복
const regExp2 = /A{2}/g // A가 2번 반복
const regExp3 = /A{2,}/g // A가 2번 이상 반복
const regExp4 = /A+/g // A가 최소 1번 이상 반복
const regExp5 = /A?B/g // A가 최대 한번(0번 포함) 반복되고 이어지는 문자가 B인 패턴
```

### OR 검색
- |은 or의 의미를 갖는다.
- `/A|B/`는 A 또는 B를 의미한다.
- [] 내의 문자는 or로 동작
- [] 내에 -를 사용해 범위를 지정한다.

```ts
const regExp = /A|b/g // 'A' 또는 'B'를 전역 검색
const regExp2 = /A+|B+/g // 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp3 = /[AB]+/g // 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp4 = /[A-Z]+/g // 'A' ~ 'Z'가 한번 이상 반복되는 문자열을 전역 검색
const regExp5 = /[A-Za-z]+/g // 'A' ~ 'Z' 또는 'a' ~ 'z'가 한번 이상 반복되는 문자열을 전역 검색
const regExp6 = /[0-9,]+/g // '0' ~ '9' 또는 ','가 한번 이상 반복되는 문자열을 전역 검색
```

- **\d**: 숫자를 의미한다. -> [0-9]
- **\D**: 숫자가 아닌 문자를 의미한다.
- **\w**: 알파벳, 숫자, 언더스코어를 의미한다. -> [a-zA-Z0-9_]
- **\W**: 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

### NOT 검색
- []내의 ^은 not의 의미를 갖는다.
- `/[^0-9]/`는 숫자를 제외한 문자열을 의미한다.

```ts
const regExp = /[^0-9]+/g // 숫자를 제외한 문자열을 전역 검색
```

### 시작 위치로 검색
- [] 밖의 ^은 문자열을 시작을 의미한다.
- `/^http/`는 문자열이 http로 시작하는지 검사한다.
```ts
const regExp = /^https/; // 'https'로 시작하는지 검사
```

### 마지막 위치로 검색
- $는 문자열의 마지막을 의미한다.
- `/com$/`은 문자열이 com으로 끝나는지 검사한다.
```ts
const regExp = /com$/; // 'com'으로 끝나는지 검사
```




















