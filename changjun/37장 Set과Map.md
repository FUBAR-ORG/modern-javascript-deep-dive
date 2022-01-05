# 37장. Set과 Map

# Set

Set 객체는 배열과 유사하지만 배열과 다른 점이 있다.

- 동일한 값을 중복하여 포함할 수 없다.
- 요소 순서에 의미가 없다.
- 인덱스로 요소에 접근할 수 없다.

## Set객체의 생성

```js
const set = new Set();
console.log(set); // Set(0){};
```

```js
//1. Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}
```

```js
const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
//2. 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.
```

```js
// 3. 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

```js
// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

Set객체는 Set 생성자 함수로 생성하고, 이터러블을 인수로 전달받아 Set객체를 생성한다. 이 때 이터러블의 중복된 값은 Set요소에 저장되지 않는다.

## Set 요소 개수 확인

- **Set.prototype.size**

```js
const { size } = new Set([1, 2, 3]);
console.log(size); //3
```

## Set 요소 추가

- **Set.prototype.add**

```js
const set = new Set();
set.add(1); // Set(1) {1}
set.add(2).add(3); // Set(3) {1, 2, 3}  //method chaining
```

```js
const set = new Set();
console.log(NaN === NaN); //false;
console.log(0 === -0); // true;

//NaN 과 NaN을 같다고 판단하고 중복 허용 하지 않는다.
set.add(NaN);
```

- add 메서드는 새로운 요소가 추가된 Set객체를 반환하므로 연속적으로 호출 가능
- 중복 요소는 추가되지 않으나 에러 발생하지 않음
- 객체나 배열과 같은 `JavaScript`의 모든 값을 요소로 저장 가능
- NaN과 NaN은 다른데 Set은 이를 같다고 평가하여 중복 추가 하지 않음

## Set 요소 존재 여부 확인

- **Set.prototype.has**
- 반환값 : boolean

```js
const set = new Set([1, 2, 3]);
console.log(set.has(2)); // true;
console.log(set.has(4)); //false;
```

## Set 요소 삭제

- **Set.prototype.delete**
- 반환값: boolean

```js
const set = new Set([1, 2, 3]);
set.delete(2); // Set(2)//{1,3}

//존재하지 않는 요소에 대한 삭제는 무시
set.delete(0);

//delete는 불리언 값을 반환하므로 method chaining이 불가능
set.delete(1).delete(2);
```

## Set 요소 일괄 삭제

- **Set.prototype.clear**

```js
const set = new Set([1, 2, 3]);
set.clear();
console.log(set); // Set(0){}
```

## Set 요소 순회

```js
const set = new Set([1, 2, 3]);
set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

# Map

- Map은 키와 값의 쌍으로 이루어진 자료구조
- 객체와 유사하지만 차이점이 있다.

|        **구분**        |        **객체**         |        **Map**        |
| :--------------------: | :---------------------: | :-------------------: |
| 키로 사용할 수 있는 값 |       문자열,심벌       | 객체를 포함한 모든 값 |
|        이터러블        |            X            |           O           |
|     요소 갯수 확인     | Object.keys(obj).length |       map.size        |

## Map 객체 생성

```js
const map1 = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

- `Map` 생성자 함수로 생성
- 생성자 함수는 이터러블을 인수로 받아 `Map 객체` 생성
- 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성
- 중복된 키를 갖는 요소가 존재하면 값이 덮어 쓰기가 됨.

```js
const map = new Map([
  ['key1', 'value1'],
  ['key1', 'value2'],
]);
console.log(map); // Map(1) {"key1" => "value2"} 덮어씜
```

## Map 요소 갯수 확인

- **Map.prototype.size**

```js
const { size } = new Map(['key1', 'value1'], ['key2', 'value2']);
console.log(size); //2
```

size 프로퍼티는 getter만 존재 즉 size에 값을 할당하면 무시된다.

## Map 요소 추가

- **Map.prototype.set**

```js
const map = new Map();
console.log(map); // Map(0) {}

//method chaining 가능
map.set('key1', 'value1').set('key2', 'value2');
console.log(map); // Map(1) {"key1" => "value1" , "key2"=>"value2"}
```

- 중복된 요소는 그 값이 덮어 씌워짐

```js
const map = new Map();

map.set('key1', 'value1').set('key1', 'value2');

console.log(map); // Map(1) {"key1" => "value2"}
```

- NaN과 NaN을 같다고 평가하고 중복 추가를 허용하지 않음

```js
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, 'value1').set(NaN, 'value2');
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, 'value1').set(-0, 'value2');
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```

- Map 객체는 키 타입에 제한이 없음 따라서 객체를 포함한 모든 값을 키로 사용 가능

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map.set(lee, 'developer').set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```

## Map 요소 가져오기

- **Map.prototype.get**
- 인수 : 키를 전달
- 반환 : 인수로 전달받은 키를 갖는 값, 없으면 undefined

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

## Map 요소 존재 여부 확인

- **Map.prototype.has**

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

## Map 요소 삭제

- **Map.prototype.delete**

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

## Map 요소 전체 삭제

- **Map.prototype.clear**

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.clear();
console.log(map); // Map(0) {}
```

## Map 요소 순회

- **Map.prototype.forEach**

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
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

- Map 객체는 이터러블 이므로 `for .. of` 문으로 순회 가능
- Map 객체는 이터러블 이므로 스프레드문법, 비구조화 할당이 가능

## Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.
