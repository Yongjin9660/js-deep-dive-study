# 39. DOM

> 💡 DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API

## 39.1 노드

### HTML 요소와 노드 객체

![image](https://user-images.githubusercontent.com/55246584/155140963-b464c1d4-0f8c-49ba-b8cb-62a49f7d32ec.png)

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM에 구성하는 요소 노드 객체로 변환됨

### 노드 객체의 타입

### 노드 객체의 상속 구조

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

## 39.3 노드 탐색

## 39.4 노드 정보 취득

## 39.5 요소 노드의 텍스트 조작

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
