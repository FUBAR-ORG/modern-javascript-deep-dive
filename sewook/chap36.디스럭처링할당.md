# 36장 디스트럭처링 할당

구조 분해 할당(destructuring assignment)이란 객체나 배열을 여러 변수로 분해(unpack) 할 수 있는 특별한 문법이다. 객체나 배열을 함수에 전달할 때 객체나 배열의 전체가 아닌 일부만 필요한 경우 구조 분해 할당을 사용하면 효율적으로 코드를 작성할 수 있다.

<br>

### Array destructuring

배열을 변수로 구조 분해 할당하는 예제이다.

```javascript
// arr은 이름과 성을 가진 배열이다.
let arr = ['John', 'Smith'];

// 변수 `firstName`과 `surName`에 배열 `arr`의 요소를 구조 분해 할당
// sets firstName = arr[0]
// and surname = arr[1]
let [firstName, surname] = arr;

alert(firstName); // John
alert(surname); // Smith

// split을 사용할 경우
let arr2 = 'Lonzo Ball'.split(' '); // ["Lonzo", "Ball"]
let [fN, sN] = arr2;
alert(fN); // Lonzo
alert(sN); // Ball
```

구조 분해 할당 시 쉼표를 사용해 불필요한 배열의 요소를 무시할 수 있다.

```javascript
// 배열의 두 번째 요소가 필요하지 않은 경우
let [firstName, , title] = [
  'Julius',
  'Caesar',
  'Consul',
  'of the Roman Republic',
];

alert(title); // Consul
```

할당이 가능한(assignable) 모든 것들은 `=` 연산자 왼쪽에 위치해 구조 분해 할당을 받는 대상이 될 수 있다.

```javascript
// 변수가 아닌 객체 프로퍼티에 구조 분해 할당
let user = {};
[user.name, user.surname] = 'John Smith'.split(' ');

alert(user.name); // John
alert(user.surname); // Smith
```

일반적으로 배열의 요소가 구조 분해 할당을 받는 대상의 수보다 많을 경우 나머지 항목은 생략된다.

```javascript
let [name1, name2] = ['Julius', 'Caesar', 'Consul', 'of the Roman Republic'];

alert(name1); // Julius
alert(name2); // Caesar
//  "Consul", "of the Roman Republic"는 생략
```

또한 생략되는 요소들을 수집하기 위해서 `...` 연산자를 사용할 수 있다.

```javascript
let [name1, name2, ...title] = [
  'Julius',
  'Caesar',
  'Consul',
  'of the Roman Republic',
];

// rest is array of items, starting from the 3rd one
alert(title[0]); // Consul
alert(title[1]); // of the Roman Republic
alert(title.length); // 2
```

반대로 구조 분해 할당을 받는 대상의 수가 배열의 요소보다 많을 경우 `undefined`로 할당을 받게 된다.

```javascript
let [firstName, surname] = [];

alert(firstName); // undefined
alert(surname); // undefined
```

`undefined` 대신 `=`를 사용해 기본 값으로 대체할 수 있다.

```javascript
let [name = 'Guest', surname = 'Anonymous'] = ['Julius'];

alert(name); // Julius (배열의 요소를 할당)
alert(surname); // Anonymous (기본 값 사용)
```

<br>

### Object destructuring

객체를 구조 분해 할당하는 기본 문법은 아래와 같다.

```javascript
let {var1, var2} = {var1:…, var2:…}
```

객체 구조 분해 할당 시 `=` 연산자의 오른쪽에는 원본 객체가 있어야하며 할당을 받는 대상이되는 왼쪽에는 원본 객체의 프로퍼티와 대응되는 변수가 위치해야 한다.

```javascript
let options = {
  title: 'Menu',
  width: 100,
  height: 200,
};

let { title, width, height } = options;

// let {height, width, title} = { title: "Menu", height: 200, width: 100 } 와 같은 구문으로도 작성할 수 있다.

alert(title); // Menu
alert(width); // 100
alert(height); // 200
```

생략된 프로퍼티의 경우 `=`를 사용해 기본 값을 설정할 수 있다.

```javascript
let options = {
  title: 'Menu',
};

let { width = 100, height = 200, title } = options;

alert(title); // Menu
alert(width); // 100
alert(height); // 200
```

프로퍼티가 많은 복잡한 객체의 경우 필요한 것만 할당하거나 `...`을 사용해 효율적으로 할당할 수 있다.

```javascript
let options = {
  title: 'Menu',
  width: 100,
  height: 200,
};

// title 프로퍼티만 필요한 경우
let { title } = options;

alert(title); // Menu
```

```javascript
let options = {
  title: 'Menu',
  height: 200,
  width: 100,
};

// title 은 options의 title 프로퍼티를 할당받고
// rest는 options의 나머지 프로퍼티 모두를 할당 받게 된다.
let { title, ...rest } = options;

// title="Menu", rest={height: 200, width: 100}
alert(rest.height); // 200
alert(rest.width); // 100
```

<br>

### Nested destructuring

만약 객체나 배열이 다른 객체 및 배열을 포함하는 중첩된 형태일 경우 구조 분해 할당을 위해 좀 더 복잡한 패턴을 사용해야한다.

```javascript
let options = {
  size: {
    width: 100,
    height: 200,
  },
  items: ['Cake', 'Donut'],
  extra: true,
};

let {
  size: { width, height },
  items: [item1, item2],
  title = 'Menu', // 기본 값 사용
} = options;

alert(title); // Menu
alert(width); // 100
alert(height); // 200
alert(item1); // Cake
alert(item2); // Donut

alert(size); // Uncaught ReferenceError: size is not defined
alert(extra); // Uncaught ReferenceError: extra is not defined
```

`option` 객체로부터 `width`, `height`, `item1`, `item2`를 할당받고 `title`은 기본 값을 할당 받았으므로 `size` 및 `items`에 대한 변수는 존재하지 않는다.

<br>

### Smart function prameters

구조 분해 할당을 통해 함수의 파라미터를 객체로 전달하는 예제이다.

```javascript
let options = {
  title: 'My menu',
  items: ['Item1', 'Item2'],
};

// 객체를 파라미터로 받았지만 실제로는 구조 분해 할당을 통해 변수로 분해된다.
function showMenu({
  title = 'Untitled',
  width = 200,
  height = 100,
  items = [],
}) {
  // title, items는 options 객체로부터 할당
  // width, height는 기본 값 할당

  alert(`${title} ${width} ${height}`); // My Menu 200 100

  alert(items); // Item1, Item2
}

showMenu(options);
```

모든 파라미터들에 기본 값을 할당하기 위해서는 전체 파라미터 엔터티의 기본 값을 빈 객체로 설정해야한다.

```javascript
function showMenu({ title = 'Menu', width = 100, height = 200 }) {
  alert(`${title} ${width} ${height}`);
}

showMenu({}); // 모든 파라미터들에 기본 값 할당, Menu 100 200
showMenu(); // error
```

```javascript
// showMenu 함수의 전체 파라미터 엔터티를 {}로 기본 값 설정
function showMenu({ title = 'Menu', width = 100, height = 200 } = {}) {
  alert(`${title} ${width} ${height}`);
}

showMenu({}); // Menu 100 200
showMenu(); // Menu 100 200
```

<br>

<br>

---

**Reference**

- [Destructuring assignment](https://javascript.info/destructuring-assignment)
