# 46장. 제네레이터와 async/await

제네레이터를 사용해 비동기 처리를 동기처럼 동작하도록 구현했지만 코드가 장황해지고 가독성이 안좋아졌다. 이를 해결하기 위해 ES8에 async/await 문법이 도입되었다.

## 46.6 async/await

- 제네레이터보다 가독성이 좋다.
- 프로미스를 기반으로 동작한다.
- 프로미스의 then/catch/finally 후속처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요없이 마치 동기처럼 사용할 수 있다.

```js
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

### 46.6.1 async 함수

- await 키워드는 반드시 async 내부에서 사용해야 한다.
- 언제나 프로미스를 반환한다.
- 암묵적으로 반환값을 resolve로 하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) {
    return n;
  }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```

클래스의 constructor 메서드는 async 메서드가 될 수 없다. 그 이유는 constructor는 인스턴스를 반환해야하지만 async는 언제나 프로미스를 반환하기 때문이다.

### 46.6.2 await

- await는 프로미스가 settled상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await는 반드시 프로미스 앞에 사용해야 한다.

```js
const fetch = require('node-fetch');

const getGithubUserName = async id => {
  const res = await fetch(`https://api.github.com/users/${id}`); // (1)
  const { name } = await res.json(); // (2)
  console.log(name); // Ungmo Lee
};

getGithubUserName('ungmo2');
```

위의 코드에서 (1)의 fetch 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될 때까지 (1)은 대기하게 된다.

이후 프로미스가 setteld 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.

```js
async function foo() {
  const a = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
  const b = await new Promise(resolve => setTimeout(() => resolve(2), 2000));
  const c = await new Promise(resolve => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```

모든 프로미스에 await 키워드를 사용하는 것은 주의해야 한다. 위 코드는 foo 함수는 종료될 때 까지 6초가 소요되는데 **서로 연관이 없이 개별적으로 수행되는 비동기 처리이므로** 순차적으로 처리할 필요가 없다.

```js
async function foo() {
  const res = await Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)),
    new Promise(resolve => setTimeout(() => resolve(2), 2000)),
    new Promise(resolve => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요된다.
```

아래 코드의 bar 함수는 앞선 비동기 처리의 결과를 가지고 다음 비동기 처리를 수행해야한다.

따라서 비동기 처리의 처리 순서가 보장되어야하므로 모든 promise에 await 키워드를 써서 순차적으로 처리할 수 밖에없다.

```js
async function bar(n) {
  const a = await new Promise(resolve => setTimeout(() => resolve(n), 3000));
  // 두 번째 비동기 처리를 수행하려면 첫 번째 비동기 처리 결과가 필요하다.
  const b = await new Promise(resolve =>
    setTimeout(() => resolve(a + 1), 2000)
  );
  // 세 번째 비동기 처리를 수행하려면 두 번째 비동기 처리 결과가 필요하다.
  const c = await new Promise(resolve =>
    setTimeout(() => resolve(b + 1), 1000)
  );

  console.log([a, b, c]); // [1, 2, 3]
}

bar(1); // 약 6초 소요된다.
```

### 46.6.3 에러처리

비동기 처리를 위한 콜백 패턴의 단점중 가장 심각한 것은 에러처리가 곤란하다는 것이다.

비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니기 떄문에 try catch 문으로 에러를 캐치할 수 없었다.

하지만 async/await은 try...catch 문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 **명시적으로 호출할 수 있기 때문에 호출자가 명확하다**
