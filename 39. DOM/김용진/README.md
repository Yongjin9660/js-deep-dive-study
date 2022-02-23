# 39. DOM

> 💡 DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API

## 39.1 노드

### HTML 요소와 노드 객체

![image](https://user-images.githubusercontent.com/55246584/155140963-b464c1d4-0f8c-49ba-b8cb-62a49f7d32ec.png)

👉 HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM에 구성하는 요소 노드 객체로 변환됨

**트리 자료구조**

- 노드들의 계층 구조로 이뤄짐
- 노드 객체들로 구성된 트리 자료구조를 `DOM`이라 함

![image](https://user-images.githubusercontent.com/55246584/155319510-c3bd3f0b-51fa-4af5-8c04-971d4cc70046.png)

### 노드 객체의 타입

노드 객체는 총 12개의 종류가 있음. 이 중에서 중요한 노드 타입은 다음과 같이 4가지

**문서 노드**

- DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
- document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당함

**요소 노드**

- HTML 요소를 가리키는 객체
- 문서의 구조를 표현함

**어트리뷰트 노드**

- HTML 요소의 어트리뷰트를 가리키는 객체
- 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있음

**텍스트 노드**

- HTML 요소의 텍스트를 가리키는 객체
- 문서의 정보를 표현
- 자식 노드를 가질 수 없는 리프 노드

### 노드 객체의 상속 구조

> 💡 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐

![image](https://user-images.githubusercontent.com/55246584/155320546-99fb667e-0834-4c6b-8b1a-aedf74e34867.png)

👉 이를 프로토타입 체인 관점에서 살펴보면 input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용 가능

![image](https://user-images.githubusercontent.com/55246584/155320895-71d4379f-e5ad-42b7-80f7-bf6427d2b5c8.png)

```html
<!DOCTYPE html>
<html>
	<body>
		<input type="text" />
		<script>
			const $input = document.querySelector('input');

			// input 요소 노드 객체의 프로토타입 체인
			console.log(
				Object.getPrototypeOf($input) === HTMLInputElement.prototype,
				Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
				Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
				Object.getPrototypeOf(Element.prototype) === Node.prototype,
				Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
				Object.getPrototypeOf(EventTarget.prototype) === Object.prototype
			); // 모두 true
		</script>
	</body>
</html>
```

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG를 표현하는 객체) | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

**DOM**

- HTML 문서의 계층적 구조와 정보를 표현
- 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공
- 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작 가능

## 39.2 요소 노드 취득

### id를 이용한 요소 노드 취득

> 💡 Document.prototype.getElementById 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환

- 중복된 id 값을 갖는 요소가 여러 개 존재하면 첫 번째 요소 노드만 반환
- 요소가 존재하지 않는다면 null을 반환

```js
const $elem = document.getElementById('banana');
```

- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있음

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo"></div>
		<script>
			// id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당
			console.log(foo === document.getElementById('foo')); // true
		</script>
	</body>
</html>
```

### 태그 이름을 이용한 요소 노드 취득

> 💡 Document.prototype/Element.prototype.getElementsByTagName 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환

- DOM 컬렉션 객체인 HTMLCollection 객체를 반환

```html
<!DOCTYPE html>
<html>
	<body>
		<ul>
			<li id="apple">Apple</li>
			<li id="banana">Banana</li>
			<li id="orange">Orange</li>
		</ul>
		<script>
			// 태그 이름이 li인 요소 노드를 모두 탐색하여 반환
			// 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환
			// HTMLCollection 객체는 유사 배열 객체이면서 이터러블
			const $elems = document.getElementsByTagName('li');

			// HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경
			[...$elems].forEach((elem) => {
				elem.style.color = 'red';
			});
		</script>
	</body>
</html>
```

- HTMLCollection 객체는 `유사 배열 객체이면서 이터러블`

**Document.prototype.getElementsByTagName**

- DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출
- DOM 전체에서 요소 노드를 탐색하여 반환

**Element.prototype.getElementsByTagName**

- 특정 요소 노드를 통해 호출
- 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환

### class를 이용한 요소 노드 취득

> 💡 Document.prototype/Element.prototype.getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환

- DOM 컬렉션 객체인 HTMLCollection 객체를 반환

### CSS 선택자를 이용한 요소 노드 취득

> 💡 CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법

```js
/* 전체 선택자: 모든 요소를 선택 */
* { ... }

/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p { ... }

/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo { ... }

/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo { ... }

/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type=text] { ... }

/* 후손 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div p { ... }

/* 자식 선택자: div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }

/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }

/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }

/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover { ... }

/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
  일반적으로 content 프로퍼티와 함께 사용됨 */
p::before { ... }
```

**Document.prototype/Element.prototype.querySelector**

- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환
- 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

**Document.prototype/Element.prototype.querySelectorAll**

- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 `NodeList 객체`를 반환
- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생

`NodeList 객체`: 유사 배열 객체이면서 이터러블

### 특정 요소 노드를 취득할 수 있는지 확인

> 💡 Element.prototype.matches 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인

```html
<!DOCTYPE html>
<html>
	<body>
		<ul>
			<li id="apple">Apple</li>
			<li id="banana">Banana</li>
			<li id="orange">Orange</li>
		</ul>
		<script>
			const $apple = document.querySelector('.apple');

			// $apple 노드는 '#fruits > li.apple'로 취득할 수 있음
			console.log($apple.matches('#fruits > li.apple')); // true

			// $apple 노드는 '#fruits > li.banana'로 취득할 수 없음
			console.log($apple.matches('#fruits > li.banana'));
		</script>
	</body>
</html>
```

### HTMLCollection과 NodeList

> 💡 DOM 컬렉션 객체인 HTMLCollection과 NodeListsms DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체

HTMLCollection과 NodeList는 모두 유사 배열 객체이면서 이터러블

- for ... of 문으로 순회할 수 있음
- 스프레드 문법을 사용하여 간단한 배열로 변환할 수 있음

**HTMLCollection**

- getElementsByTagName, getElementsByClassName 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체임
- `살아있는 객체`라고 부르기도 함

```html
<!DOCTYPE html>
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
<html>
	<body>
		<ul>
			<li id="apple">Apple</li>
			<li id="banana">Banana</li>
			<li id="orange">Orange</li>
		</ul>
		<script>
			// class 값이 'red'인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환
			const $elems = document.getElementsByClassName('red');
			// 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있음
			console.log($elem); // HTMLCollection(3) [li.red, li.red, li.red]

			// HTMLCollection 객체의 모든 요소의 class 값을 'blue'로 변경
			for (let i = 0; i < $elem.length; i++) {
				$elem[i].className = 'blue';
			}

			// HTMLCollection 객체의 요소가 3개에서 1개로 변경
			console.log($elems); // HTMLCollection(1) [li.red]
		</script>
	</body>
</html>
```

- 위의 예제의 의도는 모든 li 요소의 class 값이 'blue'로 변경되어 모든 li 요소는 CSS에 의해 파란색으로 렌더링되는 것
- 하지만 위 예제는 예상대로 동작하지 않음
- 두 번째 li 요소만 class 값이 변경되지 않음

다음과 같은 방법으로 문제를 해결할 수 있음

```js
// for 문을 역방향 순회
for (let i = $elems.length - 1; i >= 0; i--) {
	$elems[i].className = 'blue';
}

// while 문으로 HTMLCollection에 요소가 남아 있지 않을 때까지 무한 반복
let i = 0;
while ($elems.length > i) {
	$elems[i].className = 'blue';
}

// 유사 배열 객체이면서 이터러블인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach((elem) => (elem.className = 'blue'));
```

**NodeList**

- querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환
- NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체
- NodeList.prototype.forEach 메서드를 상속받아 사용 가능

childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작

```html
<!DOCTYPE html>
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
<html>
	<body>
		<ul id="fruits">
			<li>Apple</li>
			<li>Banana</li>
			<li>Orange</li>
		</ul>
		<script>
			const $fruits = document.getElementById('fruits');

			// childNodes 프로퍼티는 NodeList 객체(live)를 반환
			const { childNodes } = $fruits;
			console.log(childNodes instanceof NodeList); // true

			console.log($fruits); // NodeList(5) [text, li, text, li, text]

			for (let i = 0; i < childNodes.length; i++) {
				// 첫 번째, 세번 째, 다섯 번째 요소만 삭제됨
				$fruits.removeChild(childNodes[i]);
			}

			console.log(childNodes); // NodeList(2) [li, li]
		</script>
	</body>
</html>
```

👉 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장

## 39.3 노드 탐색

### 공백 텍스트 노드

> 💡 HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성

👉 노드를 탐색할 때 공백 문자가 생성한 공백 텍스트 노드에 주의해야 함

### 자식 노드 탐색

| 프로퍼티                            | 설명                                                                                                                                                               |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Node.prototype.childNodes           | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환. childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함될 수 있음      |
| Element.prototype.children          | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환. children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않음 |
| Node.prototype.firstChild           | 첫 번째 자식 노드를 반환. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드                                                                          |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환. lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드                                                                            |
| Element.prototype.firstElementChild | 첫 번째 자식 요소를 반환. firstElementChild 프로퍼티는 요소 노드만 반환                                                                                            |
| Element.prototype.lastElementChild  | 마지막 자식 요소 노드를 반환. lastElementChild 프로퍼티는 요소 노드만 반환                                                                                         |

### 자식 노드 존재 확인

> 💡 자식 노드가 존재하는지 확인하려면 Node.prototype.hasChildNodes 메서드 사용

- 텍스트 노드를 포함하여 자식 노드의 존재를 확인

```html
<!DOCTYPE html>
<html>
	<body>
		<ul id="fruits"></ul>
	</body>
	<script>
		const $fruits = document.getElementById('fruits');

		console.log($fruits.hasChildNodes()); // true
	</script>
</html>
```

👉 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 `children.length` 또는 `childElementCount` 프로퍼티를 사용

### 요소 노드의 텍스트 노드 탐색

> 💡 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근 가능

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello</div>
		<script>
			// 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근 가능
			console.log(document.getElementById('foo').firstChild); // #text
		</script>
	</body>
</html>
```

### 부모 노드 탐색

> 💡 부모 노드를 탐색하려면 Node.prototype.parentNode 프로퍼티를 사용

```html
<!DOCTYPE html>
<html>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 .banana 요소 노드를 취득
		const $banana = document.querySelector('.banana');

		// .banana 요소 노드의 부모 노드를 탐색
		console.log($banana.parentNode); // ul#fruits
	</script>
</html>
```

### 형제 노드 탐색

| 프로퍼티                              | 설명                                                                                                  |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Node.prototype.previousSibling        | 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환. 요소 노드 뿐만 아니라 텍스트 노드일 수도 있음 |
| Node.prototype.nextSibling            | 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환. 요소 노드뿐만 아니라 텍스트 노드일 수도 있음  |
| Node.prototype.previousElementSibling | 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환. 요소 노드만 반환                    |
| Node.prototype.nextElementSibling     | 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환. 요소 노드만 반환                    |

```html
<!DOCTYPE html>
<html>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 .banana 요소 노드를 취득
		const $fruits = document.getElementById('fruits');

		// 첫 번째 자식 노드 탐색
		// 요소 노드 뿐 아니라 텍스트 노드를 반환할 수도 있음
		const { firstChild } = $fruits;
		console.log(firstChild); // #text

		// 첫 번째 자식 노드의 다음 형제 노드를 탐색
		const { nextSibling } = firstChild;
		console.log(nextSibling); // li.apple

		// 이전 형제 노드 탐색
		const { previousSibling } = nextSibling;
		console.log(previousSibling); // #text

		// #fruits 요소의 첫 번째 자식 요소 노드 탐색
		const { firstElementChild } = $fruits;
		console.log(firstElementChild); // li.apple

		// #fruits 요소의 첫 번째 자식 요소 노드의 다음 형제 노드 탐색
		const { nextElementSibling } = firstElementChild;
		console.log(nextElementSibling); // li.banana

		// li.banana의 이전 형제 요소 노드 탐색
		const { previousElementSibling } = nextElementSibling;
		console.log(previousElementSibling); // li.applie
	</script>
</html>
```

## 39.4 노드 정보 취득

**Node.prototype.nodeType**

> 💡 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환, 노드 타입 상수는 Node에 정의되어 있음

- `Node.ELEMENT_NODE`: 요소 노드 타입을 나타내는 상수 1을 반환
- `Node.TEXT_NODE`: 텍스트 노드 타입을 나타내는 상수 3을 반환
- `Node.DOCUMENT_NODE`: 문서 노드 타입을 나타내는 상수 9를 반환

**Node.prototype.nodeName**

> 💡 노드의 이름을 문자열로 반환

- `요소 노드`: 대문자 문자열로 태그이름 ("UL", "LI" 등)을 반환
- `텍스트 노드`: 문자열 노드: 문자열 "#text"를 반환
- `문서 노드`: 문자열 "#document"를 반환

## 39.5 요소 노드의 텍스트 조작

### nodeValue

> 💡 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환

- Node.prototype.nodeValue 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello</div>
	</body>
	<script>
		// 문서 노드의 nodeValue 프로퍼티를 참조
		console.log(document.nodeValue); // null

		// 요소 노드의 nodeValue 프로퍼티를 참조
		const $foo = document.getElementById('foo');
		console.log($foo.nodeValue); // null

		// 텍스트 노드의 nodeValue 프로퍼티 참조
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeValue); // Hello
	</script>
</html>
```

- 텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드의 값을 반환
- 텍스트 노드가 아닌 객체의 nodeValue 프로퍼티를 참조하면 null을 반환

요소 노드의 텍스트를 변경하려면 다음과 같은 순서의 처리가 필요함

1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색함. 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색함
2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경

```js
const $textNode = document.getElementById('foo').firstChild;

$textNode.nodeValue = 'World';

console.log($textNode.nodeValue); // World
```

### textContent

> 💡 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환

- Node.prototype.textContent 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello <span>world!</span></div>
	</body>
	<script>
		// #foo 요소 노드의 텍스트를 모두 취득
		console.log(document.getElementById('foo').textContent); // Hello world!
	</script>
</html>
```

textContent 프로퍼티와 유사한 동작을 하는 `innerText` 프로퍼티가 있음
innerText 프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋음

- innerText 프로퍼티 CSS에 순종적
  - CSS에 의해 비표시(visibility: hidden;)로 지정된 요소 노드의 텍스트를 반환하지 않음
- innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느림

## 39.6 DOM 조작

## 39.7 어트리뷰트

## 39.8 스타일

## 39.9 DOM 표준

> 💡 W3C과 WHATWG이라는 두 단체에서 공통된 표준을 만듦

DOM은 현재 다음과 가티 4개의 버전이 있음

| 레벨        | 표준 문서 URL                          |
| ----------- | -------------------------------------- |
| DOM Level 1 | https://www.w3.org/TR/REC-DOM-Level-1  |
| DOM Level 2 | https://www.w3.org/TR/DOM-Level-2-Core |
| DOM Level 3 | https://www.w3.org/TR/DOM-Level-3-Core |
| DOM Level 4 | https://dom.spec.whatwg.org            |

### 참고

- https://yung-developer.tistory.com/74
- https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html
- https://velog.io/@hangem422/js-web-node
