# `DOM`

- `DOM`(`Document Object Model`)은 `HTML` 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 `API`. 즉, 프로퍼티와 메서드를 제공하는 트리 자료구조.

> 브라우저가 이해할 수 있는 자료구조.

## 노드

### `HTML` 요소와 노드 객체

- `HTML` 요소(`HTML element`)는 `HTML` 문서를 구성하는 개별적인 요소.
- 렌더링 엔진에 의해 파싱되어 `DOM`을 구성하는 요소 노드 객체로 변환.
- 요소의 어트리뷰트는 어트리뷰트 노드, 텍스트 콘텐츠는 텍스트 노드.

> `HTML` 요소는 `HTML` 요소들의 집합으로 구성되며, 중첩 관계를 가져 계층적인 부자 관계. 즉, 트리 관계 자료구조를 구성.

> 노드 객체들로 구성된 트리 자료구조를 `DOM`이라 부르며, `DOM` 트리라고 부르기도 함.

### 노드 객체 타입

- `DOM`은 노드 객체는 계층적인 구조로 구성.
- 노드 객체는 총 12개의 종류(타입)가 있으며 상속 구조를 가짐.
- 중요한 노드 타입

  - 문서 노드(`document node`)

    - `DOM` 트리 최상단에 존재하는 루트 노드.
    - `document` 객체를 가리킴.
    - 브라우저가 렌더링한 `HTML` 문서 전체를 가리키는 객체로서 전역 객체 `window`의 `document` 프로퍼티에 바인딩 되어 있음.
    - 루트 노드이므로 `DOM` 트리의 노드들에 접근하기 위한 진입점(`entry point`) 역할.
    - 모든 `JavaScript` 코드는 `script`로 분리되어 있어도 하나의 전역 객체 `window`를 공유.

    > `HTML` 문서 당 `document` 객체는 유일.

  - 요소 노드(`element node`)

    - `HTML` 요소를 가리키는 객체.

    > 문서의 구조를 표현.

  - 어트리뷰트 노드(`attribute node`)

    - 어트리뷰트를 가리키는 객체.
    - 어트리뷰트가 지정된 `HTML` 요소의 요소 노드와 연결되어 있음.

    > 요소 노드에 접근해야만 해당 요소 노드의 어트리뷰트 노드 접근 가능.

  - 텍스트 노드(`text node`)

    - 텍스트를 가리키는 객체.
    - 요소 노드의 자식 노드이며 리프노드.

    > 문서의 정보를 표현.

- 그 외에 `Comment`, `DocumentType`, `DocumentFragment` 등이 있음.

### 노드 객체의 상속 구조

- `DOM API`를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작.
- 노드 객체는 호스트 객체.
- `JavaScript` 객체이므로 프로토타입에 의한 상속 구조를 가짐.
- 모든 노드 객체는 `Object`, `EventTarget`, `Node` 인터페이스를 상속.
- 문서 노드는 `Document`, `HTMLDocument`를 상속, 어트리뷰트 노드는 `Attr`, 텍스트 노드는 `CharacterData` 인터페이스를 상속 받음.
- 요소 노드는 `Element` 인터페이스 상속 받으며, `HTMLElement`와 태그의 종류별로 세분화된 `HTMLHeadElement`, `HTMLBodyElement`, `HTMLUListElement` 등의 인터페이스를 상속 받음.
- `input` 요소 노드는 `HTMLInputElement`, `HTMLElement`, `Element`, `Node`, `EventTarget`, `Object`의 `prototype`에 바인딩되어 있으므로 모든 프로퍼티나 메서드를 상속 받아 사용 가능.

|                          `input` 요소 노드 객체의 특성                           | 프로토타입을 제공하는 객체 |
| :------------------------------------------------------------------------------: | :------------------------: |
|                                       객체                                       |          `Object`          |
|                             이벤트를 발생시키는 객체                             |       `EventTarget`        |
|                            트리 자료구조의 노드 객체                             |           `Node`           |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(`HTML`, `XML`, `SVG`)를 표현하는 객체 |         `Element`          |
|                웹 문서의 요소 중에서 `HTML` 요소를 표현하는 객체                 |       `HTMLElement`        |
|                 `HTML` 요소 중에서 `input` 요소를 표현하는 객체                  |     `HTMLInputElement`     |

> 노드 객체 상속 구조는 개발자 도구의 `Elements` 패널 우측의 `Properties` 패널에서 확인 가능.

- `EventTarget` 인터페이스는 이벤트에 관련된 기능(`EventTarget.addEventListener`, `Event.removeEventListener` 등)을 제공하며, 모든 노드 객체에서 사용 가능.
- `Node` 인터페이스는 트리 탐색 기능(`Node.parentNode`, `Node.childNodes`, `Node.previousSibling`, `Node.nextSibling` 등), 노드 정보 제공 기능(`Node.nodeType`, `Node.nodeName` 등)을 제공.
- `HTMLElement` 인터페이스는 `HTML` 요소가 갖는 공통적인 기능(`style` 프로퍼티 등)을 제공.
- `HTMLInputElement` 요소는 `input` 프로퍼티에만 필요한 `value`를 제공하며, 이는 `HTML` 요소 종류에 따라 각각 다름.

> `DOM`은 `HTML` 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류. 즉, 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 `DOM API`로 제공.

> 이 `DOM API`를 통해 `HTML`의 구조나 내용 또는 스타일 등을 동적으로 조작 가능.

---

## 요소 노드 취득

- `HTML`의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 함.

### `id`를 이용한 요소 노드 취득

- `Document.prototype.getElementById` 메서드
  - 인수로 전달한 `id` 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환.
  - `Document.prototype`의 프로퍼티이므로 반드시 `document`를 통해 호출.
  - `id` 값은 문서 내 유일한 값이어야 하지만 여러 개 존재하더라도 에러가 발생하지 않음.
  - `class` 어트리뷰트와 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없음.
  - 여러 개일 경우 첫 번째 요소 노드만 반환.
  - 인수로 전달된 `id` 어트리뷰트 값을 갖는 요소가 존재하지 않으면 `null` 반환.
- `HTML` 요소에 `id` 어트리뷰트를 부여하면 `id` 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수효과.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo"></div>
      <script>
        console.log(foo === document.getElementById("foo"));

        // 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역변수는 삭제되지 않음.
        delete foo;
        console.log(foo); // <div id="foo"></div>
      </script>
    </body>
  </html>
  ```

- `id` 값과 동일한 이름의 전역 변수가 이미 선언되어 있으면 부수효과 실행되지 않음.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo"></div>
      <script>
        let foo = 1;

        console.log(foo); // 1
      </script>
    </body>
  </html>
  ```

### 태그 이름을 이용한 요소 노드 취득

- `Document.prototype.getElementsByTagName`/`Element.prototype.getElementsByTagName` 메서드
  - 인수로 전달한 태그 이름은 갖는 모든 요소 노드들을 탐색하여 반환.
  - 여러 개의 요소 노드 객체를 갖는 `DOM` 컬렉션 객체인 `HTMLCollection` 객체를 반환.
  - 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않으면 빈 `HTMLCollection` 객체를 반환.
  - `HTMLCollection`은 유사 배열 객체이면서 이터러블.
  - `HTML` 문서의 모든 요소 노드를 취득하려면 인수로 `*`을 전달.
  - `Document.prototype`은 루트 노드에서부터 탐색, `Element.prototype`은 특정 요소 노드의 자손 노드를 탐색.

### `class`를 이용한 요소 노드 취득

- `Document.prototype.getElementsByClassName`/`Element.prototype.getElementsByClassName` 메서드
  - 인수로 전달한 `class` 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환.
  - 인수로 전달할 `class` 값은 공백으로 구분하여 여러 개의 `class` 지정 가능.
  - 여러 개의 요소 노드 객체를 갖는 `DOM` 컬렉션 객체인 `HTMLCollection` 객체를 반환.
  - 인수로 전달된 `class` 어트리뷰트 값을 갖는 요소가 존재하지 않으면 빈 `HTMLCollection` 객체를 반환.
  - `HTML` 문서의 모든 요소 노드를 취득하려면 인수로 `*`을 전달.
  - `Document.prototype`은 루트 노드에서부터 탐색, `Element.prototype`은 특정 요소 노드의 자손 노드를 탐색.

### `CSS` 선택자를 이용한 요소 노드 취득

> `CSS` 선택자(`selector`)는 스타일을 적용하고자 하는 `HTML` 요소를 특정할 때 사용하는 문법.

```css
/* 전체 선택자: 모든 요소를 선택 */
* {
  ...;
}
/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p {
  ...;
}
/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo {
  ...;
}
/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo {
  ...;
}
/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type="text"] {
  ...;
}
/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p {
  ...;
}
/* 자식 선택자: p 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p {
  ...;
}
/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul {
  ...;
}
/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul {
  ...;
}
/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover {
  ...;
}
/* 가상 요소 선택자: p 요소의 콘텐츠 앞에 위치하는 공간을 선택
  일반적으로 content 프로퍼티와 함께 사용 */
p::before {
  ...;
}
```

- `Document.prototype.querySelector`/`Element.prototype.querySelector` 메서드

  - 인수로 전달한 `CSS` 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환.
  - 인수로 전달한 `CSS` 선택자를 만족시키는 요소 노드가 존재하지 않으면 `null` 반환.
  - 인수로 전달한 `CSS` 선택자가 문법에 맞지 않는 경우 `DOMException` 에러 발생.
  - `Document.prototype`은 루트 노드에서부터 탐색, `Element.prototype`은 특정 요소 노드의 자손 노드를 탐색.

- `Document.prototype.querySelectorAll`/`Element.prototype.querySelectorAll` 메서드
  - 인수로 전달한 `CSS` 선택자를 만족시키는 모든 요소 노드를 탐색하여 `DOM` 컬렉션 객체인 `NodeList` 객체를 반환.
  - `NodeList`은 유사 배열 객체이면서 이터러블.
  - 인수로 전달한 `CSS` 선택자를 만족시키는 요소 노드가 존재하지 않으면 빈 `NodeList` 반환.
  - 인수로 전달한 `CSS` 선택자가 문법에 맞지 않는 경우 `DOMException` 에러 발생.
  - `HTML` 문서의 모든 요소 노드를 취득하려면 인수로 전체 선택자 `*`을 전달.
  - `Document.prototype`은 루트 노드에서부터 탐색, `Element.prototype`은 특정 요소 노드의 자손 노드를 탐색.

> `CSS` 선택자 문법을 사용하는 메서드는 `getElement~` 메서드보다 다소 느린 것으로 알려짐.  
> 하지만 `CSS` 선택자를 통해 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고, 일관된 방식으로 요소 노드를 취득할 수 있다는 장점.  
> 따라서 `id` 어트리뷰트가 있는 요소 노드를 취득하는 경우에는 `getElementById`를 사용하고, 그 외에는 `CSS` 선택자 문법을 사용하는 것을 권장.

### 특정 요소 노드를 취득할 수 있는지 확인

- `Element.prototype.matches` 메서드

  - 인수로 전달한 `CSS` 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인.

  > `ELement.prototype.matches` 메서드는 이벤트 위임을 사용할 때 유용.

### `HTMLCollection`과 `NodeList`

- `DOM` 컬렉션 객체인 `HTMLCollection`과 `NodeList`는 `DOM API`가 여러 개의 결과 값을 반환하기 위한 `DOM` 컬렉션 객체.
- 두 가지 모두 유사 배열 객체이면서 이터러블.
- 따라서 `for ... of` 문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환 가능.
- 두 가지 객체의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 객체.
- `HTMLCollection`은 언제나 `live` 객체로 동작.
- `NodeList`는 대부분의 경우 노드 객체의 상태를 실시간으로 반영하지 않고 과거 정적 상태를 유지하는 `non-live` 객체로 동작하지만, 경우에 따라 `live`로 동작 가능.

#### `HTMLCollection`

- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 `DOM` 컬렉션 객체.

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <style>
        .red {
          color: red;
        }
        .blue {
          color: blue;
        }
      </style>
    </head>
    <body>
      <ul id="fruits">
        <li class="red">Apple</li>
        <li class="red">Banana</li>
        <li class="red">Orange</li>
      </ul>
      <script>
        const $elems = document.getElementsByClassName("red");
        console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

        for (let i = 0; i < $elems.length; i++) {
          $elems[i].className = "blue";
        }

        console.log($elems); // HTMLCollection(1) [li.red]
      </script>
    </body>
  </html>
  ```

  - 위 예제는 두 번째 `li` 요소만 `class` 값이 변경되지 않음.
  - 살아 있는 `DOM` 컬렉션 객체이므로 첫 번째 반복문에서 `blue`로 `class`를 변경하게 되면, `$elems`에서 실시간으로 제거됨.
  - 그 후 두 번째 반복문은 초기 세 번째 `li` 요소를 나타내므로 `1`, `3`만 변경.
  - `for` 문을 역방향으로 순회하거나, `while` 문을 사용하여 `HTMLCollection` 객체에 노드 객체가 남아있지 않을 때까지 반복하여 회피 가능.

> 근본적인 `HTMLCollection` 객체를 사용하지 않고, 배열로 변환하는 것이 가장 좋음.  
> 배열로 변환하면 `Array`의 함수 또한 사용 가능.

#### `NodeList`

- `NodeList` 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 `DOM` 컬렉션 객체.
- `NodeList.prototyp.forEach` 메서드를 상속 받아 사용할 수 있으며, `Array.prototyp.forEach`와 사용 방법이 동일.
- `childNodes` 프로퍼티가 반환하는 `NodeList` 객체는 `HTMLCollection`과 같이 실시간으로 노드 객체의 상태 변경을 반영하는 `live` 객체로 동작.

> 노드 객체의 상태 변경과 상관없이 안전하게 `DOM` 컬렉션을 사용하려면 해당 객체를 배열로 변환하여 사용하는 것을 권장.

---

## 노드 탐색

- `Node`, `Element` 인터페이스는 `DOM` 트리 상의 노드를 탐색할 수 있도록 트리 탐색 프로퍼티를 제공.
- `Node.prototype`의 탐색 접근자 프로퍼티

  - `parentNode`, `previousSibling`, `firstChild`, `childNodes`

- `Element.prototype`의 탐색 접근자 프로퍼티

  - `previousElementSibling`, `nextElementSibling`, `children`

- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티.
  - `setter` 없이 `getter`만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티.
  - 읽기 전용 접근자 프로퍼티에 값을 할당하면 아무런 에러 없이 무시.

### 공백 텍스트 노드

- `HTML` 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백(`white space`) 문자는 텍스트 노드를 생성.

> 이를 공백 텍스트 노드라고 함.

### 자식 노드 탐색

- 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않음.

> 텍스트 노드 또는 요소 노드만 반환.

- `Node.prototype.previousSibling`

  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환.
  - 요소 노드 혹은 텍스트 노드 반환.

- `Node.prototype.nextSibling`

  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환.
  - 요소 노드 혹은 텍스트 노드 반환.

- `Element.prototype.previousElementSibling`

  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환.
  - 요소 노드 반환.

- `Element.prototype.nextElementSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환.
  - 요소 노드 반환.

---

## 노드 정보 취득

### `Node.prototype.nodeType`

- 노드 객체의 종류. 즉, 노드 타입을 나타내는 상수를 반환.
- 노드 타입 상수는 `Node`에 정의되어 있음.
- `Node.ELEMENT_NODE`: 요소 노드 타입을 나타내는 상수 `1`을 반환.
- `Node.TEXT_NODE`: 텍스트 노드 타입을 나타내는 상수 `3`을 반환.
- `Node.DOCUMENT_NODE`: 문서 노드 타입을 나타내는 상수 `9`를 반환.

### `Node.prototype.nodeName`

- 노드의 이름을 문자열로 반환.
- 요소 노드: 대문자 문자열로 태그 이름(`"UL"`, `"LI"` 등)을 반환.
- 텍스트 노트: 문자열 `"#text"`를 반환.
- 문서 노드: 문자열 `"#document"`를 반환.

---

## 요소 노드의 텍스트 조작

### `nodeValue`

- `Node.prototype.nodeValue` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티로, 참조와 할당이 모두 가능.
- `nodeValue` 프로퍼티를 참조하면 노드 객체의 값을 반환.
- 노드 객체의 값이란 텍스트 노드의 텍스트이며, 텍스트 노드가 아닌 노드의 `nodeValue` 프로퍼티를 참조하면 `null` 반환.
- `nodeValue` 프로퍼티에 값을 할당하면 텍스트 노드의 값. 즉, 텍스트를 변경할 수 있음.
- 요소 노드의 텍스트를 변경하기 위한 순서
  1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드 탐색.(`firstChild` 프로퍼티)
  2. 탐색한 텍스트 노드의 `nodeValue` 프로퍼티를 사용하여 텍스트 노드의 값 변경.

### `textContent`

- `Node.prototype.textContent` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티.
- 요소 노드의 텍스트와 자손 노드의 텍스트를 모두 취득하거나 변경.
- 요소 노드의 `textContent` 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환.

> 요소 노드의 `childNodes` 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값. 즉, 텍스트를 모두 반환.(`HTML` 마크업은 무시.)

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello <span>world!</span></div>
  </body>
  <script>
    console.log(document.getElementById("foo").textContent); // Hello world!
  </script>
</html>
```

- `nodeValue` 프로퍼티도 텍스트를 취득할 수 있지만, 텍스트 노드의 `nodeValue`를 참조할 때만 의미가 있으므로 코드가 더 복잡.
- 요소 노드의 `textContent` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가.

  - 이때 할당한 문자열에 `HTML` 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급.
    > `HTML` 마크업이 파싱되지 않음.

- `textContent` 프로퍼티와 유사한 동작을 하는 `innerText` 프로퍼티가 있음.

  - `innerText` 프로퍼티는 `CSS`에 순종적.
    - `visibility: hidden;`과 같은 비표시로 지정된 요소 노드의 텍스트를 반환하지 않음.
  - `innerText` 프로퍼티는 `CSS`를 고려해야 하므로 `textContent`프로퍼티 보다 느림.

  > 위와 같은 이유로 사용하지 않는 것이 좋음.

---

## `DOM` 조작

> `DOM` 조작(`DOM manipulation`)은 새로운 노드를 생성하여 `DOM`에 추가하거나 기존 노드를 삭제 또는 교체하는 것.

- `DOM` 조작에 의해 `DOM`에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트 발생.
  - 성능에 영향을 줌.

> 복잡한 컨텐츠를 다루는 `DOM` 조작은 성능 최적화를 위해 주의해서 다루어야 함.

### `innerHTML`

- `Element.prototype.innerHTML` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티.
- 요소 노드의 `HTML` 마크업을 취득하거나 변경.
- 요소 노드의 `innerHTML` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 `HTML` 마크업이 파싱되어 요소 노드의 자식 노드로 `DOM`에 반영.

> 간단한 `DOM` 조작 가능.

- `innerHTML` 프로퍼티에 사용자로부터 입력받은 데이터(`untrusted input data`)를 그대로 할당하는 것은 크로스 사이트 스크립팅 공격(`Cross-site Script Attacks, XSS`)에 취약하므로 위험.
- `HTML5`는 `innerHTML` 프로퍼티로 삽입된 `script` 요소 내의 `JavaScript` 코드를 실행하지 않음.
- 기존 자식을 모두 제거하고 처음부터 다시 새롭게 자식 노드를 생성하여 `DOM`에 반영하므로 비효율적.
- 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없음.

#### `HTML 새니티제이션`(`HTML sanitization`)

- 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능.
- 직접 구현할 수도 있으나 `DOMPurity` 라이브러리를 사용하는 것을 권장.

### `insertAdjacentHTML` 메서드

- `Element.prototype.insertAdjacentHTML`(`position`, `DOMString`) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입.
- 첫 번째 인수로 삽입할 위치(`beforebegin`, `afterbegin`, `beforeend`, `afterend`)를 지정.
- 두 번째 인수로 `HTML` 마크업 문자열(`DOMString`) 전달.
- 기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 `innerHTML` 프로퍼티보다 빠르고 효율적.
- `HTML` 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약함.

### 노드 생성과 추가

#### 요소 노드 생성

- `Document.prototype.createElement(tagName)` 메서드는 요소 노드를 생성하여 반환.
- `tagName` 인수에는 태그 이름을 나타내는 문자열을 전달.
- `createElement` 메서드로 생성한 요소 노드는 기존 `DOM`에 추가되지 않고 홀로 존재하는 상태로, 이후에 `DOM`에 추가하는 처리가 필요.
- `createElement` 메서드로 생성한 요소 노드는 아무런 자식 노드가 없는 상태.

#### 텍스트 노드 생성

- `Document.prototype.createTextNode(text)` 메서드는 텍스트 노드를 생성하여 반환.
- `createTextNode` 메서드로 생성한 텍스트 노드는 요소 노드의 자식 노드에 추가되지 않고 홀로 존재하는 상태로, 이후에 요소 노드에 추가하는 처리가 필요.

#### 텍스트 노드를 요소 노드의 자식 노드로 추가

- `Node.prototype.appendChild(childNode)` 메서드는 인수로 전달한 노드를 메서드를 호출한 노드의 마지막 자식 노드로 추가.
- `childNode` 인수에는 추가할 자식 노드를 전달.
- 요소 노드에 자식 노드가 하나도 없는 경우에는 `textContent` 프로퍼티를 사용하여 텍스트 노드를 추가하는 것이 더욱 간편.
- 요소 노드에 자식 노드가 있는 경우 `textContent` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가되므로 주의.

#### 요소 노드를 `DOM`에 추가

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    // 1. 요소 노드 생성
    const $li = document.createElement("li");
    // 2. 텍스트 노드 생성
    const textNode = document.createTextNode("Banana");
    // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
    $li.appendChild(textNode);
    // 4. $li 요소를 #fruits 요소의 마지막 자식 노드로 추가
    $fruits.appendChild($li);
  </script>
</html>
```

> 기존 `DOM`에 요소 노드를 추가하는 처리는 이 과정. 이 때 리플로우와 리페인트 발생.

### 복수의 노드 생성과 추가

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    ["Apple", "Banana", "Orange"].forEach((text) => {
      const $li = document.createElement("li");
      const textNode = document.createTextNode(text);
      $li.appendChild(textNode);
      $fruits.appendChild($li);
    });
  </script>
</html>
```

- 위 예제는 3개의 요소 노드를 생성하여 `DOM`에 3번 추가하므로 `DOM`이 3번 변경.

> 리플로우와 리페인트가 3번 실행.

- 회피하기 위해 컨테이너 요소를 만들어 3개의 요소를 감싸고 추가하면 1번만 변경되지만, 불필요한 컨테이너(`div`) 요소가 추가됨.

#### `DocumentFragment`

- 문서, 요소, 어트리뷰트, 텍스트 노드와 같은 노드 객체의 일종.
- 부모 노드가 없어서 기존 `DOM`과는 별도로 존재한다는 특징.
- 자식 노드들의 부모노드로서 별도의 서브 `DOM`을 구성하여 기존 `DOM`에 추가하기 위한 용도.
- 기존 `DOM`과 별도로 존재하므로 `DocumentFragment` 노드에 자식 노드를 추가하여도 기존 `DOM`에는 어떠한 변경도 발생하지 않음.
- `DocumentFragment` 노드를 `DOM`에 추가하면 자신은 제거되고 자신의 자식 노드만 `DOM`에 추가.
- `Document.prototyp.createDocumentFragment` 메서드는 비어 있는 `DocumentFragment` 노드를 생성하여 반환.

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits"></ul>
    </body>
    <script>
      const $fruits = document.getElementById("fruits");
      const $fragment = document.createDocumentFragment();
      ["Apple", "Banana", "Orange"].forEach((text) => {
        const $li = document.createElement("li");
        const textNode = document.createTextNode(text);
        $li.appendChild(textNode);
        $fragment.appendChild($li);
      });
      $fruits.appendChild($fragment);
    </script>
  </html>
  ```

> 실제로 `DOM` 변경은 한 번 발생되므로, 리플로우와 리페인트가 한 번만 실행됨.

### 노드 삽입

#### 마지막 노드로 추가

- `Node.prototype.appendChild` 메서드는 인수로 전달받은 노드를 마지막 자식 노드로 `DOM`에 추가.

#### 지정한 위치에 노드 삽입

- `Node.prototype.insertBefore` 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입.
- 두 번째 인수로 전달받은 노드는 호출한 노드의 자식 노드이어야 함.
- 자식 노드가 아니면 `DOMException` 에러 발생.
- 두 번째 인수로 전달받은 노드가 `null` 이면 `appendChild` 메서드와 동일하게 동작.(마지막 자식 노드로 추가)

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    const $li = document.createElement("li");
    $li.appendChild(document.createTextNode("Orange"));
    $fruits.insertBefore($li, $fruits.lastElementChild);
  </script>
</html>
```

#### 노드 이동

- `DOM`에 이미 존재하는 노드를 `appendChild` 또는 `insertBefore` 메서드를 사용하여 `DOM`에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가.

> 즉, 노드가 이동함.

#### 노드 복사

- `Node.prototype.cloneNode` 메서드는 노드의 사본을 생성하여 반환.
- 매개변수 `deep`은 깊은 복사(자손 노드까지 포함된 노드 생성)와 얕은 복사(자신만의 사본 생성)를 결정.
- 얕은 복사는 텍스트 노드도 복사되지 않음.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    const $apple = $fruits.firstElementChild;
    const $shallowClone = $apple.cloneNode();
    $shallowClone.textContent = "Banana";
    $fruits.appendChild($shallowClone);
    const $deepClone = $fruits.cloneNode(true);
    $fruits.appendChild($deepClone);
  </script>
</html>
```

#### 노드 교체

- `Node.prototype.replaceChild` 메서드는 자식 노드를 다른 노드로 교체.
- 첫 번째 인수에는 교체할 새로운 노드를 전달.
- 두 번째 인수에는 이미 존재하는 교체될 노드를 전달.
  - 메서드를 호출한 노드의 자식 노드이어야 함.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    const $newChild = document.createElement("li");
    $newChild.textContent = "Banana";
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
  </script>
</html>
```

#### 노드 삭제

- `Node.prototype.removeChild` 메서드는 인수로 전달한 노드를 `DOM`에서 삭제.
- 인수로 전달한 노드는 메서드를 호출한 노드의 자식 노드이어야 함.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");
    $fruits.removeChild($fruits.lastElementChild);
  </script>
</html>
```

---

## 어트리뷰트

### 어트리뷰트 노드와 `attributes` 프로퍼티

- `HTML` 요소는 여러 개의 어트리뷰트(`attribute`, 속성)을 가질 수 있음.
- `HTML` 요소의 동작을 제어하기 위한 추가적인 정보를 제공하는 `HTML` 어트리뷰트는 `HTML` 요소의 시작 태그에 `어트리뷰트 이름="어트리뷰트 값"` 형식으로 정의.

  ```html
  <input id="user" type="text" value="ungmo2" />
  ```

- 글로벌 어트리뷰트(`id`, `class`, `style`, `title`, `lang`, `tabindex`, `draggable`, `hidden` 등)와 이벤트 핸들러 어트리뷰트(`onclick`, `onchange`, `onfocus`, `onblur`, `oninput`, `onkeypress`, `onkeydown`, `onkeyup`, `onmouseover`, `onsubmit`, `onload` 등)는 모든 `HTML` 요소에서 공통적으로 사용할 수 있음.
- `type`, `value`, `checked`는 `input` 요소에만 사용할 수 있는 것과 같이, 특정 `HTML` 요소에만 한정적으로 사용할 수 있는 어트리뷰트도 있음.
- `HTML` 문서가 파싱될 때 `HTML` 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결.
- `HTML` 어트리뷰트당 하나의 어트리뷰트 노드가 생성.
- 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 `NamedNodeMap` 객체에 담겨서 요소 노드의 `attributes` 프로퍼티에 저장.
- `Element.prototype.attributes` 프로퍼티로 요소 노드의 모든 어트리뷰트 노드를 취득할 수 있음.
- `Element.prototype.attributes`는 `getter`만 존재하는 읽기 전용 접근자 프로퍼티. `NamedNodeMap` 객체 반환.

### `HTML` 어트리뷰트 조작

- `Element.prototype.getAttribute`/`Element.prototype.setAttribute` 메서드를 사용하면 직접 `HTML` 어트리뷰트 값을 취득하거나 변경할 수 있음.
- `Element.prototype.hasAttribute` 메서드로 특정 어트리뷰트의 존재 확인 가능.
- `Element.prototype.removeAttribute` 메서드로 특정 어트리뷰트 삭제 가능.

### `HTML` 어트리뷰트 vs `DOM` 어트리뷰트

- 요소 노드 객체에는 `HTML` 어트리뷰트에 대응하는 프로퍼티(`DOM` 프로퍼티)가 존재.
- `DOM` 프로퍼티들은 `HTML` 어트리뷰트 값을 초기값으로 가지고 있음.
- `DOM` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티.

> `HTML` 어트리뷰트의 역할은 `HTML` 요소의 초기 상태를 지정하는 것으로, 첫 렌더링이 끝난 시점의 값과 동일하며, 변하지 않음.

- 요소 노드는 상태(`state`)를 가지고 있음.
- `input` 요소 노드는 사용자에 입력에 의해 변화.
- 그러나 초기 상태 값을 관리하지 않으면 웹페이지를 처음 표시하거나 새로고침할 때 초기 상태를 표시할 수 없음.

> 요소 노드는 초기 상태와 최신 상태를 관리해야 함.

#### 어트리뷰트 노드

> `HTML` 어트리뷰트로 지정한 `HTML` 요소의 초기 상태는 어트리뷰트 노드에서 관리.

- 어트리뷰트 노드에서 관리하는 어트리뷰트 값은 사용자의 입력에 의해 상태가 변경되어도 변하지 않고 `HTML` 어트리뷰트로 지정한 `HTML` 요소의 초기 상태를 그대로 유지.

#### `DOM` 프로퍼티

> 사용자가 입력한 최신 상태는 `HTML` 어트리뷰트에 대응하는 요소 노드의 `DOM` 프로퍼티가 관리.

- `DOM` 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지.
- `DOM` 프로퍼티에 값을 할당하는 것은 `HTML` 요소의 최신 상태 값을 변경하는 것을 의미.
- `HTML` 요소에 지정한 어트리뷰트 값에는 어떠한 영향도 주지 않음.

- 사용자 입력에 의한 상태 변화와 관계가 있는 `DOM` 프로퍼티만 최신 상태 값을 관리.
- 사용자 입력에 의한 상태 변화와 관계가 없는 `DOM` 프로퍼티는 항상 동일한 값으로 연동.

#### `HTML` 어트리뷰트와 `DOM` 프로퍼티의 대응 관계

- 대부분의 `HTML` 어트리뷰트는 `HTML` 어트리뷰트 이름과 동일한 `DOM` 프로퍼티와 1:1로 대응.
- 특수한 경우 언제나 1:1로 대응하는 것은 아니며, `HTML` 어트리뷰트 이름과 `DOM` 프로퍼티 키가 반드시 일치하는 것도 아님.

  - `id` 어트리뷰트와 `id` 프로퍼티는 1:1 대응하며, 동일한 값으로 연동.
  - `input` 요소의 `value` 어트리뷰트는 `value` 프로퍼티와 1:1 대응하며, 어트리뷰트는 초기 상태, 프로퍼티는 최신 상태를 가짐.
  - `class` 어트리뷰트는 `className`, `classList` 프로퍼티와 대응
  - `for` 어트리뷰트는 `htmlFor` 프로퍼티와 1:1 대응
  - `td` 요소의 `colspan` 어트리뷰트와 대응하는 프로퍼티가 존재하지 않음.
  - `textContent` 프로퍼티는 대응하는 어트리뷰트가 존재하지 않음.
  - 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따름(`Ex)` `maxlength` -> `maxLength`)

#### `DOM` 프로퍼티 값의 타입

- `HTML` 어트리뷰트 값은 언제나 문자열.
- `DOM` 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수 있음.
  `Ex)` `checked` 어트리뷰트와 프로퍼티.

### `data` 어트리뷰트와 `dataset` 프로퍼티

- `data` 어트리뷰트와 `dataset` 프로퍼티를 사용하면 `HTML` 요소에 사용자 정의 어트리뷰트와 `JavaScript` 간에 데이터를 교환할 수 있음.
- `data` 어트리뷰트는 `data-` 접두사 다음에 이름을 붙여 사용.
- `HTMLElement.dataset` 프로퍼티를 통해 `data` 어트리뷰트 값을 취득.
- `dataset` 프로퍼티는 `HTML` 요소의 모든 `data` 어트리뷰트의 정보를 제공하는 `DOMStringMap` 객체를 반환.
- `DOMStringMap` 객체는 `data` 어트리뷰트의 `data-` 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있음.
- `data-` 접두사 다음에 존재하지 않는 이름을 키로 사용하여 `dataset` 프로퍼티에 값을 할당하면 `HTML` 요소에 `data` 어트리뷰트가 추가됨.
  - 카멜 케이스의 프로퍼티 키는 `data-` 접두사 다음에 케밥 케이스로 자동 변경되어 추가.

---

## 스타일

### 인라인 스타일 조작

- `HTMLElement.prototype.style` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일(`inline style`)을 취득하거나 추가 또는 변경.

```html
<!DOCTYPE html>
<html>
  <body>
    <div style="color: red">Hello World</div>
  </body>
  <script>
    const $div = document.querySelector("div");
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }
    $div.style.color = "blue";
    $div.style.width = "100px";
    $div.style.height = "100px";
    $div.style.backgroundColor = "yellow";
  </script>
</html>
```

- `style` 프로퍼티를 참조하면 `CSSStyleDeclaration` 객체를 반환.
- `CSSStyleDeclaration` 객체는 다양한 `CSS` 프로퍼티에 대응하는 프로퍼티를 가지고 있으며, 이 프로퍼티에 값을 할당하면 해당 `CSSStyleDeclaration` 프로퍼티가 인라인스타일로 `HTML` 요소에 추가되거나 변경.
- `CSS` 프로퍼티는 케밥 케이스를 따르며, 이에 대응하는 `CSSStyleDeclaration` 객체는 카멜 케이스를 따름.(`Ex)` `background-color` -> `backgroundColor`)
- 케밥 케이스의 `CSS` 프로퍼티를 그대로 사용하려면 대괄호 표기법 사용.
  ```js
  $div.style["background-color"] = "yellow";
  ```

### 클래스 조작

- `class` 어트리뷰트에 대응하는 `DOM` 프로퍼티는 `class`가 아니라 `className`과 `classList`.
  - `class`가 `JavaScript` 예약어이기 때문.

#### `className`

- `Element.prototype.className` 프로퍼티는 `setter`와 `getter` 모두 존재하는 접근자 프로퍼티로서 `HTML` 요소의 `class` 어트리뷰트 값을 취득하거나 변경.
- `className` 프로퍼티는 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편

#### `classList`

- `Element.prototype.classList` 프로퍼티는 `class` 어트리뷰트의 정보를 담은 `DOMTokenList` 객체를 반환.
- `DOMTokenList` 객체는 `class` 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블.
- `DOMTokenList` 객체의 메서드
  - `add`: 인수로 전달한 1개 이상의 문자열을 `class` 어트리뷰트 값으로 추가
  - `remove`: 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 `class` 어트리뷰트에서 삭제.
    - 일치하는 클래스가 없으면 에러 없이 무시.
  - `item`: 인수로 전달한 `index`에 해당하는 클래스를 `class` 어트리뷰트에서 반환.
  - `contains`: 인수로 전달한 문자열과 일치하는 클래스가 `class` 어트리뷰트에 포함되어 있는지 확인.
  - `replace`: `class` 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경.
  - `toggle`: 인수로 전달한 문자열과 일치하는 클래스가 `class` 어트리뷰트에 존재하면 제거하고, 존재하지 않으면 추가.
  - `forEach`, `entires`, `keys`, `values`, `supports` 메서드 등.

### 요소에 적용되어 있는 `CSS` 스타일 참조

- `style` 프로퍼티는 인라인 스타일만 반환.
- 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 `style` 프로퍼티로 참조할 수 없음.
- `window.getComputedStyle` 메서더는 첫 번째 인수로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 `CSSStyleDeclaration` 객체에 담아 반환.
- 평가된 스타일이란, 모든 스타일이 조합되어 최종적으로 적용된 스타일.
- `getComputedStyle` 메서드의 두 번째 인수로 `::before`, `::after`와 같은 의사 요소를 지정하는 문자열을 전달할 수 있음.
  - 의사 요소가 아닌 일반 요소의 경우 두 번째 인수 생략.

---

## `DOM` 표준

- `HTML`과 `DOM` 표준은 `W3C`(World Wide Web Consortium)과 `WHATWG`(Web Hypertext Application Technology Working Group)이라는 단체가 공통된 표중 제작.
- 이제부터는 `WHATWG`(주류 브라우저 벤더사가 주도)이 단일 표준을 내놓기로 함
- `DOM`은 현재 4개의 레벨(버전)이 있음.

|     레벨      |            표준 문서 `URL`             |
| :-----------: | :------------------------------------: |
| `DOM Level 1` | https://www.w3.org/TR/REC-DOM-Level-1  |
| `DOM Level 2` | https://www.w3.org/TR/DOM-Level-2-Core |
| `DOM Level 3` | https://www.w3.org/TR/DOM-Level-3-Core |
| `DOM Level 4` |      https://dom.spec.whatwg.org       |
