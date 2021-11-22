# React Portals

## Portal

> 포탈(Portal)은 `현관` `관문` `현관문`의 뜻을 가진 말이다.

포탈이란 용어는 게임이나 영화에서 많이 등장한다. 메이플스토리에서 캐릭터가 맵을 이동할 때 포탈을 이용하거나 마블 캐릭터인 닥터스트레인지가 공간을 이동할 때 마법 포탈을 열어 이동하는것 처럼

## React에서 Portal

위의 사전적 의미를 톺아보며 그럼 React에서도 뭘 이동시킬 수 있는 건가? 라는 생각을 하게 되었다.

Portals는 리액트 프로젝트에서 컴포넌트를 렌더링 할 때 UI를 어디에 렌더링 시킬 지 DOM을 사전에 선택하여 부모 컴포넌트에 종속되지 않게 렌더링 할 수 있는 기능이다.

## 기존의 흐름

기존의 리액트에서 컴포넌트를 렌더링할 때 children은 부모 컴포넌트의 `DOM 내부`에 렌더링되었다.

리액트 프로젝트에서 엔트리 파일인 src/index.js를 보면 아래와 같은 코드가 있다.

```js
ReactDOM.render(<App />, document.getElementById('root'));
```

이 코드는 id값이 `root`인 DOM을 찾고 거기에 `App`컴포넌트를 렌더링하는 소스다.

`root`는 public/index.html에서 찾아볼 수 있다.

## Portal을 활용해 컴포넌트 만들기

하지만 `Portal`를 사용하면 부모 컴포넌트에 종속되지 않고 독립적으로 컴포넌트를 렌더링 할 수 있다.

portal의 구조

```js
ReactDOM.createPortal(child, container);
```

- 첫 번째 인자(child)는 엘리먼트, 문자열, 혹은 fragment와 같은 어떤종류의 react 자식
- 두 번째 인자(container)는 DOM 엘리먼트이다.

#### 1. 먼저 엔트리 파일에 src/ModalPortal.jsx를 만들자.

```js
//src/ModalPortal.jsx;

import ReactDom from 'react-dom';

const ModalPortals = ({ children }) => {
  const el = document.getElementById('modal');
  return ReactDom.createPortal(children, el);
};

export default ModalPortals;
```

#### 2. public/index.html파일에 id가 modal인 dom 추가

```html
<body>
  <noscript> You need to enable JavaScript to run this app. </noscript>
  <div id="root"></div>
  <div id="modal"></div>
</body>
```

#### 3. 렌더링 할 모달 만들기

```js
//src/Modal.js
import React from 'react';

const Modal = () => {
  return (
    <div className="Modal">
      <h1>모달 컴포넌트</h1>
    </div>
  );
};

export default Modal;
```

#### 4. App에서 렌더링하기

```js
import React, { Component } from 'react';
import ModalPortal from './MordalPortal';
import Modal from './Modal';
import './App.css';

const App = () => {
  return (
    <>
    <div className="App">
    </div>
    <ModalPortal><Modal/></MordalPortal>
    </>
  );
};

export default App;
```

아까 만들었던 ModalPortal 컴포넌트를 App에서 렌더링 하고 children으로 렌더링할 컴포넌트를 넣어주면 된다.

그러면 root에 종속되지 않고 modal이라는 id를 가진 DOM에 렌더링 된다.

## Portal의 장점

경고창, alert창, 모달 등과 같이 전역에서 쓰이는 UI의 우선순위를 결정하기에 편한것 같다.
