# 39. DOM

> 💡 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조

<br>

## 39.1 노드

#### 1) HTML 요소와 노드 객체

> HTML 요소는 HTML 문서를 구성하는 개별적인 요소 의미

<br>

<img style="height:250px;" src="https://media.vlpt.us/images/hangem422/post/9565d686-2059-473b-a98b-8445876df26a/web-node01.png">

<br>

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM 을 구성하는 `요소 노드 객체`로 변환됨.
  - `HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환됨.`
    <br>
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 `중첩 관계`를 가짐.

<br>

#### \*\* **트리 자료구조**

![img](https://gmlwjd9405.github.io/images/data-structure-tree/tree-terms.png)
<br>

- 트리 자료구조는 **노드들의 계층 구조**로 이루어짐.

  - 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 (부자, 형제 관계) 구조를 표현하는 비선형 자료구조
    <br>

- 트리 자료구조는 **하나의 최상위 노드에서 시작**

  - 최상위 노드는 부모 노드가 없으며, `루트 노드`라고도 불림.
  - 루트 노드는 0개 이상의 자식 노드를 가지며, 자식 노드가 없는 노드를 `리프 노드`라고 함.
    <br>

- **노드 객체들로 구성된 트리 자료구조를 DOM** 이라고 하며, 노드 객체의 트리로 구조화되어 있기 때문에 DOM 을 `DOM 트리`라고 부르기도 함.
  <br>

#### \*\* **노드 객체의 타입**

```jsx
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" htef="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
    <script src="app.js"></script>
</html>
```

- 렌더링 엔진이 위 HTML 문서를 파싱한 경우 다음과 같은 DOM 생성

<br>

![image](https://user-images.githubusercontent.com/77706631/155495328-002cecc1-75bf-4c28-9693-dcd400ffbb83.png)
<br>

- DOM 은 노드 객체의 계층적인 구조로 구성
- 노드 객체는 총 12개의 종류가 있으며 상속 구조를 가짐.
  <br>

#### 1) 문서 노드

- **`DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴`**
  <br>
- HTML 문서당 document 객체는 유일함.

  - script 태그에 의해 자바스크립트 코드가 분리되어 있어도 하나의 전역 객체 window 를 공유
    => window 의 document 프로퍼티에 바인딩되어 있는 하나의 document 객체를 바라봄.
    <br>

- 문서 노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 `DOM 트리의 노드들에 접근하기 위한 진입점 역할`
  <br>

##### \*\* **`document 객체란?`**

- 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체
- 전역 객체 window 의 document 프로퍼티에 바인딩되어 있음.
- 문서 노드는 window.document 또는 document 로 참조 가능
  <br>

#### 2) 요소 노드

- **`HTML 요소를 가리키는 객체`**
  <br>

- HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화함.
  => 요소 노드는 문서의 구조를 표현

<br>

#### 3) 어트리뷰트 노드

- **`HTML 요소의 어트리뷰트를 가리키는 객체`**
  <br>

- 부모 노드와 연결되어 있지 않고, `어트리뷰트가 지정된 HTML 요소의 요소 노드와만 연결되어 있음.`
  - 부모 노드가 없으므로 요소 노드의 형제 노드 X
  - 어트리뷰트 노드에 접근하여 어트리뷰트를 참조/변경하려면 먼저 요소 노드에 접근해야 함.
    <br>

#### 4) 텍스트 노드

- **`HTML 요소의 텍스트를 가리키는 객체`**
  <br>

- `요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드`
  => 텍스트 노드는 DOM 트리의 최종단이기 때문에 텍스트 노드에 접근하려면 부모 노드인 요소 노드에 먼저 접근해야 함.

<br>

#### \*\* **노드 객체의 상속 구조**

- DOM 을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 `DOM API` 사용 가능
- 이를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수 있게 됨.
  => `DOM API 를 통해 HTML 구조나 내용 또는 스타일 등을 동적으로 조작 가능`

<br>

![img](https://media.vlpt.us/images/hangem422/post/ab228c20-bf12-4046-8ba4-e460492800c0/web-node04.png)

<br>

- **모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받음**

<br>

<img src="https://media.vlpt.us/images/lechuck/post/e0091ac9-ff9b-48bc-901c-3423727185f7/image.png">

<br><br>

| input 요소 노드 객체의 특성                                                | 프로토타입을 제공하는 객체 |
| -------------------------------------------------------------------------- | -------------------------- |
| 객체                                                                       | Object                     |
| 이벤트를 발생시키는 객체                                                   | EventTarget                |
| 트리 자료구조의 노드 객체                                                  | Node                       |
| 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element                    |
| 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체                            | HTMLElement                |
| HTML 요소 중에서 input 요소를 표현하는 객체                                | HTMLInputElement           |

<br>

- input 요소를 파싱하여 객체화한 input 요소 노드 객체는 프로토타입 체인 안에 있는 모든 프로토타입의 프로퍼티/메서드 사용 가능

- 노드 객체의 상속 구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인 가능

<br>

## 39.2 요소 노드 취득

- HTML 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 함.

<br>

#### 1) id 를 이용한 요소 노드 취득

- `Document.prototype.getElementById` 메서드는 인수로 전달한 id 값을 갖는 하나의 요소 노드를 탐색하여 반환
  - getElementById 메서드는 Document.prototype 의 프로퍼티이므로 반드시 document 를 통해 호출해야 함.

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul>
			<li id="apple">Apple</li>
			<li id="apple">Banana</li>
			<li id="orange">Orange</li>
		</ul>
	</body>
	<script>
		// getElementById 메서드는 언제나 단 하나의 요소 노드를 반환
		// 첫 번째 li 요소가 파싱되어 생성된 요소 노드가 반환됨.
		const $elem = document.getElementById("apple");

		// 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
		$elem.style.color = "red";

		// id 값이 'grape' 인 요소 노드를 탐색하여 반환한다. -> null 반환
		const $elem = document.getElementById("grape");
		$elem.style.color = "red"; // TypeError
	</script>
</html>
```

- `id 값은 HTML 문서 내에서 유일한 값`이어야 하며, class 어트리뷰트와는 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없음.
  <br>

- `getElementById 메서드는 언제나 단 하나의 요소 노드만을 반환`

  - 중복된 id 값을 갖는 HTML 요소가 여러 개 존재하더라도 에러 X
  - getElementById 메서드는 인수로 전달된 id 값을 갖는 첫 번째 요소 노드만 반환
    <br>

- 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 getElementById 메서드는 null 을 반환
  <br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<div id="foo"></div>
	</body>
	<script>
		// id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당된다.
		console.log(foo === document.getElementById("foo")); // true

		// 암묵적 전역으로 생성된 전역 프로퍼티는 삭제되지만 전역 변수는 삭제되지 않는다.
		delete foo;
		console.log(foo); // <div id="foo"></div>
	</script>
</html>
```

- `HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과 O`
  <br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<div id="foo"></div>
	</body>
	<script>
		let foo = 1;

		// id 값과 동일한 이름의 전역 변수가 이미 선언되어 있으며 노드 객체가 재할당되지 않는다.
		console.log(foo); // 1
	</script>
	x
</html>
```

- 단, id 값과 동일한 이름의 전역 변수가 이미 선언되어 있다면 이 전역 변수에 노드 객체가 재할당되지 않음.
  <br>

#### 2) 태그 이름을 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByTagName` 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
  <br>
- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환
  - `HTMLCollection 객체는 유사 배열 객체이면서 이터러블`

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul>
			<li id="apple">Apple</li>
			<li id="banana">Banana</li>
			<li id="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 태그 이름이 li 인 요소 노드를 모두 탐색하여 반환한다.
		// 탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
		const $elems = document.getElementByTagName("li");

		// 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
		// HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
		[...$elems].forEach((elem) => {
			elem.style.color = "red";
		});

		// 모든 요소 노드를 탐색하여 반환한다.
		const $all = document.getElementByTagName("*");
		// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script,apple: li#apple, banana: li#banana, orange: li#orange]
	</script>
</html>
```

- HTML 문서의 모든 노드 요소 노드를 취득하려면 getElementByTagName 메서드의 인수로'\*' 를 전달
  <br>
- 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체를 반환
  <br>

#### 3) class 를 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 class 값을 갖는 모든 요소 노드들을 탐색하여 반환
  <br>

- getElementsByClassName 메서드는 여러 개의 요소 노드 객체를 갖는 `DOM 컬렉션 객체인 HTMLCollection 객체를 반환`

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul>
			<li class="fruit apple">Apple</li>
			<li class="fruit banana">Banana</li>
			<li class="fruit orange">Orange</li>
		</ul>
	</body>
	<script>
		// class 값이 'fruit' 인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
		const $elems = document.getElemetByClassName("fruit");

		// 취득한 모든 요소의 CSS color 프로퍼티 값을 변경한다.
		[...$elems].forEach((elem) => {
			elem.style.color = "red";
		});

		// class 값이 'fruit apple' 인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
		const $apples = document.getElementByClassName("fruit apple");

		// 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
		[...$apples].forEach((elem) => {
			elem.style.color = "blue";
		});
	</script>
</html>
```

- `인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class 지정 가능`
  <br>
- Element.prototype.getElementByClassName 메서드는 특정 요소 노드를 통해 호출하며 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
  <br>
- 인수로 전달된 class 값을 갖는 요소가 존재하지 않는 경우 getElementsByClassName 메서드는 빈 HTMLCollection 객체를 반환

<br>

#### 4) CSS 선택자를 이용한 요소 노드 취득

<br>

```js
/* 전체 선택자 : 모든 요소를 선택 */
* { ... }

/* 태그 선택자 : 모든 p 태그 요소를 모두 선택 */
p { ... }

/* id 선택자 : id 값이 'foo' 인 요소를 선택 */
#foo { ... }

/* class 선택자 : class 값이 'foo' 인 요소를 모두 선택 */
.foo { ... }

/* 어트리뷰트 선택자 : input 요소 중에 type 어트리뷰트 값이 'text' 인 요소를 모두 선택 */
input[type=text] { ... }

/* 후손 선택자 : div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }

/* 자식 선택자 : div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }

/* 인접 형제 선택자 : p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }

/* 일반 형제 선택자 : p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }

/* 가상 클래스 선택자 : hover 상태인 a 요소를 모두 선택 */
a:hover { ... }

/* 가상 요소 선택자 : p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
                      일반적으로 content 프로퍼티와 함께 사용된다. */
p::before { ... }
```

- CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul>
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// class 어트리뷰트 값이 'banana' 인 첫 번째 요소 노드를 탐색하여 반환한다.
		const $elem = document.querySelector(".banana");

		// 취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
		$elem.style.color = "red";
	</script>
</html>
```

- `Document.prototype/Element.prototype.querySelector` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환
  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번재 요소 노드만 반환
  - 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
  - 인수로 전달된 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul>
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// ul 요소의 자식 요소인 li 요소를 모두 탐색하여 반환한다.
		const $elems = document.querySelectorAll("ul > li");

		// 취득한 요소 노드들은 NodeList 객체에 담겨 반환된다.
		console.log($elems); // NodeList(3) [li.apple, li.banana, li.orange]

		// 취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
		// NodeList 는 forEach 메서드를 제공한다.
		$elems.forEach((elem) => {
			elem.style.color = "red";
		});
	</script>
</html>
```

- `Document.prototype/Element.prototype.querySelectorAll` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환
  <br>

- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `NodeList 객체를 반환`
- 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환

  - NodeList 객체는 `유사 배열 객체이면서 이터러블`
    <br>

- 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생
  <br>

- HTML 문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 \* 를 전달
  <br>

#### 5) 특정 요소 노드를 취득할 수 있는지 확인

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		const $apple = document.querySelector(".apple");

		// #apple 노드는 '#fruits > li.apple' 로 취득할 수 있다.
		console.log($apple.matches("#fruits > li.apple")); // true

		// #apple 노드는 '#fruits > li.banana' 로 취득할 수 없다.
		console.log($apple.matches("#fruits > li.banana")); // false
	</script>
</html>
```

- `Element.prototype.matches` 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는 지 확인
  <br>

#### 6) HTMLCollection 과 NodeList

- DOM 컬렉션 객체인 HTMLCollection 과 NodeList 는 **DOM API 가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체**
  <br>

- HTMLCollection 과 NodeList 모두 `유사 배열 객체이면서 이터러블`
  - for... of 문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단히 배열로 변환 가능
    <br>
- HTMLCollection 과 NodeList 는 **노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 객체** 라는 것
  - HTMLCollection 은 언제나 live 객체로 동작
  - NodeList 는 대부분의 경우 non-live 객체로 동작하지만 경우에 따라 종종 live 객체로 동작

<br>

##### HTML Collection

- getElementsByTagName, getElementsByClassName 메서드가 반환하는 `HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아있는 DOM 컬렉션 객체`

<br>

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
	</body>
	<script>
		// class 값이 'red' 인 요소 노드를 모두 탐색하여 HTMLCollection 객체에 담아 반환한다.
		const $elems = document.getElementByClassName(".red");

		// 이 시점에 HTMLCollection 객체에는 3개의 요소 노드가 담겨 있다.
		console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

		// HTMLCollection 객체의 모든 요소의 class 값을 'blue' 로 변경한다.
		for (let i = 0; i < $elems.length; i++) {
			$elems[i].className = "blue";
		}

		// HTMLCollection 객체의 요소가 3개에서 1개로 변경되었다.
		console.log($elems);
	</script>
</html>
```

- 위 예제를 실행해보면 두 번째 li 요소만 class 값이 변경되지 않음.
  - HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문

<br>

```js
// 유사 배열 객체이면서 이터러블인 HTMLCollection 을 배열로 변환하여 순회
[...$elems].forEach((elem) => (elem.className = "blue"));
```

- for 문을 역방향으로 순회하거나 위와 같이 while 문을 사용하면 올바르게 변경할 수 있음.
  <br>

#### NodeList

- `querySelectAll` 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환
  - NodeList 객체는 대부분의 경우 실시간으로 노드 객체의 상태 변경을 반영하지 않는 non-live 객체로 동작
  - `단, childNodes 프로퍼티가 반환하는 NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작`
    <br>

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
		const $fruits = document.getElementById('fruits');

		// childNodes 프로퍼티는 NodeList 객체 (live) 를 반환한다.
		  const { childNodes } = $fruits;
		  console.log(childNodes instanceof NodeList);    // true

		  // $fruits 요소의 자식 노드는 공백 텍스트 노드를 포함하여 모두 5개이다.
		  console.log(childNodes);/ /  NodeList(5) [text, li, text, li, text]

		  for (let i = 0; i < childNodes.length; i++) {
		    // removeChild 메서드는 $fruits 요소의 자식 노드를 DOM 에서 삭제한다.
		    // removeChild 메서드가 호출될 때마다 NodeList 객체인 childNodes 가 실시간으로 변경된다.
		    // 따라서 첫 번째, 세 번째, 다섯 번째 요소만 삭제한다.
		    $fruits.removeChild(childNodes[i]);
		  }

		  // 예상과 다르게 $fruits 요소의 모든 자식 노드가 삭제되지 않는다.
		  console.lgg(childNodes); // NodeList(2) [li, li]
	</script>
</html>
```

- 이처럼 HTMLCollection 이나 NodeList 객체는 예상과 다르게 동작할 때가 있어 다루기 까다롭고 실수하기 쉬움.
  <br>
- 따라서 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 **HTMLCollection 이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장**
  - `HTMLCollection 이나 NodeList 객체는 모두 유사 배열 객체이면서 이터러블`
  - `스프레드 문법이나 Array.from 메서드를 사용하여 간단히 배열로 변환 가능`

<br>

## 39.3 노드 탐색

- `노드 탐색 프로퍼티는 모두 접근자 프로퍼티`
  - setter 없이 getter 만 존재하여 참조만 가능한 `읽기 전용 접근자 프로퍼티`
  - 읽기 전용 프로퍼티에 값을 할당하면 아무런 에러 없이 무시됨.
    <br>

#### 1) 공백 텍스트 노드

- HTML 요소 사이의 스페이스, 탭, 줄바꿈 등의 공백 문자는 텍스트 노드를 생성
  - 이를 `공백 텍스트 노드` 라고 부름.

<br>

<img style="height:400px;" src="https://media.vlpt.us/images/ash3767/post/dfad1cfb-be19-45c9-8029-cd4a9d51a03a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-01%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.27.42.png">

<br>

- HTML 문서의 공백 문자는 공백 텍스트 노드를 생성하기 때문에 노드를 탐색할 때 주의해야 함.

<br>

#### 2) 자식 노드 탐색

##### \*\* **`자식 노드를 탐색할 때 사용하는 노드 탐색 프로퍼티`**

| 프로퍼티                            | 설명                                                                                                                                                                        |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.childNodes           | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList 에 담아 반환한다. childNodes 프로퍼티가 반환한 NodeList 에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다. |
| Element.prototype.children          | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection 에 담아 반환한다. children 프로퍼티가 반환한 HTMLCollection 에는 텍스트 노드가 포함되지 않는다. |
| Node.prototype.firstChild           | 첫 번째 자식 노드를 반환한다. firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                            |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환한다. lastChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드다.                                                                              |
| Element.prototype.firstElementChild | 첫 번째 자식 요소 노드를 반환한다. firstElementChild 프로퍼티는 요소 노드만 반환한다.                                                                                       |
| Element.prototype.lastElementChild  | 마지막 자식 요소 노드를 반환한다. lastElementChild 프로퍼티는 요소 노드만 반환한다.                                                                                         |

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
		const $fruits = document.getElementById("fruits");

		// #fruits 요소의 모든 자식 노드를 탐색한다.
		// childNodes 프로퍼티가 반환한 NodeList 에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있다.
		console.log($fruits.childNodes);
		// NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

		// #fruits 요소의 모든 자식 노드를 탐색한다.
		// childNodes 프로퍼티가 반환한 HTMLCollection 에는 요소 노드만 포함되어 있다.
		console.log($fruits.children);
		// HTMLCollection(3) [li.apple, li.banana, li.orange]

		// #fruits 요소의 첫 번째 자식 노드를 탐색한다.
		// firstChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
		console.log($fruits.firstChild); // #text

		// #fruits 요소의 마지막 자식 노드를 탐색한다.
		// firstChild 프로퍼티는 텍스트 노드를 반환할 수도 있다.
		console.log($fruits.lastChild); // #text

		// #fruits 요소의 첫 번째 자식 노드를 탐색한다.
		// firstElementChild 프로퍼티는 요소 노드만 반환한다.
		console.log($fruits.firstElementChild); // li.apple

		// #fruits 요소의 마지막 자식 노드를 탐색한다.
		// lastElementChild 프로퍼티는 요소 노드만 반환한다.
		console.log($fruits.lastElementChild); // li.orange
	</script>
</html>
```

<br>

#### 3) 자식 노드 존재 확인

- `Node.prototype.hasChildNodes` 메서드는 자식 노드가 존재하는지 확인하여 불리언 값 반환
- 단, hasChildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul id="fruits"></ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
		const $fruits = document.getElementById("fruits");

		// #fruits 요소에 자식 노드가 존재하는지 확인한다.
		// hasChildNodes 메서드는 텍스트 노드를 포함하여 자식 노드의 존재를 확인한다.
		console.log($fruits.hasChildNodes()); // true
	</script>
</html>
```

#### 4) 요소 노드의 텍스트 노드 탐색

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<div id="foo">Hello</div>
	</body>
	<script>
		// 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.
		console.log(document.getElementById("foo").firstChild); // #text
	</script>
</html>
```

- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드이므로 `firstChild` 프로퍼티로 접근 가능
  - firstChild 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드

<br>

#### 5) 부모 노드 탐색

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
		const $banana = document.querySelector(".banana");

		// .banana 요소 노드의 부모 노드를 탐색한다.
		console.log($banana.paraetNode); // ul#fruits
	</script>
</html>
```

- 부모 노드를 탐색하려면 `Node.prototype.parentNode` 프로퍼티 사용
- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우 X

<br>

#### 6) 형제 노드 탐색

##### \*\* **`부모 노드가 같은 형제 노드를 탐색 할 때 사용하는 노드 탐색 프로퍼티`**

| 프로퍼티                                  | 설명                                                                                                                                                                       |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.previousSibling            | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환한다. previousSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| Node.prototype.nextSibling                | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다. nextSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다.     |
| Element.prototype.previousElementsSibling | 부모 노드가 같은 형제 요소 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환한다. previousElementsSibling 프로퍼티는 요소 노드만 반환한다.                            |
| Element.prototype.nextElementsSibling     | 부모 노드가 같은 형제 요소 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환한다. nextElementsSibling 프로퍼티는 요소 노드만 반환한다.                                |

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<ul id="fruits">
			<li class="apple">Apple</li>
			<li class="banana">Banana</li>
			<li class="orange">Orange</li>
		</ul>
	</body>
	<script>
		// 노드 탐색의 기점이 되는 #fruits 요소 노드를 취득한다.
		const $fruits = document.ElementById("fruits");

		// #fruits 요소의 첫 번째 자식 노드를 탐색한다.
		// firstChild 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
		const { firstChild } = $fruits;
		console.log(firstChild); // #text

		// #fruits 요소의 첫 번째 자식 노드 (텍스트 노드) 의 다음 형제 노드를 탐색한다.
		// nextSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
		const { nextSibling } = firstChild;
		console.log(nextSibling); // li.apple

		// li.apple 요소의 이전 형제 노드를 탐색한다.
		// previousSibling 프로퍼티는 요소 노드뿐만 아니라 텍스트 노드를 반환할 수도 있다.
		const { previousSibling } = nextSibling;
		console.log(previousSibling); // #text

		// #fruits 요소의 첫 번째 자식 요소 노드를 탐색한다.
		// firstElementChild 프로퍼티는 요소 노드만 반환한다.
		const { firstElementChild } = $fruits;
		console.log(firstElementChild); // li.apple

		// #fruits 요소의 첫 번째 자식 요소 노드 (li.apple) 의 다음 형제 노드를 탐색한다.
		// nextElementSibling 프로퍼티는 요소 노드만 반환한다.
		const { nextElementSibling } = firstElementChild;
		console.log(nextElementSibling); // li.banana

		// li.banana 요소의 이전 형제 요소 노드를 탐색한다.
		// previousElementsSibling 프로퍼티는 요소 노드만 반환한다.
		const { previousElementsSibling } = nextElementSibling;
		console.log(previousElementsSibling); // li.apple
	</script>
</html>
```

<br>

## 39.4 노드 정보 취득

##### \*\* **`노드 객체에 대한 정보를 취득할 때 사용하는 노드 정보 프로퍼티`**

| 프로퍼티                | 설명                                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| Node.prototype.nodeType | 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 Node 에 정의되어 있다. |
|                         | - `Node.ELEMENT_NODE` : 요소 노드 타입을 나타내는 상수 1을 반환                                    |
|                         | - `Node.TEXT_NODE` : 텍스트 노드 타입을 나타내는 상수 3을 반환                                     |
|                         | - `Node.DOCUMENT_NODE` : 문서 노드 타입을 나타내는 상수 9를 반환                                   |
| Node.prototype.nodeName | 노드의 이름을 문자열로 반환한다.                                                                   |
|                         | - `요소 노드` : 대문자 문자열로 태그 이름("UL", "LI" 등) 을 반환                                   |
|                         | - `텍스트 노드` : 문자열 "#text" 를 반환                                                           |
|                         | - `문서 노드` : 문자열 "#document" 를 반환                                                         |

<br>

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<link rel="stylesheet" htef="style.css" />
	</head>
	<body>
		<div id="foo">Hello</div>
	</body>
	<script>
		// 문서 노드의 노드 정보를 취득한다.
		console.log(document.nodeType); // 9
		console.log(document.nodeName); // #document

		// 요소 노드의 노드 정보를 취득한다.
		const $foo = document.getElementById("foo");
		console.log($foo.nodeType); // 1
		console.log($foo.nodeName); // DIV

		// 텍스트 노드의 노드 정보를 취득한다.
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeType); // 3
		console.log($textNode.nodeName); // #text
	</script>
</html>
```

<br>

## 39.5 요소 노드의 텍스트 조작

<br><br>
\*\* 이미지 출처 : https://velog.io/@hangem422/js-web-node
https://velog.io/@lechuck/DOM-API
