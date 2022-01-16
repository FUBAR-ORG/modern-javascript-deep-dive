# 39장. DOM

## DOM이란?

- Dodument Object Model은 HTML문서의 계층적 구조와 정보 그리고 여러 메소드들로 이루어진 트리 자료구조
- 노드 객체들로 구성된 트리 자료구조를 DOM이라 부르며 DOM 트리라고도 한다.

## 노드

### HTML요소와 노드객체

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
- HTML 요소는 시작 및 종료 태그, 어트리뷰트, 콘텐츠로 이루어진다.
- 서로 중첩관계를 갖고 이로인해 계층적인 부자관계가 형성된다.

## 노드의 종류

HTML 요소는 렌더링 엔진을 통해 DOM을 구성하는 `요소 노드`객체로 변환된다.이때 HTML 요소의 어트리뷰트는 `어트리뷰트 노드`로 텍스트 콘텐츠는 `텍스트 노드`로 변환된다.

## 노드 객체 타입

- 문서 노드

  - DOM 트리 최상위 노드이며 document 객체를 가리킨다.
  - window.document로 참조할 수 있으며 HTML문서당 하나의 유일한 객체이다.

- 요소 노드

  - HTML 요소를 가리키는 객체이다.
  - 중첩에 의해 계층적인 부자관계를 가지고, 문서의 구조를 표현한다.

- 어트리뷰트 노드

  - HTML 요소의 어트리뷰트를 가리키는 객체이며 어트리뷰트가 지정된 요소 노드와 연결되어있다.
  - 어트리뷰트를 참조하거나 변경하려면 요소 노드에 접근해야 한다.

- 텍스트 노드
  - HTML 요소의 텍스트를 가리키는 객체이다.
  - 문서의 정보를 표현한다.
  - 요소 노드의 자식이자 DOM 트리의 리프노드이다.

## 노드 객체의 상속 구조

노드 객체는 ECMAScript에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 제공하는 호스트 객체이다. 하지만 노드 객체도 자바스크립트 객체이기 때문에 `Object`를 상속받는다.

![상속구조](./asset/노드상속.png)

모든 노드 객체는 `Object`, `EventTarget`, `Node`인터페이스를 상속받는다.

input과 같은 요소는 다음과 같은 구조로 되어있다.

Object -> EventTarget -> Node -> Element -> HTMLElement -> HTMLInputElement

# 요소 노드 취득 - DOM API

### id를 이용한 요소 노드 취득

- **Document.prototype.getElementById**

id는 HTML문서 내에서 유일한 값이여야 하지만 중복되는 id값이 있을 경우 첫 번째 요소만 반환한다.

### 태그를 이용한 요소 노드 취득

- **Document.prototype.getElementByTagName**
- **Element.prototype.getElementByTagName**

여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.  
이 객체는 유사 배열 객체이면서 이터러블이다.

위 둘의 차이는 DOM 전체에서 탐색하느냐 특정 요소 노드의 자식중에 탐색하느냐 차이다.

### class를 이용한 노드 취득

- **Document.prototype.getElementByClassName**
- **Element.prototype.getElementByClassName**

### CSS 선택자를 이용한 노드 취득

- **Document.prototype.querySelectorAll**
- **Element.prototype.querySelectorAll**

### 특정 요소를 취득할 수 있는지 확인

- **Element.prototype.matches**

css 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인

### HTMLCollection

- DOM API가 여러 개의 결과 값을 반환하기 위한 DOM 컬렉션 객체이다.
- 유사 배열 객체, 이터러블 이다.
- 노드 객체의 상태 변화를 실시간으로 반영하는 **살아있는 객체**이다.

```js
const $elem = document.getElementsByClassName('red');

console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

// HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경한다.
for (let i = 0; i < $elems.length; i++) {
  $elems[i].className = 'blue';
}

console.log($elmes); // HTMLCollection(1) [li.red]
```

모든 요소가 `blue`로 변할것 같지만 실시간으로 반영하기 때문에 첫 번째 반복때 elems[0]이 blue로 변경되면서 그 다음요소는 3번째 요소를 가리키기 때문이다.

해결 방법은 for문을 역으로 돌리기, while문으로 반복하기

### NodeList

HTMLCollection 객체의 부작용을 해결하기 위해 querySelectorAll 메소드를 사용하여 NodeList 객체를 반환하게 만든다.

- NodeList 객체는 실시간으로 노드 객체의 상태 변화를 반영하지 않는다.
- childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요하다.
- 안전하게 사용하려면 배열로 변환해서 사용하는 것을 권장한다.

# 노드 탐색

- Node.prototype은 parentNode, previousSibling, firstChild,childNodes 제공
- Element.prototype은 previousElementSibling, nextElementSibling, children 제공

노드 탐색 프로퍼티는 모두 접근자 프로퍼티며 setter없이 getter만 존재한다.

## 공백 텍스트 노드

HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성하는데 이를 공백 텍스트 노드라고 한다.

## 자식 노드 탐색

- Node.prototype.childNodes
- Node.prototype.firstChild
- Node.prototype.lastChild
- Element.prototype.children
- Element.prototype.firstElementChild
- Element.prototype.lastElementChild

NodeList는 요소 뿐만 아니라 텍스트 노드도 포함되어 있지만 Element는 텍스트 노드가 포함되지 않음.

## 부모 노드 탐색

- Node.prototype.parentNode

## 형제 노드 탐색

- Node.prototype.previousSibling
- Node.prototype.nextSibling
- Element.prototype.previousElementSibling
- Element.prototype.nextElementSibling

## 노드 정보 취득

- Node.prototype.nodeType
  - 노드의 타임을 나타내는 상수 반환
- Node.prototype.nodeName
  - 노드의 이름을 문자열로 반환


# 요소 노드의 텍스트 조작

## nodeValue
- Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 가지고 있다.
- 노드 객체이 nodeValu 프로퍼티를 참조하면 노드 객체의 값을 반환한다. (노드의 텍스트)
- 문서 노드나 요소 노드를 참조하면 null을 반환
- nodeValue에 값을 할당하면 텍스트 노드의 값, 즉 텍스트를 변경 가능

## textContent

- Node.prototype.textContent
- setter, getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 반환
- 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역 내의 텍스트를 모두 반환

# DOM 조작

DOM 조작에 의해 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하여 성능에 영향을 준다.

## innterHTML
- Element.prototype.innerHTML
- setter, getter 모두 존재

```js
const $text = document.getElementById('text');
// 노드 추가
$text.innerHTML += '<li class="text">text</li>';
// 노드 교체
$text.innerHTML = '<li class="text2">text2</li>';
// 노드 삭제
$text.innerHTML = '';
```

## insertAdjacentHTML
- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입할 수 있다.
- 두 번째 인수는 HTML 마크업 문자를 파싱하고 첫 번째 인수로 입력한 위치에 삽입해 DOM에 반영한다.
- 기존 요소에 영향을 주지 않아 innerHTML보다 빠르다.

```js
const $text = document.getElementById('text');
$text.insertAdjacentHTML('hello','<p>hello<p>');
```

## 노드 생성과 추가

### 1. 요소 노드 생성

- Document.prototype.createElement 메서드는 요소 노드를 생성해 반환

```js
const $li = document.createElement('li');
```

### 2. 텍스트 노드 생성

- Document.prototype.createTextNode 메서드는 텍스트 노드를 생성해 반환

```js
const textNode  document.createTextNode('text');
```

### 3. 노드의 생성과 추가
- Node.prototype.appendChild 메서드는 노드의 마지막 자식으로 요소를 추가

```js
const $li = document.createElement('li');
const textNode = document.createTextNode('text');
$li.appendChild(textNode)
```

### 4. 복수의 노드 생성과 추가

```html
  <script>
    const $fruits = document.getElementById('fruits');

    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
      $fragment.appendChild($li);
    });

    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
  </script>
```

여러 개의 요소 노드를 DOM에 추가하는 경우 `DocumentFragment` 노드를 사용하는 것이 더 효율적이다.
(여러번의 리페인트와 리플로우를 막기위해)

## 노드 삽입
- Node.prototype.appendChild : 인수로 전달 받은 노드를 마지막 자식노드로 추가
- Node.prototype.inserBefore: 첫 번째 인수로 전달받은 노드를 두번 째 인수로 노드 앞에 삽입.

## 노드 이동

- DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다. 

## 노드 복사
- Node.protoype.cloneNode 메서드는 노드의 사본을 생성하여 반환한다.

## 노드 교체

- Node.prototype.replaceChild 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.

## 노드 삭제

- Node.prototype.removeChild 메서드는 인수로 전달한 노드를 DOM에서 삭제한다.

# 어트리뷰트

## 어트리뷰트 노드와 attributes 프로퍼티

- HTML 문서의 구성 요소인 html 요소는 여러 개의 어트리뷰트를 가질 수 있다.
- HTML 요소의 동작을 제어하기 위한 추가적인 정보를 제공한다.
- 대부분의 어트리뷰트는 모든 html 요소에서 사용될 수 있지만, type, value, checked 어티르뷰트는 input 요소에서만 사용할 수 있다.
- HTML 문서가 파싱될 때 html 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연걸된다.

## 어트리뷰트 조작

- HTML 어트리뷰트 값을 참조하려면 `Element.prototype.getAttribute` 메서드를 사용하고, HTML 어트리뷰트 값을 변경하려면 `Element.prototype.setAttribute`메서드를 사용한다.

```html
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트 값을 취득
    const inputValue = $input.getAttribute('value');
    console.log(inputValue); // ungmo2

    // value 어트리뷰트 값을 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo
  </script>
</body>
</html>
```
