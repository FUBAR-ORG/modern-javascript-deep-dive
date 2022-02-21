# 31장. RegExp

## 31.1 정규 표현식이란?

일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어이다.
정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다.
특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 때 사용한다.

```js
// 사용자로부터 입력받은 휴대폰 번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); //false
```

## 31.2 정규 표현식의 생성

정규 표현식 객체(RegExp 객체)를 생성하기 위해서 `정규 표현식 리터럴`과 `RegExp 생성자 함수`를 사용할 수 있다.

정규 표현식 리터럴은 `패턴`과 `플래그`로 구성된다.

- 정규 표현식 리터럴로 객체 생성

```js
const target = 'Is this all there is?';

// 패턴: is
// 플래그 : i => 대소문자 구별하지 않고 검색

const regexp = /is/i;

regexp.test(target); // true
```

- RegExp 생성자 함수로 객체 생성

```js
new RegExp(pattern[, flags])
```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec

`exec` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다. 매칭결과가 없는경우 null을 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;
regExp.exec(target);
```

### 31.3.2 RegExp.prototype.test

`test`메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;
regExp.test(target); //true
```

### 31.3.3. String.prototype.match

String 표준 빌트인 객체가 제공하는 `match`메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;
target.match(regExp);
```

`exec`메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하지만 `match`메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/g;
target.match(regExp); =["is","is"]
```

## 31.4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

- i: 대소문자를 구별하지 않고 패턴을 검색한다.
- g: 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
- m: 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서 상관 없이 하나 이상의 플래그를 동시에 설정할 수 있다.

```js
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
tatget.match(/is/i);

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);
```

## 31.5 패턴

정규 표현식은 일정한 규칙을 가진 문자여르이 집합을 표현하기 위해 사용하는 형식언어 이다. 패턴은 `/`로 열고 닫으며 문자열의 따옴표는 생략한다.

### 31.5.1 문자열 검색

- 대소문자 구분

```js
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); //true

// target과 정규 표현식 매칭 결과를 구한다.
target.match(regExp);
```

- 대소문자를 구별하지 않고 검색하려면 플래그 i를 사용한다.

```js
const target = 'Is this all there is?';

const regExp = /is/i;

target.match(regExp);
```

- 모든 문자열을 전역 검색하려면 플래그 g를 사용한다.

```js
const target = 'Is this all there is?';

const regExp = /is/g;

target.match(regExp);
```

### 31.5.2 임의의 문자열 검색

```js
const target = 'Is this all there is?';

const regExp = /.../g;

target.match(regExp); //["Is ", "thi",s l, 'll ', 'the', 're ', 'is?']
```

### 31.5.3 반복 검색

```js
const target = 'A AA B BB Aa Bb AAA';
// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // ['A','AA','A','AA', 'A']
```

```js
const target = 'A AA B BB Aa Bb AAA';
// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // ['AA','AA']
```

```js
const target = 'A AA B BB Aa Bb AAA';
// 'A'가 최소 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // ['AA','AAA']
```

```js
const target = 'A AA B BB Aa Bb AAA';
// 'A'가 최소 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // ['A','AA' ,'A', 'AAA']
```

?는 앞선 패턴이 최대 한번(0번 포함)이상 반복되는 문자열을 의미한다. 즉 ?는 `{0,1}`과 같다.

```js
const target = 'color colour';
// u가 최대 한번(0번 포함)이상 반복되고 그다음 r이나오는 문자열을 전역 탐색
const regExp = /colou?r/g;
target.match(regExp); // ['color','colour']
```

### 31.5.4 OR 검색

`|`는 or의 의미를 갖는다.

- A또는 B를 검색한다.

```js
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g;
target.match(regExp); //['A','A','A','B','B','B','A','B']
```

- 분해되지 않는 단어 레벨로 검색하기 위해서는 `+`를 함께 사용한다.

```js
const target = 'A AA B BB Aa Bb';
const regExp = /A+|B+/g;
target.match(regExp); //['A','A','A','B','B','B','A','B']
```

- `[]`내의 문자는 or로 동작한다. 그 뒤에 `+`를 사용하면 앞선 패턴을 한번 이상 반복한다.

```js
const target = 'A AA B BB Aa Bb';
const regExp = /[AB]+/g;
target.match(regExp); //['A','A','A','B','B','B','A','B']
```

- 범위를 지정하려면 []내에 `-`를 사용한다.

```js
const target = 'A AA B ZZ Aa Bb';
const regExp = /[A-Z]+/g;
target.match(regExp); //['A','A','A','B','B','B','A','B']
```

- 대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같다.

```js
const target = 'A AA B ZZ Aa Bb';
const regExp = /[A-Za-z]+/g;
target.match(regExp); //['A','A','A','B','B','B','A','B']
```

- 숫자를 검색하는 방법

```js
const target = 'AA BB 12,345';
const regExp = /[0-9, ]+/g;
target.match(regExp); //["12,345"]
```

- `\d`는 숫자를 의미한다. 즉 `\d`는 `[0-9]`d와 같다. `\D`는 반대로 문자를 의미한다.

```js
const target = 'AA BB 12,345';
const regExp = /[\d, ]+/g;
target.match(regExp); //["12,345"]
```

### 31.5.5 NOT 검색

[...] 내의 ^은 not의 의미를 갖는다. 예를들어 [^0-9]는 숫자를 제외한 문자를 의미한다

- 숫자를 제외한 문자열 검색

```js
const target = 'AA BB 12 Ab Bb';
const regExp = /[^0-9]+/g;
target.match(regExp);
```

## 31.5.6 시작 위치로 검색

[...]밖의 ^은 문자열의 시작을 의미한다. 단,[...] 내의 ^은 not의 의미를 가지므로 주의해야한다.

```js
const target = 'https://poiemaweb.com';
const regExp = /^https/;
regExp.test(target); //true
```

## 31.5.7 마지막 위치로 검색

`$`는 문자열의 마지막을 의미한다.

```js
const target = 'https://poiemaweb.com';
const regExp = /com$/;
regExp.test(target); //true
```

## 31.6 자주 사용하는 정규 표현식

### 31.6.1 특정 단어로 시작하는지 검사

### 31.6.2 특정 단어로 끝나는지 검사

### 31.6.3 숫자로만 이루어진 문자열인지 검사

### 31.6.4 하나 이상의 공백으로 시작하는지 검사

### 31.6.5 아이디로 사용가능한지 검사

### 31.6.6 메일 주소 형식에 맞는지 검사

### 31.6.7 핸드폰 번호 형식에 맞는지 검사

### 31.6.8 특수 문자 포함 여부 검사
