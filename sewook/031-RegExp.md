# 31장 RegExp

정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)다.

- 자바스크립트의 고유 문법이 아니며 대부분의 언어에 내장되어 있다.
- 자바스크립트에는 ES3에서 도입되었다.
- 정규 표현식은 특정 패턴과 일치하는 문자열을 검색/추출/치환 하는 **패턴 매칭 기능**을 제공한다.

<br>

## 정규 표현식의 생성

정규 표현식 객체는 리터럴과 RegExp 생성자 함수를 사용해 생성할 수 있다.

```js
const regExpWithLiteral = /pattern/;
const regExpWithConstructor = new RegExp(/pattern/);
```

<br>

## RegExp 메서드

정규 표현식을 사용하는 메서드는 다음과 같다.

- RegExp.prototype
  - **exec**: 대응되는 문자열을 찾는다. 정보를 가지고 있는 배열을 반환하며 대응되는 문자열을 찾지 못했다면 null을 반환한다.
  - **test**: 대응되는 문자열 여부에 따라 true | false를 반환한다.
- String.prototype
  - **match**: exec와 같은 동작을 한다. g 플래그 지정 시 exec는 첫 번째 매칭만 반환하지만 match는 모든 매칭 결과를 반환한다.
  - **replace**: 대응되는 문자열을 찾아 다른 문자열로 치환한다.
  - **search**: 대응되는 문자열이 있는지 검사해 대응된 부분의 인덱스를 반환한다. 대응되는 문자열을 찾지 못했다면 -1을 반환한다.
  - **split**: 대응되는 문자열을 찾아 전체 문자열을 배열로 분할해 반환한다.

```js
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

<br>

## 플래그

플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 옵셔널이며 하나 이상의 플래그를 동시에 설정할 수 있다.

- **i**: 대소문자를 구분하지 않도록 설정한다.
- **g**: 일치하는 모든 부분을 선택하도록 설정한다.
- **m**: 행이 바뀌더라도 패턴 검색을 계속한다.
- y: 대상 문자열의 현재 위치부터 비교를 시작하도록 설정한다.
- s: .이 개행 문자 \n도 포함하도록 ‘dotall’ 모드를 활성화한다.([설명](https://ko.javascript.info/regexp-character-classes))
- u: 유니코드 전체를 지원한다.([설명](https://ko.javascript.info/regexp-unicode))

```js
const target = 'Is this all there is?';

target.match(/is/i); // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
target.match(/is/g); // ['is', 'is']
target.match(/is/gi); // ['Is', 'is', 'is']
target.match(/is/m); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

<br>

## 패턴

정규 표현식은 패턴과 플래그로 구성된다. 패턴은 문자열의 일정한 규칙을 표현하며 플래그는 검색 방식을 설정한다.

### 임의의 문자열 검색

- `.`은 문자 한 개를 의미한다. `.`이 3개 연속된 `...`은 3자리 문자열과 매치한다.
- []는 범위를 지정할 때 사용한다.

```js
const target = 'Is this all there is?';
target.match(/.../g);
// ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
target.match(/[a-z]/g);
// ['s', 't', 'h', 'i', 's', 'a', 'l', 'l', 't', 'h', 'e', 'r', 'e', 'i', 's']
```

### 반복 검색

- {m, n}은 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
- {n}은 {n, n}이 축약된 패턴이다.
- {n, }은 n번 이상 반복되는 문자열을 의미한다.
- +는 한번 이상 반복되는 문자열을 의미한다. -> {1,}
- ?는 0~1번 반복되는 문자열을 의미한다. -> {0,1}
- 콤마 뒤에 공백이 있으면 정상 동작하지 않는다.

```js
/A{1,2}/g // A가 최소 1번, 최대 2번 반복
/A{2}/g // A가 2번 반복
/A{2, }/g // A가 2번 이상 반복

/A+/g // A가 최소 1번 이상 반복
/A?B/g // A가 최대 한번(0번 포함) 반복되고 이어지는 문자가 B인 패턴
```

### OR 검색

- |은 or의 의미를 갖는다.
- `/A|B/`는 A 또는 B를 의미한다.

```js
const target = 'A AA B BB Aa Bb';

/A|b/g // A 또는 B를 전역 검색

/A+|B+/g === /[AB]+/g // A 또는 B가 한번이상 반복되는 문자열
// A AA B BB A B
```

- **\d**: 숫자를 의미한다. -> [0-9]
- **\D**: 숫자가 아닌 문자를 의미한다.
- **\w**: 알파벳, 숫자, 언더스코어를 의미한다. -> [a-zA-Z0-9_]
- **\W**: 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```js
const target = 'AA BB 12,345 _$%&';

/[\d,]+/g // 0-9 또는 ,가 한번 이상 반복
// ['12,345']

/[\D,]+/g // 숫자가 아닌 문자 또는 ,가 한번 이상 반복
// ['AA BB ', ',', ' _$%&']

/[\w,]+/g // 알파벳, 숫자, 언더스코어, ','가 반복되는 문자열
// ['AA', 'BB', '12,345', '_']
```

### NOT 검색

- []내의 ^은 not의 의미를 갖는다.
- `/[^0-9]/`는 숫자를 제외한 문자열을 의미한다.

### 위치 검색

- [] 밖의 ^은 문자열을 시작을 의미한다.
- `/^http/`는 문자열이 http로 시작하는지 검사한다.
- $는 문자열의 마지막을 의미한다.
- `/com$/`은 문자열이 com으로 끝나는지 검사한다.

### Summary

| 구분         | 키워드                       | 설명                            |
| ------------ | ---------------------------- | ------------------------------- |
| **메서드**   | string.match(regex)          | 대응 결과를 반환                |
|              | string.replace(regex, '...') | 대응 결과를 문자열로 치환       |
|              | string.replace(regex)        | 대응된 부분의 인덱스 반환       |
|              | regex.test(string)           | 대응 여부 반환                  |
| **플래그**   | i                            | 대소문자 구분 안함              |
|              | g                            | 전역 검색                       |
|              | m                            | 행이 바뀌어도 검색              |
| **특수문자** | {m, n}                       | 앞 문자 최소 m번, 최대 n번 반복 |
|              | \|                           | OR                              |
|              | +                            | 앞 문자 A 한번 이상 반복        |
|              | [^]                          | NOT                             |
|              | ^....                        | ...으로 시작하는지 검사         |
|              | ...$                         | ...으로 끝나는지 검사           |
|              | \d                           | 숫자                            |
|              | \D                           | 숫자 아닌 문자                  |
|              | \w                           | 알파벳, 숫자, 언더스코어        |
|              | \s                           | 공백 문자(\t\r\n\v\f)           |
