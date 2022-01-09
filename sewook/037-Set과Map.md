# 36장 Set과 Map

## Set

Set 객체는 수학접 집합을 구현하기 위한 자료구조다.

- 중복을 포함할 수 없다.
- 요소 순서에 의미가 없다.
- 인덱스로 요소에 접근할 수 없다.
- 자바스크립트의 모든 값을 요소로 저장할 수 있다.
- 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
- Set 객체는 이터러블이다.

```js
// #1 생성: Set 생성자 함수
const arr = [1, 2, 3, 3, 3];
const set = new Set(arr); // Set(3) {1, 2, 3}

// 중복 제거된 배열 생성
const uniq = [...new Set(arr)]; // [1, 2, 3]

// #2 요소 개수 확인
// getter 함수만 존재하는 접근자 프로퍼티기 때문에 객체 요소이 변경 불가하다.
set.size; // 3

// #3 요소 추가
// 중복 요소를 추가하면 무시된다.
set.add(4);
set; // Set(4) {1, 2, 3, 4}

// #4 요소 존재 여부 확인
set.has(1); // true

// #5 요소 삭제
set.delete(1);
set; // Set(3) {2,3,4}

// 요소

// #6 요소 일괄 삭제
set.clear();
set; // Set() {size: 0}

// forEach로 순회
// 인덱스가 없어 v1, v2가 같은 값을 가진다.
set.add(1).add(2);
set.forEach((v1, v2, set) => console.log(v1, v2, set));
/*
1 1 Set(2) {1,2}
2 2 Set(2) {1,2}
*/
```

Set 객체는 수학적 집합을 구현하기 위한 자료구조다.

```js
// #1. 교집합
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};

// #2. 합집합
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

// #3. 차집합
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

// #4. 부분 집합과 상위 집합
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};
```

<br>

# Map

Map 객체는 키와 값으로 이루어진 컬렉션이다.

- 객체를 포함한 모든 값을 키로 사용할 수 있다.
- 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.
- Map 객체는 이터러블이다.
- 중복된 키를 갖는 요소가 존재할 수 없다.

```js
// #1 생성
const map = new Map([
  ['k1', 'v1'],
  ['k2', 'v2'],
]);

// #2 요소 추가
map.set('k3', 'v3');

// #3 요소 취득
map.get('k3'); // v3

// #4 요소 존재 여부 확인
map.has();

// #5 요소 삭제
map.delete('k3');

// #6 forEach로 순회
map.forEach((v, k, map) => console.log(v, k, map));
/*
  v1 k1 Map(2) ...
  v2 k2 Map(2) ...
*/

// #6 일괄 삭제
map.clear();

// #7 Map 객체는 이터러블이면서 이터레이터인 객체를 반환하는 메서드를 제공한다.
map.keys(); // MapIterator {'k1', 'k2'}
map.values(); // MapIterator {'v1', 'v2'}
map.entries(); // apIterator {'k1' => 'v1', 'k2' => 'v2'}
```
