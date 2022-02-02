- Quiz_1

  ```js
  async function sleep() {
    console.log("sleep");
  }
  async function foo() {
    await sleep();
    console.log("foo");
  }
  async function bar() {
    console.log("bar");
  }
  async function baz() {
    console.log("temp");
    foo();
    console.log("baz");
    await bar();
    console.log("test");
  }
  baz();
  ```

  - 프로미스로 변경

  ```js
  function sleep() {
    // console.log("sleep");
    return new Promise((resolve, reject) => {
      console.log("sleep");
      resolve();
    });
  }
  function foo() {
    return new Promise((resolve, reject) => {
      sleep().then(() => {
        console.log("foo");
        resolve();
      });
    });
  }
  function bar() {
    // console.log("bar");
    return new Promise((resolve, reject) => {
      console.log("bar");
      resolve();
    });
  }
  function baz() {
    // console.log("temp");
    // foo();
    // console.log("baz");
    return new Promise((resolve, reject) => {
      console.log("temp");
      foo();
      console.log("baz");
      bar().then(() => {
        console.log("test");
        resolve();
      });
    });
  }
  baz();
  ```

  - 실행 결과

  ```shell
  temp
  sleep
  baz
  bar
  foo
  test
  ```

- Quiz_2

  ```js
  async function sleep() {
    console.log("sleep");
  }
  async function foo() {
    await sleep();
    console.log("foo");
  }
  async function bar() {
    console.log("bar");
  }
  async function baz() {
    console.log("temp");
    const f = foo();
    console.log("baz");
    await f;
    await bar();
    console.log("test");
  }
  baz();
  ```

  - 프로미스로 변경

  ```js
  function sleep() {
    return new Promise((resolve, reject) => {
      console.log("sleep");
      resolve();
    });
  }
  function foo() {
    return new Promise((resolve, reject) => {
      sleep().then(() => {
        console.log("foo");
        resolve();
      });
    });
  }
  function bar() {
    return new Promise((resolve, reject) => {
      console.log("bar");
      resolve();
    });
  }
  function baz() {
    return new Promise((resolve, reject) => {
      console.log("temp");
      const f = foo();
      console.log("baz");
      f.then(() => {
        bar().then(() => {
          console.log("test");
          resolve();
        });
      });
    });
  }
  baz();
  ```

  - 실행 결과

  ```shell
  temp
  sleep
  baz
  foo
  bar
  test
  ```
