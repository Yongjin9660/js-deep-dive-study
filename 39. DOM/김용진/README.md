# 39. DOM

> π‘ DOMμ HTML λ¬Έμμ κ³μΈ΅μ  κ΅¬μ‘°μ μ λ³΄λ₯Ό νννλ©° μ΄λ₯Ό μ μ΄ν  μ μλ API

## 39.1 λΈλ

### HTML μμμ λΈλ κ°μ²΄

![image](https://user-images.githubusercontent.com/55246584/155140963-b464c1d4-0f8c-49ba-b8cb-62a49f7d32ec.png)

π HTML μμλ λ λλ§ μμ§μ μν΄ νμ±λμ΄ DOMμ κ΅¬μ±νλ μμ λΈλ κ°μ²΄λ‘ λ³νλ¨

**νΈλ¦¬ μλ£κ΅¬μ‘°**

- λΈλλ€μ κ³μΈ΅ κ΅¬μ‘°λ‘ μ΄λ€μ§
- λΈλ κ°μ²΄λ€λ‘ κ΅¬μ±λ νΈλ¦¬ μλ£κ΅¬μ‘°λ₯Ό `DOM`μ΄λΌ ν¨

![image](https://user-images.githubusercontent.com/55246584/155319510-c3bd3f0b-51fa-4af5-8c04-971d4cc70046.png)

### λΈλ κ°μ²΄μ νμ

λΈλ κ°μ²΄λ μ΄ 12κ°μ μ’λ₯κ° μμ. μ΄ μ€μμ μ€μν λΈλ νμμ λ€μκ³Ό κ°μ΄ 4κ°μ§

**λ¬Έμ λΈλ**

- DOM νΈλ¦¬μ μ΅μμμ μ‘΄μ¬νλ λ£¨νΈ λΈλλ‘μ document κ°μ²΄λ₯Ό κ°λ¦¬ν΄
- document κ°μ²΄λ DOM νΈλ¦¬μ λ£¨νΈ λΈλμ΄λ―λ‘ DOM νΈλ¦¬μ λΈλλ€μ μ κ·ΌνκΈ° μν μ§μμ  μ­ν μ λ΄λΉν¨

**μμ λΈλ**

- HTML μμλ₯Ό κ°λ¦¬ν€λ κ°μ²΄
- λ¬Έμμ κ΅¬μ‘°λ₯Ό ννν¨

**μ΄νΈλ¦¬λ·°νΈ λΈλ**

- HTML μμμ μ΄νΈλ¦¬λ·°νΈλ₯Ό κ°λ¦¬ν€λ κ°μ²΄
- μ΄νΈλ¦¬λ·°νΈκ° μ§μ λ HTML μμμ μμ λΈλμ μ°κ²°λμ΄ μμ

**νμ€νΈ λΈλ**

- HTML μμμ νμ€νΈλ₯Ό κ°λ¦¬ν€λ κ°μ²΄
- λ¬Έμμ μ λ³΄λ₯Ό νν
- μμ λΈλλ₯Ό κ°μ§ μ μλ λ¦¬ν λΈλ

### λΈλ κ°μ²΄μ μμ κ΅¬μ‘°

> π‘ λΈλ κ°μ²΄λ μλ°μ€ν¬λ¦½νΈ κ°μ²΄μ΄λ―λ‘ νλ‘ν νμμ μν μμ κ΅¬μ‘°λ₯Ό κ°μ§

![image](https://user-images.githubusercontent.com/55246584/155320546-99fb667e-0834-4c6b-8b1a-aedf74e34867.png)

π μ΄λ₯Ό νλ‘ν νμ μ²΄μΈ κ΄μ μμ μ΄ν΄λ³΄λ©΄ input μμ λΈλ κ°μ²΄λ νλ‘ν νμ μ²΄μΈμ μλ λͺ¨λ  νλ‘ν νμμ νλ‘νΌν°λ λ©μλλ₯Ό μμλ°μ μ¬μ© κ°λ₯

![image](https://user-images.githubusercontent.com/55246584/155320895-71d4379f-e5ad-42b7-80f7-bf6427d2b5c8.png)

```html
<!DOCTYPE html>
<html>
	<body>
		<input type="text" />
		<script>
			const $input = document.querySelector("input");

			// input μμ λΈλ κ°μ²΄μ νλ‘ν νμ μ²΄μΈ
			console.log(
				Object.getPrototypeOf($input) === HTMLInputElement.prototype,
				Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
				Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
				Object.getPrototypeOf(Element.prototype) === Node.prototype,
				Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
				Object.getPrototypeOf(EventTarget.prototype) === Object.prototype
			); // λͺ¨λ true
		</script>
	</body>
</html>
```

| input μμ λΈλ κ°μ²΄μ νΉμ±                                                | νλ‘ν νμμ μ κ³΅νλ κ°μ²΄ |
| -------------------------------------------------------------------------- | -------------------------- |
| κ°μ²΄                                                                       | Object                     |
| μ΄λ²€νΈλ₯Ό λ°μμν€λ κ°μ²΄                                                   | EventTarget                |
| νΈλ¦¬ μλ£κ΅¬μ‘°μ λΈλ κ°μ²΄                                                  | Node                       |
| λΈλΌμ°μ κ° λ λλ§ν  μ μλ μΉ λ¬Έμμ μμ(HTML, XML, SVGλ₯Ό νννλ κ°μ²΄) | Element                    |
| μΉ λ¬Έμμ μμ μ€μμ HTML μμλ₯Ό νννλ κ°μ²΄                            | HTMLElement                |
| HTML μμ μ€μμ input μμλ₯Ό νννλ κ°μ²΄                                | HTMLInputElement           |

**DOM**

- HTML λ¬Έμμ κ³μΈ΅μ  κ΅¬μ‘°μ μ λ³΄λ₯Ό νν
- λΈλ νμμ λ°λΌ νμν κΈ°λ₯μ νλ‘νΌν°μ λ©μλμ μ§ν©μΈ DOM APIλ‘ μ κ³΅
- μ΄ DOM APIλ₯Ό ν΅ν΄ HTMLμ κ΅¬μ‘°λ λ΄μ© λλ μ€νμΌ λ±μ λμ μΌλ‘ μ‘°μ κ°λ₯

## 39.2 μμ λΈλ μ·¨λ

### idλ₯Ό μ΄μ©ν μμ λΈλ μ·¨λ

> π‘ Document.prototype.getElementById λ©μλλ μΈμλ‘ μ λ¬ν id μ΄νΈλ¦¬λ·°νΈ κ°μ κ°λ νλμ μμ λΈλλ₯Ό νμνμ¬ λ°ν

- μ€λ³΅λ id κ°μ κ°λ μμκ° μ¬λ¬ κ° μ‘΄μ¬νλ©΄ μ²« λ²μ§Έ μμ λΈλλ§ λ°ν
- μμκ° μ‘΄μ¬νμ§ μλλ€λ©΄ nullμ λ°ν

```js
const $elem = document.getElementById("banana");
```

- HTML μμμ id μ΄νΈλ¦¬λ·°νΈλ₯Ό λΆμ¬νλ©΄ id κ°κ³Ό λμΌν μ΄λ¦μ μ μ­ λ³μκ° μλ¬΅μ μΌλ‘ μ μΈλκ³  ν΄λΉ λΈλ κ°μ²΄κ° ν λΉλλ λΆμ ν¨κ³Όκ° μμ

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo"></div>
		<script>
			// id κ°κ³Ό λμΌν μ΄λ¦μ μ μ­ λ³μκ° μλ¬΅μ μΌλ‘ μ μΈλκ³  ν΄λΉ λΈλ κ°μ²΄κ° ν λΉ
			console.log(foo === document.getElementById("foo")); // true
		</script>
	</body>
</html>
```

### νκ·Έ μ΄λ¦μ μ΄μ©ν μμ λΈλ μ·¨λ

> π‘ Document.prototype/Element.prototype.getElementsByTagName λ©μλλ μΈμλ‘ μ λ¬ν νκ·Έ μ΄λ¦μ κ°λ λͺ¨λ  μμ λΈλλ€μ νμνμ¬ λ°ν

- DOM μ»¬λ μ κ°μ²΄μΈ HTMLCollection κ°μ²΄λ₯Ό λ°ν

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
			// νκ·Έ μ΄λ¦μ΄ liμΈ μμ λΈλλ₯Ό λͺ¨λ νμνμ¬ λ°ν
			// νμλ μμ λΈλλ€μ HTMLCollection κ°μ²΄μ λ΄κ²¨ λ°ν
			// HTMLCollection κ°μ²΄λ μ μ¬ λ°°μ΄ κ°μ²΄μ΄λ©΄μ μ΄ν°λ¬λΈ
			const $elems = document.getElementsByTagName("li");

			// HTMLCollection κ°μ²΄λ₯Ό λ°°μ΄λ‘ λ³ννμ¬ μννλ©° color νλ‘νΌν° κ°μ λ³κ²½
			[...$elems].forEach((elem) => {
				elem.style.color = "red";
			});
		</script>
	</body>
</html>
```

- HTMLCollection κ°μ²΄λ `μ μ¬ λ°°μ΄ κ°μ²΄μ΄λ©΄μ μ΄ν°λ¬λΈ`

**Document.prototype.getElementsByTagName**

- DOMμ λ£¨νΈ λΈλμΈ λ¬Έμ λΈλ, μ¦ documentλ₯Ό ν΅ν΄ νΈμΆ
- DOM μ μ²΄μμ μμ λΈλλ₯Ό νμνμ¬ λ°ν

**Element.prototype.getElementsByTagName**

- νΉμ  μμ λΈλλ₯Ό ν΅ν΄ νΈμΆ
- νΉμ  μμ λΈλμ μμ λΈλ μ€μμ μμ λΈλλ₯Ό νμνμ¬ λ°ν

### classλ₯Ό μ΄μ©ν μμ λΈλ μ·¨λ

> π‘ Document.prototype/Element.prototype.getElementsByClassName λ©μλλ μΈμλ‘ μ λ¬ν class μ΄νΈλ¦¬λ·°νΈ κ°μ κ°λ λͺ¨λ  μμ λΈλλ€μ νμνμ¬ λ°ν

- DOM μ»¬λ μ κ°μ²΄μΈ HTMLCollection κ°μ²΄λ₯Ό λ°ν

### CSS μ νμλ₯Ό μ΄μ©ν μμ λΈλ μ·¨λ

> π‘ CSS μ νμλ μ€νμΌμ μ μ©νκ³ μ νλ HTML μμλ₯Ό νΉμ ν  λ μ¬μ©νλ λ¬Έλ²

```js
/* μ μ²΄ μ νμ: λͺ¨λ  μμλ₯Ό μ ν */
* { ... }

/* νκ·Έ μ νμ: λͺ¨λ  p νκ·Έ μμλ₯Ό λͺ¨λ μ ν */
p { ... }

/* id μ νμ: id κ°μ΄ 'foo'μΈ μμλ₯Ό λͺ¨λ μ ν */
#foo { ... }

/* class μ νμ: class κ°μ΄ 'foo'μΈ μμλ₯Ό λͺ¨λ μ ν */
.foo { ... }

/* μ΄νΈλ¦¬λ·°νΈ μ νμ: input μμ μ€μ type μ΄νΈλ¦¬λ·°νΈ κ°μ΄ 'text'μΈ μμλ₯Ό λͺ¨λ μ ν */
input[type=text] { ... }

/* νμ μ νμ: div μμμ μμ μμ μ€ p μμλ₯Ό λͺ¨λ μ ν */
div p { ... }

/* μμ μ νμ: div μμμ μμ μμ μ€ p μμλ₯Ό λͺ¨λ μ ν */
div > p { ... }

/* μΈμ  νμ  μ νμ: p μμμ νμ  μμ μ€μ p μμ λ°λ‘ λ€μ μμΉνλ ul μμλ₯Ό μ ν */
p + ul { ... }

/* μΌλ° νμ  μ νμ: p μμμ νμ  μμ μ€μ p μμ λ€μ μμΉνλ ul μμλ₯Ό λͺ¨λ μ ν */
p ~ ul { ... }

/* κ°μ ν΄λμ€ μ νμ: hover μνμΈ a μμλ₯Ό λͺ¨λ μ ν */
a:hover { ... }

/* κ°μ μμ μ νμ: p μμμ μ½νμΈ μ μμ μμΉνλ κ³΅κ°μ μ ν
  μΌλ°μ μΌλ‘ content νλ‘νΌν°μ ν¨κ» μ¬μ©λ¨ */
p::before { ... }
```

**Document.prototype/Element.prototype.querySelector**

- μΈμλ‘ μ λ¬λ CSS μ νμλ₯Ό λ§μ‘±μν€λ μμ λΈλκ° μ¬λ¬ κ°μΈ κ²½μ° μ²« λ²μ§Έ μμ λΈλλ§ λ°ν
- μΈμλ‘ μ λ¬λ CSS μ νμλ₯Ό λ§μ‘±μν€λ μμ λΈλκ° μ‘΄μ¬νμ§ μλ κ²½μ° null λ°ν
- μΈμλ‘ μ λ¬ν CSS μ νμκ° λ¬Έλ²μ λ§μ§ μλ κ²½μ° DOMException μλ¬κ° λ°μ

**Document.prototype/Element.prototype.querySelectorAll**

- μΈμλ‘ μ λ¬λ CSS μ νμλ₯Ό λ§μ‘±μν€λ μμκ° μ‘΄μ¬νμ§ μλ κ²½μ° λΉ `NodeList κ°μ²΄`λ₯Ό λ°ν
- μΈμλ‘ μ λ¬ν CSS μ νμκ° λ¬Έλ²μ λ§μ§ μλ κ²½μ° DOMException μλ¬κ° λ°μ

`NodeList κ°μ²΄`: μ μ¬ λ°°μ΄ κ°μ²΄μ΄λ©΄μ μ΄ν°λ¬λΈ

### νΉμ  μμ λΈλλ₯Ό μ·¨λν  μ μλμ§ νμΈ

> π‘ Element.prototype.matches λ©μλλ μΈμλ‘ μ λ¬ν CSS μ νμλ₯Ό ν΅ν΄ νΉμ  μμ λΈλλ₯Ό μ·¨λν  μ μλμ§ νμΈ

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
			const $apple = document.querySelector(".apple");

			// $apple λΈλλ '#fruits > li.apple'λ‘ μ·¨λν  μ μμ
			console.log($apple.matches("#fruits > li.apple")); // true

			// $apple λΈλλ '#fruits > li.banana'λ‘ μ·¨λν  μ μμ
			console.log($apple.matches("#fruits > li.banana"));
		</script>
	</body>
</html>
```

### HTMLCollectionκ³Ό NodeList

> π‘ DOM μ»¬λ μ κ°μ²΄μΈ HTMLCollectionκ³Ό NodeListsms DOM APIκ° μ¬λ¬ κ°μ κ²°κ³Όκ°μ λ°ννκΈ° μν DOM μ»¬λ μ κ°μ²΄

HTMLCollectionκ³Ό NodeListλ λͺ¨λ μ μ¬ λ°°μ΄ κ°μ²΄μ΄λ©΄μ μ΄ν°λ¬λΈ

- for ... of λ¬ΈμΌλ‘ μνν  μ μμ
- μ€νλ λ λ¬Έλ²μ μ¬μ©νμ¬ κ°λ¨ν λ°°μ΄λ‘ λ³νν  μ μμ

**HTMLCollection**

- getElementsByTagName, getElementsByClassName λ©μλκ° λ°ννλ HTMLCollection κ°μ²΄λ λΈλ κ°μ²΄μ μν λ³νλ₯Ό μ€μκ°μΌλ‘ λ°μνλ μ΄μ μλ DOM μ»¬λ μ κ°μ²΄μ
- `μ΄μμλ κ°μ²΄`λΌκ³  λΆλ₯΄κΈ°λ ν¨

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
			// class κ°μ΄ 'red'μΈ μμ λΈλλ₯Ό λͺ¨λ νμνμ¬ HTMLCollection κ°μ²΄μ λ΄μ λ°ν
			const $elems = document.getElementsByClassName("red");
			// μ΄ μμ μ HTMLCollection κ°μ²΄μλ 3κ°μ μμ λΈλκ° λ΄κ²¨ μμ
			console.log($elem); // HTMLCollection(3) [li.red, li.red, li.red]

			// HTMLCollection κ°μ²΄μ λͺ¨λ  μμμ class κ°μ 'blue'λ‘ λ³κ²½
			for (let i = 0; i < $elem.length; i++) {
				$elem[i].className = "blue";
			}

			// HTMLCollection κ°μ²΄μ μμκ° 3κ°μμ 1κ°λ‘ λ³κ²½
			console.log($elems); // HTMLCollection(1) [li.red]
		</script>
	</body>
</html>
```

- μμ μμ μ μλλ λͺ¨λ  li μμμ class κ°μ΄ 'blue'λ‘ λ³κ²½λμ΄ λͺ¨λ  li μμλ CSSμ μν΄ νλμμΌλ‘ λ λλ§λλ κ²
- νμ§λ§ μ μμ λ μμλλ‘ λμνμ§ μμ
- λ λ²μ§Έ li μμλ§ class κ°μ΄ λ³κ²½λμ§ μμ

λ€μκ³Ό κ°μ λ°©λ²μΌλ‘ λ¬Έμ λ₯Ό ν΄κ²°ν  μ μμ

```js
// for λ¬Έμ μ­λ°©ν₯ μν
for (let i = $elems.length - 1; i >= 0; i--) {
	$elems[i].className = "blue";
}

// while λ¬ΈμΌλ‘ HTMLCollectionμ μμκ° λ¨μ μμ§ μμ λκΉμ§ λ¬΄ν λ°λ³΅
let i = 0;
while ($elems.length > i) {
	$elems[i].className = "blue";
}

// μ μ¬ λ°°μ΄ κ°μ²΄μ΄λ©΄μ μ΄ν°λ¬λΈμΈ HTMLCollectionμ λ°°μ΄λ‘ λ³ννμ¬ μν
[...$elems].forEach((elem) => (elem.className = "blue"));
```

**NodeList**

- querySelectorAll λ©μλλ DOM μ»¬λ μ κ°μ²΄μΈ NodeList κ°μ²΄λ₯Ό λ°ν
- NodeList κ°μ²΄λ μ€μκ°μΌλ‘ λΈλ κ°μ²΄μ μν λ³κ²½μ λ°μνμ§ μλ κ°μ²΄
- NodeList.prototype.forEach λ©μλλ₯Ό μμλ°μ μ¬μ© κ°λ₯

childNodes νλ‘νΌν°κ° λ°ννλ NodeList κ°μ²΄λ HTMLCollection κ°μ²΄μ κ°μ΄ μ€μκ°μΌλ‘ λΈλ κ°μ²΄μ μν λ³κ²½μ λ°μνλ live κ°μ²΄λ‘ λμ

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
			const $fruits = document.getElementById("fruits");

			// childNodes νλ‘νΌν°λ NodeList κ°μ²΄(live)λ₯Ό λ°ν
			const { childNodes } = $fruits;
			console.log(childNodes instanceof NodeList); // true

			console.log($fruits); // NodeList(5) [text, li, text, li, text]

			for (let i = 0; i < childNodes.length; i++) {
				// μ²« λ²μ§Έ, μΈλ² μ§Έ, λ€μ― λ²μ§Έ μμλ§ μ­μ λ¨
				$fruits.removeChild(childNodes[i]);
			}

			console.log(childNodes); // NodeList(2) [li, li]
		</script>
	</body>
</html>
```

π λΈλ κ°μ²΄μ μν λ³κ²½κ³Ό μκ΄μμ΄ μμ νκ² DOM μ»¬λ μμ μ¬μ©νλ €λ©΄ HTMLCollectionμ΄λ NodeList κ°μ²΄λ₯Ό λ°°μ΄λ‘ λ³ννμ¬ μ¬μ©νλ κ²μ κΆμ₯

## 39.3 λΈλ νμ

### κ³΅λ°± νμ€νΈ λΈλ

> π‘ HTML μμ μ¬μ΄μ μ€νμ΄μ€, ν­, μ€λ°κΏ(κ°ν) λ±μ κ³΅λ°± λ¬Έμλ νμ€νΈ λΈλλ₯Ό μμ±

π λΈλλ₯Ό νμν  λ κ³΅λ°± λ¬Έμκ° μμ±ν κ³΅λ°± νμ€νΈ λΈλμ μ£Όμν΄μΌ ν¨

### μμ λΈλ νμ

| νλ‘νΌν°                            | μ€λͺ                                                                                                                                                               |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Node.prototype.childNodes           | μμ λΈλλ₯Ό λͺ¨λ νμνμ¬ DOM μ»¬λ μ κ°μ²΄μΈ NodeListμ λ΄μ λ°ν. childNodes νλ‘νΌν°κ° λ°νν NodeListμλ μμ λΈλλΏλ§ μλλΌ νμ€νΈ λΈλλ ν¬ν¨λ  μ μμ      |
| Element.prototype.children          | μμ λΈλ μ€μμ μμ λΈλλ§ λͺ¨λ νμνμ¬ DOM μ»¬λ μ κ°μ²΄μΈ HTMLCollectionμ λ΄μ λ°ν. children νλ‘νΌν°κ° λ°νν HTMLCollectionμλ νμ€νΈ λΈλκ° ν¬ν¨λμ§ μμ |
| Node.prototype.firstChild           | μ²« λ²μ§Έ μμ λΈλλ₯Ό λ°ν. firstChild νλ‘νΌν°κ° λ°νν λΈλλ νμ€νΈ λΈλμ΄κ±°λ μμ λΈλ                                                                          |
| Node.prototype.lastChild            | λ§μ§λ§ μμ λΈλλ₯Ό λ°ν. lastChild νλ‘νΌν°κ° λ°νν λΈλλ νμ€νΈ λΈλμ΄κ±°λ μμ λΈλ                                                                            |
| Element.prototype.firstElementChild | μ²« λ²μ§Έ μμ μμλ₯Ό λ°ν. firstElementChild νλ‘νΌν°λ μμ λΈλλ§ λ°ν                                                                                            |
| Element.prototype.lastElementChild  | λ§μ§λ§ μμ μμ λΈλλ₯Ό λ°ν. lastElementChild νλ‘νΌν°λ μμ λΈλλ§ λ°ν                                                                                         |

### μμ λΈλ μ‘΄μ¬ νμΈ

> π‘ μμ λΈλκ° μ‘΄μ¬νλμ§ νμΈνλ €λ©΄ Node.prototype.hasChildNodes λ©μλ μ¬μ©

- νμ€νΈ λΈλλ₯Ό ν¬ν¨νμ¬ μμ λΈλμ μ‘΄μ¬λ₯Ό νμΈ

```html
<!DOCTYPE html>
<html>
	<body>
		<ul id="fruits"></ul>
	</body>
	<script>
		const $fruits = document.getElementById("fruits");

		console.log($fruits.hasChildNodes()); // true
	</script>
</html>
```

π μμ λΈλ μ€μ νμ€νΈ λΈλκ° μλ μμ λΈλκ° μ‘΄μ¬νλμ§ νμΈνλ €λ©΄ `children.length` λλ `childElementCount` νλ‘νΌν°λ₯Ό μ¬μ©

### μμ λΈλμ νμ€νΈ λΈλ νμ

> π‘ μμ λΈλμ νμ€νΈ λΈλλ firstChild νλ‘νΌν°λ‘ μ κ·Ό κ°λ₯

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello</div>
		<script>
			// μμ λΈλμ νμ€νΈ λΈλλ firstChild νλ‘νΌν°λ‘ μ κ·Ό κ°λ₯
			console.log(document.getElementById("foo").firstChild); // #text
		</script>
	</body>
</html>
```

### λΆλͺ¨ λΈλ νμ

> π‘ λΆλͺ¨ λΈλλ₯Ό νμνλ €λ©΄ Node.prototype.parentNode νλ‘νΌν°λ₯Ό μ¬μ©

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
		// λΈλ νμμ κΈ°μ μ΄ λλ .banana μμ λΈλλ₯Ό μ·¨λ
		const $banana = document.querySelector(".banana");

		// .banana μμ λΈλμ λΆλͺ¨ λΈλλ₯Ό νμ
		console.log($banana.parentNode); // ul#fruits
	</script>
</html>
```

### νμ  λΈλ νμ

| νλ‘νΌν°                              | μ€λͺ                                                                                                  |
| ------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Node.prototype.previousSibling        | νμ  λΈλ μ€μμ μμ μ μ΄μ  νμ  λΈλλ₯Ό νμνμ¬ λ°ν. μμ λΈλ λΏλ§ μλλΌ νμ€νΈ λΈλμΌ μλ μμ |
| Node.prototype.nextSibling            | νμ  λΈλ μ€μμ μμ μ λ€μ νμ  λΈλλ₯Ό νμνμ¬ λ°ν. μμ λΈλλΏλ§ μλλΌ νμ€νΈ λΈλμΌ μλ μμ  |
| Node.prototype.previousElementSibling | νμ  μμ λΈλ μ€μμ μμ μ μ΄μ  νμ  μμ λΈλλ₯Ό νμνμ¬ λ°ν. μμ λΈλλ§ λ°ν                    |
| Node.prototype.nextElementSibling     | νμ  μμ λΈλ μ€μμ μμ μ λ€μ νμ  μμ λΈλλ₯Ό νμνμ¬ λ°ν. μμ λΈλλ§ λ°ν                    |

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
		// λΈλ νμμ κΈ°μ μ΄ λλ .banana μμ λΈλλ₯Ό μ·¨λ
		const $fruits = document.getElementById("fruits");

		// μ²« λ²μ§Έ μμ λΈλ νμ
		// μμ λΈλ λΏ μλλΌ νμ€νΈ λΈλλ₯Ό λ°νν  μλ μμ
		const { firstChild } = $fruits;
		console.log(firstChild); // #text

		// μ²« λ²μ§Έ μμ λΈλμ λ€μ νμ  λΈλλ₯Ό νμ
		const { nextSibling } = firstChild;
		console.log(nextSibling); // li.apple

		// μ΄μ  νμ  λΈλ νμ
		const { previousSibling } = nextSibling;
		console.log(previousSibling); // #text

		// #fruits μμμ μ²« λ²μ§Έ μμ μμ λΈλ νμ
		const { firstElementChild } = $fruits;
		console.log(firstElementChild); // li.apple

		// #fruits μμμ μ²« λ²μ§Έ μμ μμ λΈλμ λ€μ νμ  λΈλ νμ
		const { nextElementSibling } = firstElementChild;
		console.log(nextElementSibling); // li.banana

		// li.bananaμ μ΄μ  νμ  μμ λΈλ νμ
		const { previousElementSibling } = nextElementSibling;
		console.log(previousElementSibling); // li.applie
	</script>
</html>
```

## 39.4 λΈλ μ λ³΄ μ·¨λ

**Node.prototype.nodeType**

> π‘ λΈλ κ°μ²΄μ μ’λ₯, μ¦ λΈλ νμμ λνλ΄λ μμλ₯Ό λ°ν, λΈλ νμ μμλ Nodeμ μ μλμ΄ μμ

- `Node.ELEMENT_NODE`: μμ λΈλ νμμ λνλ΄λ μμ 1μ λ°ν
- `Node.TEXT_NODE`: νμ€νΈ λΈλ νμμ λνλ΄λ μμ 3μ λ°ν
- `Node.DOCUMENT_NODE`: λ¬Έμ λΈλ νμμ λνλ΄λ μμ 9λ₯Ό λ°ν

**Node.prototype.nodeName**

> π‘ λΈλμ μ΄λ¦μ λ¬Έμμ΄λ‘ λ°ν

- `μμ λΈλ`: λλ¬Έμ λ¬Έμμ΄λ‘ νκ·Έμ΄λ¦ ("UL", "LI" λ±)μ λ°ν
- `νμ€νΈ λΈλ`: λ¬Έμμ΄ λΈλ: λ¬Έμμ΄ "#text"λ₯Ό λ°ν
- `λ¬Έμ λΈλ`: λ¬Έμμ΄ "#document"λ₯Ό λ°ν

## 39.5 μμ λΈλμ νμ€νΈ μ‘°μ

### nodeValue

> π‘ λΈλ κ°μ²΄μ nodeValue νλ‘νΌν°λ₯Ό μ°Έμ‘°νλ©΄ λΈλ κ°μ²΄μ κ°μ λ°ν

- Node.prototype.nodeValue νλ‘νΌν°λ setterμ getter λͺ¨λ μ‘΄μ¬νλ μ κ·Όμ νλ‘νΌν°

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello</div>
	</body>
	<script>
		// λ¬Έμ λΈλμ nodeValue νλ‘νΌν°λ₯Ό μ°Έμ‘°
		console.log(document.nodeValue); // null

		// μμ λΈλμ nodeValue νλ‘νΌν°λ₯Ό μ°Έμ‘°
		const $foo = document.getElementById("foo");
		console.log($foo.nodeValue); // null

		// νμ€νΈ λΈλμ nodeValue νλ‘νΌν° μ°Έμ‘°
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeValue); // Hello
	</script>
</html>
```

- νμ€νΈ λΈλμ nodeValue νλ‘νΌν°λ₯Ό μ°Έμ‘°ν  λλ§ νμ€νΈ λΈλμ κ°μ λ°ν
- νμ€νΈ λΈλκ° μλ κ°μ²΄μ nodeValue νλ‘νΌν°λ₯Ό μ°Έμ‘°νλ©΄ nullμ λ°ν

μμ λΈλμ νμ€νΈλ₯Ό λ³κ²½νλ €λ©΄ λ€μκ³Ό κ°μ μμμ μ²λ¦¬κ° νμν¨

1. νμ€νΈλ₯Ό λ³κ²½ν  μμ λΈλλ₯Ό μ·¨λν λ€μ, μ·¨λν μμ λΈλμ νμ€νΈ λΈλλ₯Ό νμν¨. νμ€νΈ λΈλλ μμ λΈλμ μμ λΈλμ΄λ―λ‘ firstChild νλ‘νΌν°λ₯Ό μ¬μ©νμ¬ νμν¨
2. νμν νμ€νΈ λΈλμ nodeValue νλ‘νΌν°λ₯Ό μ¬μ©νμ¬ νμ€νΈ λΈλμ κ°μ λ³κ²½

```js
const $textNode = document.getElementById("foo").firstChild;

$textNode.nodeValue = "World";

console.log($textNode.nodeValue); // World
```

### textContent

> π‘ μμ λΈλμ textContent νλ‘νΌν°λ₯Ό μ°Έμ‘°νλ©΄ μμ λΈλμ μ½νμΈ  μμ­(μμ νκ·Έμ μ’λ£ νκ·Έ μ¬μ΄) λ΄μ νμ€νΈλ₯Ό λͺ¨λ λ°ν

- Node.prototype.textContent νλ‘νΌν°λ setterμ getter λͺ¨λ μ‘΄μ¬νλ μ κ·Όμ νλ‘νΌν°λ‘μ μμ λΈλμ νμ€νΈμ λͺ¨λ  μμ λΈλμ νμ€νΈλ₯Ό μ·¨λνκ±°λ λ³κ²½

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello <span>world!</span></div>
	</body>
	<script>
		// #foo μμ λΈλμ νμ€νΈλ₯Ό λͺ¨λ μ·¨λ
		console.log(document.getElementById("foo").textContent); // Hello world!
	</script>
</html>
```

textContent νλ‘νΌν°μ μ μ¬ν λμμ νλ `innerText` νλ‘νΌν°κ° μμ
innerText νλ‘νΌν°λ λ€μκ³Ό κ°μ μ΄μ λ‘ μ¬μ©νμ§ μλ κ²μ΄ μ’μ

- innerText νλ‘νΌν° CSSμ μμ’μ 
  - CSSμ μν΄ λΉνμ(visibility: hidden;)λ‘ μ§μ λ μμ λΈλμ νμ€νΈλ₯Ό λ°ννμ§ μμ
- innerText νλ‘νΌν°λ CSSλ₯Ό κ³ λ €ν΄μΌ νλ―λ‘ textContent νλ‘νΌν°λ³΄λ€ λλ¦Ό

## 39.6 DOM μ‘°μ

## 39.7 μ΄νΈλ¦¬λ·°νΈ

## 39.8 μ€νμΌ

## 39.9 DOM νμ€

> π‘ W3Cκ³Ό WHATWGμ΄λΌλ λ λ¨μ²΄μμ κ³΅ν΅λ νμ€μ λ§λ¦

DOMμ νμ¬ λ€μκ³Ό κ°μ΄ 4κ°μ λ²μ μ΄ μμ

| λ λ²¨        | νμ€ λ¬Έμ URL                          |
| ----------- | -------------------------------------- |
| DOM Level 1 | https://www.w3.org/TR/REC-DOM-Level-1  |
| DOM Level 2 | https://www.w3.org/TR/DOM-Level-2-Core |
| DOM Level 3 | https://www.w3.org/TR/DOM-Level-3-Core |
| DOM Level 4 | https://dom.spec.whatwg.org            |

### μ°Έκ³ 

- https://yung-developer.tistory.com/74
- https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html
- https://velog.io/@hangem422/js-web-node
