# 39. DOM

### DOM

> **HTML 문서의 계층적 구조와 정보를 표현**하며 이를 제어할 수 있는 API,
> 즉 **프로퍼티와 메서드를 제공하는 트리 자료구조**

## 39.1 노드

### HTML 요소와 노드 객체

![alt text](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/5466c610-16c7-4e54-a18f-e027e025dbf2/Untitled.png?table=block&id=ced5d6e0-8e5f-47e2-abf1-d5a40bfe18d4&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=dt7lDwtmJmxhj6BJw6Ltmt61FJ1Brhr_0qc4AtQbtkg&downloadName=Untitled.png)

- **HTML 요소**: 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환
  - HTML 요소의 어트리뷰트 → 어트리뷰트 노드
  - HTML 요소의 텍스트 콘텐츠 → 텍스트 노드
- HTML 요소 간의 부자 관계를 반영하여 **모든 노드 객체들을 트리 자료구조로 구성**

💡 **트리 자료구조**
부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조

⇒ **노드 객체들로 구성된 트리 자료구조: DOM**(Document Object Model)

### 중요한 노드 타입

1. **문서 노드
   DOM 트리의 최상위에 존재하는 루트 노드**로서 `document` 객체를 가리킴 - 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 `window`의 document 프로퍼티에 바인딩 - 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 entry point 역할
2. **요소 노드**
   HTML 요소를 가리키는 객체 - 문서의 구조를 표현
3. **어트리뷰트 노드**
   HTML 요소의 어트리뷰트를 가리키는 객체 - 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결
4. **텍스트 노드**

   HTML 요소의 텍스트를 가리키는 객체

   - 요소 노드의 자식이며, 자식 노드를 가질 수 없는 리프 노드

### 노드 객체의 상속 구조

DOM을 구성하는 노드 객체: 자신의 정보를 제어할 수 있는 DOM API를 사용할 수 있음 → 부모, 형제, 자식을 탐색하고 어트리뷰트와 텍스트를 조작

- ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체

![노드 객체의 상속 구조](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/7b14f696-5105-4158-aad1-ef5a29a8206e/Untitled.png?table=block&id=cdac5dd2-5999-4abc-af4c-309ab21684ba&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=5HHjVvVJ1w9cl3nuzV2xZnngmyHsw3hUSoutJvkGLZA&downloadName=Untitled.png)

노드 객체의 상속 구조

- **모든 노드 객체**는 `Object`, `EventTarget`, `Node` 인터페이스를 상속 받음
- **문서 노드**: `Document`, `HTMLDocument` 인터페이스를 상속 받음
- **어트리뷰트 노드:** `Attr`, 텍스트 노드는 `CharacterData` 인터페이스를 상속 받음
- **요소 노드**: `Element` 인터페이스를 상속 받음

![alt text](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/f28eba54-36a9-4a87-a130-3e4a4392c1ee/Untitled.png?table=block&id=08e9024b-4e68-4e63-9b43-91140e320a6c&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=kSIs9QcWUX79_wQTLsMalo7IqHQqqSUyUJOv7RjmYrw&downloadName=Untitled.png)

- 노드 객체에는 노드 타입에 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고, 노드 타입에 따라 고유한 기능도 있음
  e.g. input 요소 노드 객체는 value 프로퍼티가 필요하지만, div 요소 노드 객체는 value 프로퍼티가 필요하지 않음

→ 필요한 기능을 제공하는 인터페이스(HTMLInputElement, HTMLDivElement 등)가 HTML 요소의 종류에 따라 다름

💡 **DOM**은 HTML **문서의 계층적 구조와 정보를 표현**하는 것은 물론,
**노드 타입에 따라 필요한 기능**을 프로퍼티와 메서드의 집합인 **DOM API로 제공**

## 39.2 요소 노드 취득

### id를 이용한 요소 노드 취득

`Document.prototype.getElementById` 메서드: 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환

- Document.prototype의 프로퍼티이므로 반드시 문서 노드인 document를 통해 호출해야 함
- 중복된 id가 존재하더라도 첫번째 요소 노드만 반환
- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과
  ```html
  <div id="foo"></div>
  <script>
    console.log(foo == document.getElementById("foo")); // true
    delete foo;
    console.log(foo); // <div id="foo"></div>
  </script>
  ```

### 태그 이름을 이용한 요소 노드 취득

`Document.prototype` / `Element.prototype.getElementsByTagName` 메서드: 인수로 전달한 태그 이름을 갖는 모든 요소 노드를 탐색하여 반환

```html
<ul>
  <li id="apple">apple</li>
  <li id="orange">orange</li>
</ul>
<script>
  const $elems = document.getElementsByTagName("li");

  [...$elems].forEach((elem) => {
    elem.style.color = "red";
  });
</script>
```

- 탐색된 요소 노드들은 `HTMLCollection` 객체에 담겨 반환(유사 배열 객체이면서 이터러블)

### class를 이용한 요소 노드 취득

`Document.prototype` / `Element.prototype.getElementsByClassName` 메서드: 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환

- 문서 노드인 `document`를 통해 호출하면 *DOM 전체에서 요소 노드를 탐색*하여 반환
- `Element`를 통해 호출하면 _특정 요소 노드의 자손 노드 중에서 탐색_

### 39.4 CSS 선택자를 이용한 요소 노드 취득

`Document.prototype` / `Element.prototype.querySelector` 메서드: 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환

- 여러 개인 경우 첫번째 노드만 반환
- 존재하지 않는 경우 null 반환

`Document.prototype` / `Element.prototype.querySelectorAll` 메서드: 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 NodeList 객체를 반환

- css 선택자를 사용하는 `querySelector`, `querySelectorAll` 메서드는 `getElementById`, `getElementsBy***` 메서드보다 느림

### 특정 요소 노드를 취득할 수 있는지 확인

`Element.prototype.matches` 메서드: 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인

- 이벤트 위임을 사용할 때 유용

### HTMLCollection과 NodeList

> DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체

- 유사 배열 객체이면서 이터러블 → for…of 문 순회 가능, 스프레드 문법으로 배열로 변환
- 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 live 객체

### HTMLCollection

```html
<ul id="fruits">
  <li class="red">Apple</li>
  <li class="red">Banana</li>
  <li class="red">Orange</li>
</ul>
<script>
  const $elems = document.getElementsByClassName("red");
  for (let i = 0; i < $elems.length; i++) {
    $elems[i].className = "blue";
  }

  console.log($elems); // HTMLCollection(1) [li.red]
</script>
```

for문을 순회하는 도중 `$elems`가 실시간으로 변경되어 예상한대로 동작하지 않음

해결책

1. for문을 역방향으로 순회

   ```jsx
   for (let i = $elems.length - 1; i >= 0; i--) {
     $elems[i].className = "blue";
   }
   ```

2. while 문으로 순회

   ```jsx
   let i = 0;
   while ($elems.length > i) {
     $elems[i].className = "blue";
   }
   ```

3. `HTMLCollection` 객체를 배열로 변환

   ```jsx
   [...$elems].forEach((elem) => (elem.className = "blue"));
   ```

### NodeList

대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 **non-live 객체로 동작**

- 하지만 `childNodes` 프로퍼티가 반환하는 `NodeList` 객체는 `HTMLCollection` 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 **live 객체로 동작**

```jsx
const $fruits = document.getElementById("fruits");

const { childNodes } = $fruits; // live 객체를 반환
```

노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 `HTMLCollection`이나 `Nodelist` 객체를 **배열로 변환해 사용**

```jsx
[...childNodes].forEach((childNode) => {
  $fruits.removeChild(childNode);
});
```

## 39.3 노드 탐색

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="bananan">Banana</li>
  <li class="orange">Orange</li>
</ul>
```

![트리 노드 탐색 프로퍼티](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/5d7ec8a3-af33-4eb4-a874-d8476755012d/Untitled.png?table=block&id=fee2ea34-2bb8-49bd-9968-529f9fa12c28&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=gO-COjXOkZxVkHo0LVEs4NXvI43kW5enRMWb9G45ZNU&downloadName=Untitled.png)

트리 노드 탐색 프로퍼티

DOM 트리 상의 노드를 탐색할 수 있도록 `Node`, `Element` 인터페이스는 **트리 탐색 프로퍼티를 제공**

- 노드 탐색 프로퍼티는 모두 setter 없이 getter만 존재하는 _읽기 전용 접근자 프로퍼티_

### 공백 텍스트 노드

> HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="bananan">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
</html>
```

![alt text](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/2fad30a2-bcd0-4702-b827-d2a65b00fa18/Untitled.png?table=block&id=a0d78876-ae55-4a97-981e-582b38e33a3e&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=O0tvSgGRE40p7vs37Tsf-VFpP3t45qZN7hZ4FsvV6CU&downloadName=Untitled.png)

텍스트 에디터에서 HTML 문서에 스페이스, 탭, 엔터 등을 입력하면 공백 문자가 추가

### 자식 노드 탐색

| 프로퍼티                                | 설명                                           |
| --------------------------------------- | ---------------------------------------------- |
| Node.prototype.childNodes               | 자식 노드를 모두 탐색해 NodeList를 반환        |
| (요소 노드뿐만 아니라 텍스트 노드 포함) |
| Node.prototype.children                 | 요소 노드만 탐색하여 HTMLCollection 반환       |
| Node.prototype.firstChild               | 첫번째 자식 노드를 반환(텍스트 노드/요소 노드) |
| Node.prototype.lastChild                | 마지막 자식 노드를 반환(텍스트 노드/요소 노드) |
| Element.prototype.firstElementChild     | 첫번째 자식 요소 노드를 반환                   |
| Element.prototype.lastElementChild      | 마지막 자식 요소 노드를 반환                   |

### 자식 노드 존재 확인

`Node.prototype.hasChildNodes` 메서드 - 자식 노드가 존재하면 true, 존재하지 않으면 false 반환

- 텍스트 노드를 포함
- 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 `children.length` 또는 `Element.prototype.childElementCount` 프로퍼티 사용

### 요소 노드의 텍스트 노드 탐색

- 요소 노드의 텍스트 노드는 **요소 노드의 자식 노드** → `firstChild` 프로퍼티로 접근 가능

### 부모 노드 탐색

`Node.prototype.parentNode` 프로퍼티 사용

### 형제 노드 탐색

| 프로퍼티                                 | 설명                              |
| ---------------------------------------- | --------------------------------- |
| Node.prototype.previousSibling           | 자신의 이전 형제 노드를 반환      |
| (요소 노드/텍스트 노드)                  |
| Node.prototype.nextSibling               | 자신의 다음 형제 노드를 반환      |
| (요소 노드/텍스트 노드)                  |
| Element.prototype.previousElementSibling | 자신의 이전 형제 요소 노드를 반환 |
| Element.prototype.nextElementSibling     | 자신의 다음 형제 요소 노드를 반환 |

## 39.4 노드 정보 취득

- `Node.prototype.nodeType`
  노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환
- `Node.prototype.nodeName`
  노드의 이름을 문자열로 반환

```html
<body>
  <div id="foo">Hello</div>
</body>
<script>
  const $foo = document.getElementById("foo");
  console.log($foo.nodeType); // 1
  console.log($foo.nodeName); // DIV
</script>
```

## 39.5 요소 노드의 텍스트 조작

### nodeValue

> `Node.prototype.nodeValue` 프로퍼티 - setter, getter 모두 존재하는 접근자 프로퍼티

- 텍스트 노드가 아닌 노드(문서 노드나 요소 노드)의 nodeValue 프로퍼티의 값을 참조하면 null을 반환

```html
<body>
  <div id="foo">Hello</div>
</body>
<script>
  const $foo = document.getElementById("foo");
  console.log($foo.nodeValue); // null

  const $textNode = $foo.firstChild;
  console.log($textNode.nodeValue); // Hello
</script>
```

### textContent

> `Node.prototype.textConten`t - setter, getter 모두 존재하는 접근자 프로퍼티

- 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경

```html
<body>
  <div id="foo">Hello <span>world!</span></div>
</body>
<script>
  console.log(document.getElementById("foo").textContent);
  // Hello world!
</script>
```

- 요소 노드의 `textContent` 프로퍼티에 문자열을 할당하면 HTML 마크업이 포함되어 있더라도 파싱되지 않고 문자열 그대로 인식

```html
<body>
  <div id="foo">Hello <span>world!</span></div>
</body>
<script>
  document.getElementById("foo").textContent = "Hi <span>there!</span>";
</script>
```

🚨 `textContent` 프로퍼티와 유사한 동작을 하는 `innerText` 프로퍼티는 사용하지 않는 것이 좋음 (textContent 보다 느림)

## 39.6 DOM 조작

> DOM 조작: 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것

→ 리플로우와 리페인트 발생

### innerHTML

> `Element.prototype.innerHTML` 프로퍼티: getter와 setter 모두 존재하는 접근자 프로퍼티,
> **요소 노드의 HTML 마크업을 취득하거나 변경**

- 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환
- `innerHTML` 프로퍼티에 할당한 HTML 마크업 문자열: 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영
  🚨 사용자로부터 입력받은 데이터를 그대로 `innerHTML` 프로퍼티에 할당하는 것은 **XSS 공격(크로스 사이트 스크립팅 공격)에 취약하므로 위험**
- **HTML5**: `innerHTML` 프로티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않음

```html
// XSS 공격 예시
<body>
  <div id="foo">Hello</div>
</body>
<script>
  document.getElementById(
    "foo"
  ).innerHTML = `<img src="x" onerror="alert(document.cookie)">`;
</script>
```

💡 **HTML 새니티제이션(HTML sanitization)**
: 사용자로부터 입력받은 XSS 공격을 예방하기 위해 잠재적 위험을 제거
**DOMPurify** 라이브러리를 사용하는 것을 권장
`DOMPurity.sanitize(’<img src="x" onerror="alert(document.cookie)">’);`

```html
	<ul id="fruits">
		<li class="apple">Apple</li>
	</ul>
</body>
<script>
	const $fruits = document.getElementById('fruits');
	$fruits.innerHTML += '<li class="banana">Banana</li>';
</script>
```

🚨 `innerHTML`은 기존의 자식 노드까지 모두 제거하고 **처음부터 새롭게 자식노드를 생성하여 DOM에 반영**

→ `li.apple`을 제거하고 새롭게 `li.apple`과 `li.banana`를 생성하여 `#fruits`의 자식 요소로 추가

- 또한 innerHTML는 새로운 요소의 삽입 위치를 조정할 수 없음

### insertAdjacentHTML

> `Element.prototype.insertAdjacentHTML(position, DOMString)` 메서드: 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입

- 두 번째 인수로 전달한 HTML 마크업 문자열(DOMString)으로 파싱하고 `position`에 삽입하여 DOM에 반영
- `position`으로 전달할 수 있는 문자열
  - beforebegin
  - afterbegin
  - beforeend
  - aftereend
- 새로 삽입될 요소만을 파싱하여 자식 요소로 추가하므로 `innerHTML` 프로퍼티보다 효율적이고 빠름
- 하지만 XSS 공격에 취약하다는 점은 동일

### 노드 생성과 추가

> `Document.prototype.createElement(tagName)` 메서드: 요소 노드를 생성하여 반환

```jsx
const $li = document.createElement("li");

console.log($li.childNodes); // NodeList []
```

- 자식 노드인 텍스트 노드를 포함해 아무런 자식 노드가 없음

> `Document.prototype.createTextNode(text)` 메서드: 텍스트 노드를 생성해 반환

```jsx
const textNode = document.createTextNode("Banana");

$li.appendChild(textNode);
```

> `DocumentFragment` 노드: 노드 객체의 일종으로, 부모 노드가 없이 기존 DOM과 별도로 존재

- 별도의 서브 DOM을 구성해 기존 DOM에 추가하기 위한 용도로 사용

```jsx
["apple", "banana", "orange"].forEach((text) => {
  const $li = document.createElement("li");
  const textNode = document.createTextNode(text);
  $li.appendChild(textNode);
  $fruits.appednChild($li);
});
```

🚨 DOM에 3번 추가하므로 **리플로우와 리페인트가 3번 실행**된다 → **DOM을 변경하는 것은 높은 비용**이 드는 처리

![DocumentFragment 노드](https://file.notion.so/f/f/7fcc553f-a6b8-4870-a93c-292d8556f276/63527b2f-6e28-46ff-84c8-274ce06be9ec/Untitled.png?table=block&id=cf5c4c28-ca70-4c5b-b453-cdf6f99ac58e&spaceId=7fcc553f-a6b8-4870-a93c-292d8556f276&expirationTimestamp=1723507200000&signature=61fgpR1rQ4y5Vfq6rDdJuLe6stiLwqgl4Ufe48zjt0A&downloadName=Untitled.png)

DocumentFragment 노드

- `DocumentFragment` 노드를 DOM에 추가하면 자신은 제거되고 **자신의 자식 노드만 DOM에 추가됨**

```jsx
const $fragment = document.createDocumentFragment();

["apple", "banana", "orange"].forEach((text) => {
  const $li = document.createElement("li");
  const textNode = document.createTextNode(text);
  $li.appendChild(textNode);
  $fragment.appednChild($li);
});

$fruits.appendChild($fragment);
```

### 노드 삽입

> `Node.prototype.appendChild` 메서드: 인수로 전달받은 노드를 자신을 **호출한 노드의 마지막 자식 노드로 추가**

> `Node.prototype.insertBefore(newNode, childNode)` 메서드: 첫번째 인수로 전달받은 노드를 두번째 인수로 전달받은 노드 앞에 삽입

### 노드 이동

- DOM에 이미 존재하는 노드를 `appendChild` 또는 `insertBefore` 메서드로 **DOM에 다시 추가**하면
  → 현재 위치에서 노드를 제거하고 **새로운 위치로 이동**

### 노드 복사

> `Node.prototype.cloneNode([deep: true | false])` 메서드: 노드의 사본을 생성하여 반환

- deep=true일 경우 노드를 깊은 복사해 모든 자손 노드가 포함된 사본을 생성

### 노드 교체

> `Node.prototype.replaceChild(newChild, oldChild)` 메서드: 자신을 호출한 노드의 자식 노드를 다른 노드로 교체

### 노드 삭제

> `Node.prototype.removeChild(child)` 메서드: child 매개변수에 인수로 전달한 노드를 DOM에서 삭제

## 39.7 어트리뷰트

> HTML 어트리뷰트: HTML은 여러개의 attribute를 가질 수 있음
> `어트리뷰트 이름=”어트리뷰트 값”`

- **특정 HTML 요소에만 한정적으로 사용 가능**한 어트리뷰트도 있음 e.g. type, value, checked ..
- HTML 문서가 파싱될 때
  → HTML 요소의 어트리뷰트는 **어트리뷰트 노드로 변환되어 요소 노드와 연결**됨
  이 때 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 `NamedNodeMap` 객체에 담겨 프로퍼티에 저장

> `Element.prototype.attributes` 프로퍼티: 요소 노드의 모든 어트리뷰트 노드를 취득할 수 있음. getter만 존재하는 읽기 전용 접근자 프로퍼티

```html
<input id="user" type="tex" value="jwoo" />
<script>
  const { attributes } = document.getElementById("user");
  console.log(attributes.id.value); // user
</script>
```

### HTML 어트리뷰트 조작

> `Element.prototype.getAttribute/setAttribute` 메서드: attribute 프로퍼티를 통하지 않고 요소 **노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경**

```html
<input id="user" type="tex" value="jwoo" />
<script>
  const $input = document.getElementById("user");

  const inputValue = $input.getAttribute("value");
  $input.setAttribute("value", "foo");

  // 어트리뷰트가 존재하는지 확인
  if ($input.hasAttribute("value")) {
    //어트리뷰트 제거
    $input.removeAttribute("value");
  }
</script>
```

### HTML 어트리뷰트 vs. DOM 프로퍼티

**요소 노드 객체**: HTML 어트리뷰트에 대응하는 **DOM 프로퍼티가 존재**

→ HTML 어트리뷰트 값을 초기값으로 가지고 있음

```html
<input id="user" type="tex" value="jwoo" />
<script>
  const $input = document.getElementById("user");

  $input.value = "foo"; // 요소 노드의 value 프로퍼티 값 변경
  console.log($input.value); // foo
</script>
```

- HTML 어트리뷰트가 관리되고 있는 곳
  - 요소 노드의 `attributes` 프로퍼티
  - HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티
    → 중복 관리되지 않음
    HTML 어트리뷰트의 역할은 요소의 초기 상태를 지정 → 어트리뷰트 노드로 변환되어 `attributes` 프로퍼티에 저장
- **어트리뷰트 노드**
  HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리
  - 사용자 입력에 의해 상태가 변경되어도 초기 상태를 유지
  - getAttribute/setAttribute 메서드로 초기 상태를 취득하거나 변경
- **DOM 프로퍼티**
  사용자가 입력한 최신 상태를 유지하고 관리(사용자 입력에 의한 상태 변화와 관계있는 프로퍼티만)
- HTML 어트리뷰트와 DOM 프로퍼티는 1:1로 대응하지만 항상 그런 것은 아님
  - class 어트리뷰트는 className, classList 프로퍼티와 대응
  - for 어트리뷰트는 htmlFor 프로퍼티와 대응

### data 어트리뷰트와 dataset 프로퍼티

HTML 요소에 `data-` 접두사 다음 임의의 이름을 붙여 **사용자 정의 어트리뷰트**와 자**바스크립트 간에 데이터를 교환**할 수 있음

```html
<ul class="users">
  <li id="1" data-user-id="7621" data-role="admin">lee</li>
  <li id="2" data-user-id="9524" data-role="subscriber">lee</li>
</ul>
```

- `HTMLElement.dataset` 프로퍼티로 data 어트리뷰트의 값을 취득 - `DOMStringMap` 객체 반환
  - `data-` 접두사 다음에 붙인 이름을 *카멜 케이스로 변환한 프로퍼티*로 가지고 있음

```jsx
const users = [...document.querySelector(".users").children];

const user = users.find((user) => user.dataset.userId === "7621");
user.dataset.role = "subscriber";
```

## 39.8 스타일

### 인라인 스타일 조작

`HTMLElement.prototype.style` 프로퍼티: setter와 getter 모두 존재하는 접근자 프로퍼티

```html
<div style="color: red">hello</div>
<script>
  const $div = document.querySelector("div");

  $div.style.color = "blue";
  $div.style.width = "100px";
</script>
```

- CSS 프로퍼티는 *케밥 케이스*를 따름
  - e.g. background-color → `backgroundColor`

### 클래스 조작

> `Element.prototype.className`: setter와 getter 모두 존재하는 접근자 프로퍼티

```html
<div class="box red" style="color: red">hello</div>
<script>
  const $div = document.querySelector("div");

  $div.className.replace("red", "blue");
</script>
```

> `Element.prototype.classList`: class 어트리뷰트의 정보를 담은 **DOMTokenList** 객체를 반환

```html
<div class="box red" style="color: red">hello</div>
<script>
  const $div = document.querySelector("div");

  $div.classList.replace("red", "blue");
</script>
```

- DOMTokenList가 제공하는 메서드: add, remove, item, contains, replace, toggle …

### 요소에 적용되어 있는 CSS 스타일 참조

> `window.getComputedStyle(element[, pseudo])`: 요소 노드에 적용된 스타일을 CSSStyleDeclaration 객체에 담아 반환

## 39.9 DOM 표준

- 구글, 애플, 마이크로소프트, 모질라로 구성된 4개 주류 브라우저 벤더사가 주도하는 WHATWG가 내놓은 단일 표준이 존재하고 4개의 버전이 있음
