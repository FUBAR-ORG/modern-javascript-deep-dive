# `Set`과 `Map`

## `Set`

- 중복되지 않는 유일한 값들의 집합.

- 배열과의 차이점

|        구분        | 배열 | `Set` 객체 |
| :----------------: | :--: | :--------: |
|    값 중복 포함    |  O   |     X      |
| 요소의 순서에 의미 |  O   |     X      |
| 인덱스로 요소 접근 |  O   |     X      |

> `Set` 객체는 수학적 집합의 특성과 일치하며, 수학적 집합을 구현하기 위한 자료구조.

### `Set` 객체의 생성

```js
const set = new Set(); // Set(0) {}

const set1 = new Set([1, 2, 3, 3]); // Set(3) {1, 2, 3}
const set2 = new Set("hello"); // Set(4) {"h", "e", "l", "o"}
```

- `Set` 생성자 함수로 생성.
- `Set` 생성자 함수는 이터러블을 인수로 전달받아 `Set` 객체 생성.
- 이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않음.
- 이 특성을 통해 손쉽게 중복 요소 제거 가능.

```js
const uniqArray = (array) =>
  array.filter((v, i, self) => self.indexOf(v) === i);
const uniqSet = (array) => [...new Set(array)];
```

### 요소 개수 확인

#### `Set.prototype.size`

```js
const { size } = new Set([1, 2, 3, 3]); // 3
```

- `size` 프로퍼티는 `setter` 함수 없이 `getter` 함수만 존재하는 접근자 프로퍼티.

### 요소 추가

#### `Set.prototype.add`

```js
const set = new Set();
set.add(1); // Set(1) {1}
set.add(2).add(3); // Set(3) {1, 2, 3}
```

- `add` 메서드는 새로운 요소가 추가된 `Set` 객체를 반환하므로 연속적으로 호출(`method chaining`) 가능.
- 중복 요소 추가는 허용되지 않으나, 에러는 발생하지 않음.
- 객체나 배열과 같은 `JavaScript`의 모든 값을 요소로 저장 가능.

  - `NaN`의 평가

    - `JavaScript Engine`은 `NaN`과 `NaN`을 다르다고 평가.
    - 하지만 `Set` 객체는 `NaN`과 `NaN`을 갖다고 평가하여 중복을 허용하지 않음.

    ```js
    const set = new Set();

    console.log(NaN === NaN); // false
    console.log(0 === -0); // true

    set.add(NaN).add(NaN);
    ```

### 요소 존재 여부 확인

#### `Set.prototype.has`

```js
const set = new Set([1, 2, 3]);
set.has(2); // true
set.has(4); // false
```

### 요소 삭제

#### `Set.prototype.delete`

- `delete` 메서드는 삭제 성공 여부를 나타내는 `boolean` 값 반환.
- 삭제하려는 요소 값을 인수로 전달.
- 존재하지 않는 요소를 삭제해도 에러는 발생하지 않음.

```js
const set = new Set([1, 2, 3]);
set.delete(2); // true
```

### 요소 일괄 삭제

#### `Set.prototype.clear`

```js
const set = new Set([1, 2, 3]);
set.clear();
console.log(set); // Set(0) {}
```

### 요소 순회

#### `Set.prototype.forEach`

- `Array.prototype.forEach`와 유사하게 콜백 함수와 함수의 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달.
- 콜백 함수에는 3개의 인수를 전달 받음.

  1. 현재 순회 중인 요소 값.
  2. 현재 순회 중인 요소 값.
  3. 현재 순회 중인 `Set` 객체 자체.

  - 1, 2번 인수는 같은 값이지만 `Array.prototype.forEach`와 인터페이스를 통일하기 위함.

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- `Set` 객체는 이터러블이므로 `for ... of` 문으로 순회 가능하며, 스프레드 문법과 배열 디스트럭처링의 대상으로 사용 가능.

> `Set` 객체는 요소의 순서에 의미가 없지만, 순회하는 순서는 요소가 추가된 순서를 따름.

> 다른 이터러블 순회와 호환성을 유지하기 위함.

### 집합 연산

#### 교집합

- Example_1

```js
Set.prototype.intersection = function (set) {
  const result = new Set();
  for (const value of set) {
    if (this.has(value)) result.add(value);
  }
  return result;
};
```

- Example_2

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};
```

#### 합집합

- Example_1

```js
Set.prototype.union = function (set) {
  const result = new Set(this);
  for (const value of set) {
    result.add(value);
  }
  return result;
};
```

- Example_2

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};
```

#### 차집합

- Example_1

```js
Set.prototype.difference = function (set) {
  const result = new Set(this);
  for (value of set) {
    result.delete(value);
  }
  return result;
};
```

- Example_2

```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};
```

#### 부분 집합과 상위 집합

- Example_1

```js
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    if (!this.has(value)) return false;
  }
  return true;
};
```

- Example_2

```js
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};
```

---

## `Map`

- 키와 값의 쌍으로 이루어진 컬렉션.

- 객체와의 차이점

|          구분          |           객체            |      `Map` 객체       |
| :--------------------: | :-----------------------: | :-------------------: |
| 키로 사용할 수 있는 값 |    `string`, `Symbol`     | 객체를 포함한 모든 값 |
|        이터러블        |             X             |           O           |
|     요소 개수 확인     | `Object.keys(obj).length` |      `map.size`       |

### `Map` 객체의 생성

```js
const map = new Map(); // Map(0) {}

const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]); // Map(2) {"key1" => "value1", "key2" => "value2"}
const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
const map3 = new Map([
  ["key1", "value1"],
  ["key1", "value2"],
]); // Map(1) {"key1" => "value2"}
```

- `Map` 생성자 함수로 생성.
- `Map` 생성자 함수는 이터러블을 인수로 받아 `Map` 객체 생성.
- 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성.
- 중복된 키를 갖는 요소가 존재하면 덮이므로, 중복된 키를 갖는 요소가 존재할 수 없음.

### 요소 개수 확인

#### `Map.prototype.size`

```js
const { size } = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]); // 2
```

- `size` 프로퍼티는 `setter` 함수 없이 `getter` 함수만 존재하는 접근자 프로퍼티.

### 요소 추가

#### `Map.prototype.set`

```js
const map = new Map();

map.set("key1", "value1");
map.set("key2", "value2").set("key3", "value3");

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");
```

- `set` 메서드는 새로운 요소가 추가된 `Map` 객체를 반환하므로 연속적으로 호출(`method chaining`) 가능.
- 중복된 키를 갖는 요소를 추가하면 기존 값이 덮어써지고, 에러는 발생하지 않음.
- 객체와 다르게 키 타입으로 모든 값을 사용할 수 있음.

  - `NaN`의 평가

    - `JavaScript Engine`은 `NaN`과 `NaN`을 다르다고 평가.
    - 하지만 `Map` 객체는 `NaN`과 `NaN`을 갖다고 평가하여 중복을 허용하지 않음.

    ```js
    const map = new Map();

    map.set(NaN, "value1").set(NaN, "value2");
    ```

### 요소 취득

#### `Map.prototype.get`

```js
const map = Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

- `Map` 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 `undefined` 반환.

### 요소 존재 여부 확인

#### `Map.prototype.has`

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.has(lee); // true
map.has("key"); // false
```

### 요소 삭제

#### `Map.prototype.delete`

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);
map.delete(kim);
```

- `delete` 메서드는 삭제 성공 여부를 나타내는 `boolean` 값 반환.
- 삭제하려는 요소의 키를 인수로 전달.
- 존재하지 않는 요소를 삭제해도 에러는 발생하지 않음.

### 요소 일괄 삭제

#### `Map.prototype.clear`

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);
map.clear();
```

### 요소 순회

#### `Map.prototype.forEach`

- `Array.prototype.forEach`와 유사하게 콜백 함수와 함수의 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달.
- 콜백 함수에는 3개의 인수를 전달 받음.

  1. 현재 순회 중인 요소 값.
  2. 현재 순회 중인 요소 키.
  3. 현재 순회 중인 `Map` 객체 자체.

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

- `Map` 객체는 이터러블이므로 `for ... of` 문으로 순회 가능하며, 스프레드 문법과 배열 디스트럭처링의 대상으로 사용 가능.

- `Map` 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드 제공.

|      `Map` 메서드       |                                            설명                                             |
| :---------------------: | :-----------------------------------------------------------------------------------------: |
|  `Map.prototype.keys`   |      `Map` 객체에서 요소 키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환      |
| `Map.prototype.values`  |      `Map` 객체에서 요소 값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환      |
| `Map.prototype.entries` | `Map` 객체에서 요소 키와 요소 값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |

> `Map` 객체는 요소의 순서에 의미가 없지만, 순회하는 순서는 요소가 추가된 순서를 따름.

> 다른 이터러블 순회와 호환성을 유지하기 위함.
