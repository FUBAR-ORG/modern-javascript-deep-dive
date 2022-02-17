# `RegExp`

## 정규 표현식이란?

- 정규 표현식(`regular expression`)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(`formal language`).
- 대부분의 프로그래밍 언어와 코드 에디터에 내장.
- `JavaScript`는 `Perl`의 정규 표현식 문법을 `ES3`부터 도입.
- 정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공.

  - 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능.

- 휴대폰 전화번호 패턴 정규식

  ```js
  const tel = "010-1234-567팔";

  const regExp = /^\d{3}-\d{4}-\d{4}$/;

  regExp.test(tel); // false
  ```

- 정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크 가능.
- 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않은 문제점이 있음.

---

## 정규 표현식의 생성

- 정규 표현식 리터럴 생성.

  ```js
  /regexp/i;
  ```

  - 시작, 종료 기호(`/`).
  - 패턴(`pattern`).
  - 플리그(`flag`).

  > 정규 표현식 리터럴은 패턴과 플래그로 구성.

- `RegExp` 생성자 함수로 객체 생성.

  ```js
  new RegExp(pattern[, flags])

  new RegExp(/is/i); // ES6
  new RegExp(/is/, 'i');
  new RegExp('is', 'i');
  ```

---

## `RegExp` 메서드

### `RegExp.prototype.exec`

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환.
- 매칭 결과가 없는 경우 `null` 반환.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target); // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

- 문자열 내 모든 패턴을 검색하는 `g` 플래그를 지정해도 첫 번째 매칭 결과만 반환.

### `RegExp.prototype.test`

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 `boolean` 값으로 반환.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target); // true
```

### `String.prototype.match`

- 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환.

```js
const target = "Is this all there is?";
const regExp = /is/;

target.match(regExp); // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

- 문자열 내 모든 패턴을 검색하는 `g` 플래그를 지정하면 모든 매칭 결과를 배열로 반환.

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // ["is", "is"]
```

### 그 외

- `String.prototype.replace`, `String.prototype.search`, `String.prototype.split` 등

---

## 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용.
- 총 6개.

| 플래그 |    의미     |                            설명                            |
| :----: | :---------: | :--------------------------------------------------------: |
|  `i`   | ignore case |            대소문자를 구별하지 않고 패턴을 검색            |
|  `g`   |   Global    | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
|  `m`   | Multi line  |      문자열의 행이 바뀌더라도 패턴 검색을 계속 지속.       |

- 플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서 상관 없이 하나 이상의 플래그를 동시에 설정 가능.
- 플래그를 설정하지 않은 경우 대소문자를 구별해서 패턴을 검색하며, 첫 번째 매칭한 대상만 검색하고 종료.

```js
const target = "Is this all there is?";

target.match(/is/); // ["is", index: 5, input: "Is this all there is?", groups: undefined]
target.match(/is/i); // ["Is", index: 0, input: "Is this all there is?", groups: undefined]
target.match(/is/g); // ["is", "is"]
target.match(/is/gi); // ["Is", "is", "is"]
```

---

## 패턴

- 정규 표현식은 일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(`formal language`).
- 정규 표현식은 패턴과 플래그로 구성.
  - 패턴: 문자열의 일정한 규칙을 표현하기 위해 사용.
  - 플래그: 검색 방식을 설정하기 위해 사용.
- 패턴은 특별한 의미를 가지는 메타문자(`meta character`) 또는 기호로 표현.
- 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 정규 표현식과 매치(`match`)한다고 표현.

### 문자열 검색

- 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색.

### 임의의 문자열 검색

- `.`은 임의의 문자 한 개를 의미.

```js
const target = "Is this all there is?";
const regExp = /.../g;
target.match(regExp); // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 반복 검색

- `{m,n}`은 앞선 패턴이 최소 `m`번, 최대 `n` 번 반복되는 문자열을 의미.

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{1,2}/g;
target.match(regExp); // ["A", "AA", "A", "AA", "A"]
```

- `{n}`은 앞선 패턴이 `n`번 반복되는 문자열. 즉, `{n,n}`을 의미.

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2}/g;
target.match(regExp); // ["AA", "AA"]
```

- `{n,}`은 앞선 패턴이 최소 `n`번 이상 반복되는 문자열을 의미.

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A{2,}/g;
target.match(regExp); // ["AA", "AAA"]
```

- `+`는 앞선 패턴이 최소 한 번 이상 반복되는 문자열. 즉, `{1,}`를 의미.

```js
const target = "A AA B BB Aa Bb AAA";
const regExp = /A+/g;
target.match(regExp); // ["A", "AA", "A", "AAA"]
```

- `?`는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열. 즉, `{0,1}`를 의미.

```js
const target = "color colour";
const regExp = /colou?r/g;

target.match(regExp); // ["color", "colour"]
```

### `OR` 검색

- `|`는 `or`의 의미.

```js
const target = "A AA B BB Aa Bb";
const regExp = /A|B/g;
target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
```

- 분해되지 않은 단어 레벨로 검색하기 위해서 `+`를 함께 사용.

```js
const target = "A AA B BB Aa Bb";
const regExp = /A+|B+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

- `[]` 내의 문자는 `or`로 동작하며, 그 뒤에 `+` 적용.

```js
const target = "A AA B BB Aa Bb";
const regExp = /[AB]+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

- `[]` 내에 `-`를 사용하여 범위 지정.

```js
const target = "A AA BB ZZ Aa Bb";
const regExp = /[A-Z]+/g;
target.match(regExp); // ["A", "AA", "BB", "ZZ", "A", "B"]
```

- 대소문자를 구분하지 않는 방법.

```js
const target = "AA BB Aa Bb 12";
const regExp = /[A-Za-z]+/g;
target.match(regExp); // ["AA", "BB", "Aa", "Bb"]
```

- 숫자를 검색하는 방법.

```js
const target = "AA BB 12,345";
const regExp = /[0-9]+/g;
target.match(regExp); // ["12", "345"]
target.match(/[0-9,]+/g); // ["12,345"]
```

- `\d`는 숫자를 의미, `\D`는 숫자가 아닌 문자를 의미.

```js
const target = "AA BB 12,345";
const regExp = /[\d,]+/g;
target.match(regExp); // ["12,345"]
target.match(/\D,]+/g); // ["AA BB ", ","]
```

- `\w`는 알파벳, 숫자, 언더스코어. 즉, `[A-Za-z0-9_]`를 의미, `\W`는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미.

```js
const target = "Aa Bb 12,345 _$%&";
const regExp = /[\w,]+/g;
target.match(regExp); // ["Aa", "Bb", "12,345", "_"]
target.match(/\W,]+/g); // [" ", " ", ",", " $%&"]
```

### `NOT` 검색

- `[...]` 내의 `^`은 `NOT`을 의미.

```js
const target = "AA BB 12 Aa Bb";
const regExp = /[^0-9]+/g;
target.match(regExp); // ["AA BB Aa Bb"]
```

- `\d` = `[0-9]` => `\D` = `[^0-9]`
- `\w` = `[A-Za-z0-9_]` => `\W` = `[^A-Za-z0-9_]`

### 시작 위치로 검색

- `[...]` 밖의 `^`는 문자열의 시작을 의미.

```js
const target = "https://poiemaweb.com";
const regExp = /^https/;
regExp.test(target); // true
```

### 마지막 위치로 검색

- `$`는 문자열의 마지막을 의미.

```js
const target = "https://poiemaweb.com";
const regExp = /com$/;
regExp.test(target); // true
```

---

## 자주 사용하는 정규 표현식

### 특정 단어로 시작하는지 검사

- `http://` 또는 `https://`로 시작하는지 검사

```js
/^https?:\/\//;

/^(http|https):\/\//;
```

### 특정 단어로 끝나는지 검사

- `html`로 끝나는지 검사.

```js
/html$/;
```

### 숫자로만 이루어진 문자열인지 검사

```js
/^\d+$/;
```

### 하나 이상의 공백으로 시작하는지 검사

- `\s`는 여러 가지 공백 문자(스페이스, 탭 등)을 의미. 즉, `[\t\r\n\v\f]`와 같은 의미.

```js
/^[\s]+/;
```

### 아이디로 사용 가능한지 검사

- 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사.

```js
/^[A-Za-z0-9]{4,10}$/;
```

### 메일 주소 형식에 맞는지 검사

```js
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/;
```

- 인터넷 메세지 형식(`internet message format`) 규약인 `RFC 5322`에 맞는 정교한 패턴 매칭이 필요하다면 복잡한 패턴을 사용해야 함.

### 핸드폰 번호 현식에 맞는지 검사

```js
/^\d{3}-\d{3,4}-\d{4}$/;
```

### 특수 문자 포함 여부 검사

- 특수 문자는 `A-Za-z0-9`의외의 문자.

```js
/[^A-Za-z0-9]/gi;
```

- 특수 문자를 선택적으로 검사할 수 있는 방법.

```js
/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi;
```

- 특수 문자 제거.

```js
target.replace(/[^A-Za-z0-9]/gi, "");
```
