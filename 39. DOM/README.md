# 39. DOM

> ๐ก HTML ๋ฌธ์์ ๊ณ์ธต์  ๊ตฌ์กฐ์ ์ ๋ณด๋ฅผ ํํํ๋ฉฐ ์ด๋ฅผ ์ ์ดํ  ์ ์๋ API, ์ฆ ํ๋กํผํฐ์ ๋ฉ์๋๋ฅผ ์ ๊ณตํ๋ ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ

<br>

## 39.1 ๋ธ๋

#### 1) HTML ์์์ ๋ธ๋ ๊ฐ์ฒด

> HTML ์์๋ HTML ๋ฌธ์๋ฅผ ๊ตฌ์ฑํ๋ ๊ฐ๋ณ์ ์ธ ์์ ์๋ฏธ

<br>

<img style="height:250px;" src="https://media.vlpt.us/images/hangem422/post/9565d686-2059-473b-a98b-8445876df26a/web-node01.png">

<br>

- HTML ์์๋ ๋ ๋๋ง ์์ง์ ์ํด ํ์ฑ๋์ด DOM ์ ๊ตฌ์ฑํ๋ `์์ ๋ธ๋ ๊ฐ์ฒด`๋ก ๋ณํ๋จ.
  - `HTML ์์์ ์ดํธ๋ฆฌ๋ทฐํธ๋ ์ดํธ๋ฆฌ๋ทฐํธ ๋ธ๋๋ก, HTML ์์์ ํ์คํธ ์ฝํ์ธ ๋ ํ์คํธ ๋ธ๋๋ก ๋ณํ๋จ.`
    <br>
- HTML ๋ฌธ์๋ HTML ์์๋ค์ ์งํฉ์ผ๋ก ์ด๋ค์ง๋ฉฐ, HTML ์์๋ `์ค์ฒฉ ๊ด๊ณ`๋ฅผ ๊ฐ์ง.

<br>

#### \*\* **ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ**

![img](https://gmlwjd9405.github.io/images/data-structure-tree/tree-terms.png)
<br>

- ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ๋ **๋ธ๋๋ค์ ๊ณ์ธต ๊ตฌ์กฐ**๋ก ์ด๋ฃจ์ด์ง.

  - ๋ถ๋ชจ ๋ธ๋์ ์์ ๋ธ๋๋ก ๊ตฌ์ฑ๋์ด ๋ธ๋ ๊ฐ์ ๊ณ์ธต์  (๋ถ์, ํ์  ๊ด๊ณ) ๊ตฌ์กฐ๋ฅผ ํํํ๋ ๋น์ ํ ์๋ฃ๊ตฌ์กฐ
    <br>

- ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ๋ **ํ๋์ ์ต์์ ๋ธ๋์์ ์์**

  - ์ต์์ ๋ธ๋๋ ๋ถ๋ชจ ๋ธ๋๊ฐ ์์ผ๋ฉฐ, `๋ฃจํธ ๋ธ๋`๋ผ๊ณ ๋ ๋ถ๋ฆผ.
  - ๋ฃจํธ ๋ธ๋๋ 0๊ฐ ์ด์์ ์์ ๋ธ๋๋ฅผ ๊ฐ์ง๋ฉฐ, ์์ ๋ธ๋๊ฐ ์๋ ๋ธ๋๋ฅผ `๋ฆฌํ ๋ธ๋`๋ผ๊ณ  ํจ.
    <br>

- **๋ธ๋ ๊ฐ์ฒด๋ค๋ก ๊ตฌ์ฑ๋ ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ๋ฅผ DOM** ์ด๋ผ๊ณ  ํ๋ฉฐ, ๋ธ๋ ๊ฐ์ฒด์ ํธ๋ฆฌ๋ก ๊ตฌ์กฐํ๋์ด ์๊ธฐ ๋๋ฌธ์ DOM ์ **DOM ํธ๋ฆฌ**๋ผ๊ณ  ๋ถ๋ฅด๊ธฐ๋ ํจ.
  <br>

#### \*\* **๋ธ๋ ๊ฐ์ฒด์ ํ์**

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

- ๋ ๋๋ง ์์ง์ด ์ HTML ๋ฌธ์๋ฅผ ํ์ฑํ ๊ฒฝ์ฐ ๋ค์๊ณผ ๊ฐ์ DOM ์์ฑ

<br>

![image](https://user-images.githubusercontent.com/77706631/155495328-002cecc1-75bf-4c28-9693-dcd400ffbb83.png)
<br>

- DOM ์ ๋ธ๋ ๊ฐ์ฒด์ ๊ณ์ธต์ ์ธ ๊ตฌ์กฐ๋ก ๊ตฌ์ฑ
- ๋ธ๋ ๊ฐ์ฒด๋ ์ด 12๊ฐ์ ์ข๋ฅ๊ฐ ์์ผ๋ฉฐ, ๋ชจ๋ ์์ ๊ตฌ์กฐ O
  - ์ด ์ค์์ ์ค์ํ ๋ธ๋ ํ์์ ๋ค์๊ณผ ๊ฐ์ด 4๊ฐ์ง๊ฐ ์์.
    - ๋ฌธ์ ๋ธ๋
    - ์์ ๋ธ๋
    - ์ดํธ๋ฆฌ๋ทฐํธ ๋ธ๋
    - ํ์คํธ ๋ธ๋
      <br>

#### 1) ๋ฌธ์ ๋ธ๋

- **`DOM ํธ๋ฆฌ์ ์ต์์์ ์กด์ฌํ๋ ๋ฃจํธ ๋ธ๋๋ก์ document ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํด`**
  <br>
- HTML ๋ฌธ์๋น document ๊ฐ์ฒด๋ ์ ์ผํจ.

  - script ํ๊ทธ์ ์ํด ์๋ฐ์คํฌ๋ฆฝํธ ์ฝ๋๊ฐ ๋ถ๋ฆฌ๋์ด ์์ด๋ ํ๋์ ์ ์ญ ๊ฐ์ฒด window ๋ฅผ ๊ณต์ 
    => window ์ document ํ๋กํผํฐ์ ๋ฐ์ธ๋ฉ๋์ด ์๋ ํ๋์ document ๊ฐ์ฒด๋ฅผ ๋ฐ๋ผ๋ด.
    <br>

- ๋ฌธ์ ๋ธ๋, ์ฆ document ๊ฐ์ฒด๋ DOM ํธ๋ฆฌ์ ๋ฃจํธ ๋ธ๋์ด๋ฏ๋ก `DOM ํธ๋ฆฌ์ ๋ธ๋๋ค์ ์ ๊ทผํ๊ธฐ ์ํ ์ง์์  ์ญํ `
  <br>

##### \*\* **`document ๊ฐ์ฒด๋?`**

- ๋ธ๋ผ์ฐ์ ๊ฐ ๋ ๋๋งํ HTML ๋ฌธ์ ์ ์ฒด๋ฅผ ๊ฐ๋ฆฌํค๋ ๊ฐ์ฒด
- ์ ์ญ ๊ฐ์ฒด window ์ document ํ๋กํผํฐ์ ๋ฐ์ธ๋ฉ๋์ด ์์.
- ๋ฌธ์ ๋ธ๋๋ window.document ๋๋ document ๋ก ์ฐธ์กฐ ๊ฐ๋ฅ
  <br>

#### 2) ์์ ๋ธ๋

- **`HTML ์์๋ฅผ ๊ฐ๋ฆฌํค๋ ๊ฐ์ฒด`**
  <br>

- HTML ์์ ๊ฐ์ ์ค์ฒฉ์ ์ํด ๋ถ์ ๊ด๊ณ๋ฅผ ๊ฐ์ง๋ฉฐ, ์ด ๋ถ์ ๊ด๊ณ๋ฅผ ํตํด ์ ๋ณด๋ฅผ ๊ตฌ์กฐํํจ.
  => ์์ ๋ธ๋๋ **๋ฌธ์์ ๊ตฌ์กฐ๋ฅผ ํํ**

<br>

#### 3) ์ดํธ๋ฆฌ๋ทฐํธ ๋ธ๋

- **`HTML ์์์ ์ดํธ๋ฆฌ๋ทฐํธ๋ฅผ ๊ฐ๋ฆฌํค๋ ๊ฐ์ฒด`**
  <br>

- ๋ถ๋ชจ ๋ธ๋์ ์ฐ๊ฒฐ๋์ด ์์ง ์๊ณ , `์ดํธ๋ฆฌ๋ทฐํธ๊ฐ ์ง์ ๋ HTML ์์์ ์์ ๋ธ๋์๋ง ์ฐ๊ฒฐ๋์ด ์์.`
  - ๋ถ๋ชจ ๋ธ๋๊ฐ ์์ผ๋ฏ๋ก ์์ ๋ธ๋์ ํ์  ๋ธ๋ X
  - ์ดํธ๋ฆฌ๋ทฐํธ ๋ธ๋์ ์ ๊ทผํ์ฌ ์ดํธ๋ฆฌ๋ทฐํธ๋ฅผ ์ฐธ์กฐ/๋ณ๊ฒฝํ๋ ค๋ฉด ๋จผ์  ์์ ๋ธ๋์ ์ ๊ทผํด์ผ ํจ.
    <br>

#### 4) ํ์คํธ ๋ธ๋

- **`HTML ์์์ ํ์คํธ๋ฅผ ๊ฐ๋ฆฌํค๋ ๊ฐ์ฒด`**
  <br>

- `์์ ๋ธ๋์ ์์ ๋ธ๋์ด๋ฉฐ, ์์ ๋ธ๋๋ฅผ ๊ฐ์ง ์ ์๋ ๋ฆฌํ ๋ธ๋`
  => ํ์คํธ ๋ธ๋๋ DOM ํธ๋ฆฌ์ ์ต์ข๋จ์ด๊ธฐ ๋๋ฌธ์ ํ์คํธ ๋ธ๋์ ์ ๊ทผํ๋ ค๋ฉด ๋ถ๋ชจ ๋ธ๋์ธ ์์ ๋ธ๋์ ๋จผ์  ์ ๊ทผํด์ผ ํจ.

<br>

#### \*\* **๋ธ๋ ๊ฐ์ฒด์ ์์ ๊ตฌ์กฐ**

> ๐ก ๋ธ๋ ๊ฐ์ฒด๋ ์๋ฐ์คํฌ๋ฆฝํธ ๊ฐ์ฒด์ด๋ฏ๋ก **ํ๋กํ ํ์์ ์ํ ์์ ๊ตฌ์กฐ**๋ฅผ ๊ฐ์ง.

<br>

![image](https://user-images.githubusercontent.com/55246584/155320546-99fb667e-0834-4c6b-8b1a-aedf74e34867.png)

<br>

- **๋ชจ๋  ๋ธ๋ ๊ฐ์ฒด๋ Object, EventTarget, Node ์ธํฐํ์ด์ค๋ฅผ ์์๋ฐ์**
  <br>

- ๋ธ๋ ๊ฐ์ฒด์ ์์ ๊ตฌ์กฐ๋ ๊ฐ๋ฐ์ ๋๊ตฌ์ Elements ํจ๋ ์ฐ์ธก์ Properties ํจ๋์์ ํ์ธ ๊ฐ๋ฅ

<br>

<img style="height:500px" src="https://user-images.githubusercontent.com/55246584/155320895-71d4379f-e5ad-42b7-80f7-bf6427d2b5c8.png">

<br><br>

```html
<!DOCTYPE html>
<html>
	<body>
		<input type="text" />
		<script>
			const $input = document.querySelector("input");

			// input ์์ ๋ธ๋ ๊ฐ์ฒด์ ํ๋กํ ํ์ ์ฒด์ธ
			console.log(
				Object.getPrototypeOf($input) === HTMLInputElement.prototype,
				Object.getPrototypeOf(HTMLInputElement.prototype) === HTMLElement.prototype,
				Object.getPrototypeOf(HTMLElement.prototype) === Element.prototype,
				Object.getPrototypeOf(Element.prototype) === Node.prototype,
				Object.getPrototypeOf(Node.prototype) === EventTarget.prototype,
				Object.getPrototypeOf(EventTarget.prototype) === Object.prototype
			); // ๋ชจ๋ true
		</script>
	</body>
</html>
```

<br>

| input ์์ ๋ธ๋ ๊ฐ์ฒด์ ํน์ฑ                                                | ํ๋กํ ํ์์ ์ ๊ณตํ๋ ๊ฐ์ฒด |
| -------------------------------------------------------------------------- | -------------------------- |
| ๊ฐ์ฒด                                                                       | Object                     |
| ์ด๋ฒคํธ๋ฅผ ๋ฐ์์ํค๋ ๊ฐ์ฒด                                                   | EventTarget                |
| ํธ๋ฆฌ ์๋ฃ๊ตฌ์กฐ์ ๋ธ๋ ๊ฐ์ฒด                                                  | Node                       |
| ๋ธ๋ผ์ฐ์ ๊ฐ ๋ ๋๋งํ  ์ ์๋ ์น ๋ฌธ์์ ์์(HTML, XML, SVG๋ฅผ ํํํ๋ ๊ฐ์ฒด) | Element                    |
| ์น ๋ฌธ์์ ์์ ์ค์์ HTML ์์๋ฅผ ํํํ๋ ๊ฐ์ฒด                            | HTMLElement                |
| HTML ์์ ์ค์์ input ์์๋ฅผ ํํํ๋ ๊ฐ์ฒด                                | HTMLInputElement           |

<br>

- DOM์ HTML ๋ฌธ์์ ๊ณ์ธต์  ๊ตฌ์กฐ์ ์ ๋ณด๋ฅผ ํํ

  - ๋ธ๋ ํ์์ ๋ฐ๋ผ ํ์ํ ๊ธฐ๋ฅ์ ํ๋กํผํฐ์ ๋ฉ์๋์ ์งํฉ์ธ DOM API๋ก ์ ๊ณต
  - **์ด DOM API๋ฅผ ํตํด HTML์ ๊ตฌ์กฐ๋ ๋ด์ฉ ๋๋ ์คํ์ผ ๋ฑ์ ๋์ ์ผ๋ก ์กฐ์ ๊ฐ๋ฅ**
    <br>

- ์ด๋ฅผ ํ๋กํ ํ์ ์ฒด์ธ ๊ด์ ์์ ์ดํด๋ณด๋ฉด input ์์ ๋ธ๋ ๊ฐ์ฒด๋ **ํ๋กํ ํ์ ์ฒด์ธ์ ์๋ ๋ชจ๋  ํ๋กํ ํ์์ ํ๋กํผํฐ๋ ๋ฉ์๋๋ฅผ ์์๋ฐ์ ์ฌ์ฉ ๊ฐ๋ฅ**

<br>

## 39.2 ์์ ๋ธ๋ ์ทจ๋

- HTML ๊ตฌ์กฐ๋ ๋ด์ฉ ๋๋ ์คํ์ผ ๋ฑ์ ๋์ ์ผ๋ก ์กฐ์ํ๋ ค๋ฉด ๋จผ์  ์์ ๋ธ๋๋ฅผ ์ทจ๋ํด์ผ ํจ.

<br>

#### 1) id ๋ฅผ ์ด์ฉํ ์์ ๋ธ๋ ์ทจ๋

- `Document.prototype.getElementById` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ id ๊ฐ์ ๊ฐ๋ ํ๋์ ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํ
  - getElementById ๋ฉ์๋๋ Document.prototype ์ ํ๋กํผํฐ์ด๋ฏ๋ก ๋ฐ๋์ document ๋ฅผ ํตํด ํธ์ถํด์ผ ํจ.

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
		// getElementById ๋ฉ์๋๋ ์ธ์ ๋ ๋จ ํ๋์ ์์ ๋ธ๋๋ฅผ ๋ฐํ
		// ์ฒซ ๋ฒ์งธ li ์์๊ฐ ํ์ฑ๋์ด ์์ฑ๋ ์์ ๋ธ๋๊ฐ ๋ฐํ๋จ.
		const $elem = document.getElementById("apple");

		// ์ทจ๋ํ ์์ ๋ธ๋์ style.color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		$elem.style.color = "red";

		// id ๊ฐ์ด 'grape' ์ธ ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค. -> null ๋ฐํ
		const $elem = document.getElementById("grape");
		$elem.style.color = "red"; // TypeError
	</script>
</html>
```

- `id ๊ฐ์ HTML ๋ฌธ์ ๋ด์์ ์ ์ผํ ๊ฐ`์ด์ด์ผ ํ๋ฉฐ, class ์ดํธ๋ฆฌ๋ทฐํธ์๋ ๋ฌ๋ฆฌ ๊ณต๋ฐฑ ๋ฌธ์๋ก ๊ตฌ๋ถํ์ฌ ์ฌ๋ฌ ๊ฐ์ ๊ฐ์ ๊ฐ์ง ์ ์์.
  <br>

- `getElementById ๋ฉ์๋๋ ์ธ์ ๋ ๋จ ํ๋์ ์์ ๋ธ๋๋ง์ ๋ฐํ`

  - ์ค๋ณต๋ id ๊ฐ์ ๊ฐ๋ HTML ์์๊ฐ ์ฌ๋ฌ ๊ฐ ์กด์ฌํ๋๋ผ๋ ์๋ฌ X
  - getElementById ๋ฉ์๋๋ **์ธ์๋ก ์ ๋ฌ๋ id ๊ฐ์ ๊ฐ๋ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ง ๋ฐํ**
    <br>

- `์ธ์๋ก ์ ๋ฌ๋ id ๊ฐ์ ๊ฐ๋ HTML ์์๊ฐ ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ getElementById ๋ฉ์๋๋ null ์ ๋ฐํ`
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
		// id ๊ฐ๊ณผ ๋์ผํ ์ด๋ฆ์ ์ ์ญ ๋ณ์๊ฐ ์๋ฌต์ ์ผ๋ก ์ ์ธ๋๊ณ  ํด๋น ๋ธ๋ ๊ฐ์ฒด๊ฐ ํ ๋น๋๋ค.
		console.log(foo === document.getElementById("foo")); // true

		// ์๋ฌต์  ์ ์ญ์ผ๋ก ์์ฑ๋ ์ ์ญ ํ๋กํผํฐ๋ ์ญ์ ๋์ง๋ง ์ ์ญ ๋ณ์๋ ์ญ์ ๋์ง ์๋๋ค.
		delete foo;
		console.log(foo); // <div id="foo"></div>
	</script>
</html>
```

- HTML ์์์ id ์ดํธ๋ฆฌ๋ทฐํธ๋ฅผ ๋ถ์ฌํ๋ฉด id ๊ฐ๊ณผ ๋์ผํ ์ด๋ฆ์ ์ ์ญ ๋ณ์๊ฐ ์๋ฌต์ ์ผ๋ก ์ ์ธ๋๊ณ  ํด๋น ๋ธ๋ ๊ฐ์ฒด๊ฐ ํ ๋น๋๋ ๋ถ์ ํจ๊ณผ O
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

		// id ๊ฐ๊ณผ ๋์ผํ ์ด๋ฆ์ ์ ์ญ ๋ณ์๊ฐ ์ด๋ฏธ ์ ์ธ๋์ด ์์ผ๋ฉฐ ๋ธ๋ ๊ฐ์ฒด๊ฐ ์ฌํ ๋น๋์ง ์๋๋ค.
		console.log(foo); // 1
	</script>
	x
</html>
```

- ๋จ, id ๊ฐ๊ณผ ๋์ผํ ์ด๋ฆ์ ์ ์ญ ๋ณ์๊ฐ ์ด๋ฏธ ์ ์ธ๋์ด ์๋ค๋ฉด ์ด ์ ์ญ ๋ณ์์ ๋ธ๋ ๊ฐ์ฒด๊ฐ ์ฌํ ๋น๋์ง ์์.
  <br>

#### 2) ํ๊ทธ ์ด๋ฆ์ ์ด์ฉํ ์์ ๋ธ๋ ์ทจ๋

- `Document.prototype/Element.prototype.getElementsByTagName` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ ํ๊ทธ ์ด๋ฆ์ ๊ฐ๋ ๋ชจ๋  ์์ ๋ธ๋๋ค์ ํ์ํ์ฌ ๋ฐํ
  <br>
- ์ฌ๋ฌ ๊ฐ์ ์์ ๋ธ๋ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ **HTMLCollection ๊ฐ์ฒด๋ฅผ ๋ฐํ**
  - `HTMLCollection ๊ฐ์ฒด๋ ์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด์ด๋ฉด์ ์ดํฐ๋ฌ๋ธ`

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
		// ํ๊ทธ ์ด๋ฆ์ด li ์ธ ์์ ๋ธ๋๋ฅผ ๋ชจ๋ ํ์ํ์ฌ ๋ฐํํ๋ค.
		// ํ์๋ ์์ ๋ธ๋๋ค์ HTMLCollection ๊ฐ์ฒด์ ๋ด๊ฒจ ๋ฐํ๋๋ค.
		const $elems = document.getElementByTagName("li");

		// ์ทจ๋ํ ๋ชจ๋  ์์ ๋ธ๋์ style.color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		// HTMLCollection ๊ฐ์ฒด๋ฅผ ๋ฐฐ์ด๋ก ๋ณํํ์ฌ ์ํํ๋ฉฐ color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		[...$elems].forEach((elem) => {
			elem.style.color = "red";
		});

		// ๋ชจ๋  ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค.
		const $all = document.getElementByTagName("*");
		// -> HTMLCollection(8) [html, head, body, ul, li#apple, li#banana, li#orange, script,apple: li#apple, banana: li#banana, orange: li#orange]
	</script>
</html>
```

- HTML ๋ฌธ์์ ๋ชจ๋  ๋ธ๋ ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ ค๋ฉด getElementByTagName ๋ฉ์๋์ ์ธ์๋ก '\*' ๋ฅผ ์ ๋ฌ
  <br>
- ๋ง์ฝ ์ธ์๋ก ์ ๋ฌ๋ ํ๊ทธ ์ด๋ฆ์ ๊ฐ๋ ์์๊ฐ ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ ๋น HTMLCollection ๊ฐ์ฒด๋ฅผ ๋ฐํ
  <br>

#### 3) class ๋ฅผ ์ด์ฉํ ์์ ๋ธ๋ ์ทจ๋

- `Document.prototype/Element.prototype.getElementsByClassName` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ class ๊ฐ์ ๊ฐ๋ ๋ชจ๋  ์์ ๋ธ๋๋ค์ ํ์ํ์ฌ ๋ฐํ
  <br>

- getElementsByClassName ๋ฉ์๋๋ ์ฌ๋ฌ ๊ฐ์ ์์ ๋ธ๋ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ **DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ HTMLCollection ๊ฐ์ฒด๋ฅผ ๋ฐํ**

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
		// class ๊ฐ์ด 'fruit' ์ธ ์์ ๋ธ๋๋ฅผ ๋ชจ๋ ํ์ํ์ฌ HTMLCollection ๊ฐ์ฒด์ ๋ด์ ๋ฐํํ๋ค.
		const $elems = document.getElemetByClassName("fruit");

		// ์ทจ๋ํ ๋ชจ๋  ์์์ CSS color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		[...$elems].forEach((elem) => {
			elem.style.color = "red";
		});

		// class ๊ฐ์ด 'fruit apple' ์ธ ์์ ๋ธ๋๋ฅผ ๋ชจ๋ ํ์ํ์ฌ HTMLCollection ๊ฐ์ฒด์ ๋ด์ ๋ฐํํ๋ค.
		const $apples = document.getElementByClassName("fruit apple");

		// ์ทจ๋ํ ๋ชจ๋  ์์ ๋ธ๋์ style.color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		[...$apples].forEach((elem) => {
			elem.style.color = "blue";
		});
	</script>
</html>
```

- `์ธ์๋ก ์ ๋ฌํ  class ๊ฐ์ ๊ณต๋ฐฑ์ผ๋ก ๊ตฌ๋ถํ์ฌ ์ฌ๋ฌ ๊ฐ์ class ์ง์  ๊ฐ๋ฅ`
  <br>
- Element.prototype.getElementByClassName ๋ฉ์๋๋ ํน์  ์์ ๋ธ๋๋ฅผ ํตํด ํธ์ถํ๋ฉฐ ํน์  ์์ ๋ธ๋์ ์์ ๋ธ๋ ์ค์์ ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํ
  <br>
- ์ธ์๋ก ์ ๋ฌ๋ class ๊ฐ์ ๊ฐ๋ ์์๊ฐ ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ getElementsByClassName ๋ฉ์๋๋ ๋น HTMLCollection ๊ฐ์ฒด๋ฅผ ๋ฐํ

<br>

#### 4) CSS ์ ํ์๋ฅผ ์ด์ฉํ ์์ ๋ธ๋ ์ทจ๋

<br>

```js
/* ์ ์ฒด ์ ํ์ : ๋ชจ๋  ์์๋ฅผ ์ ํ */
* { ... }

/* ํ๊ทธ ์ ํ์ : ๋ชจ๋  p ํ๊ทธ ์์๋ฅผ ๋ชจ๋ ์ ํ */
p { ... }

/* id ์ ํ์ : id ๊ฐ์ด 'foo' ์ธ ์์๋ฅผ ์ ํ */
#foo { ... }

/* class ์ ํ์ : class ๊ฐ์ด 'foo' ์ธ ์์๋ฅผ ๋ชจ๋ ์ ํ */
.foo { ... }

/* ์ดํธ๋ฆฌ๋ทฐํธ ์ ํ์ : input ์์ ์ค์ type ์ดํธ๋ฆฌ๋ทฐํธ ๊ฐ์ด 'text' ์ธ ์์๋ฅผ ๋ชจ๋ ์ ํ */
input[type=text] { ... }

/* ํ์ ์ ํ์ : div ์์์ ํ์ ์์ ์ค p ์์๋ฅผ ๋ชจ๋ ์ ํ */
div p { ... }

/* ์์ ์ ํ์ : div ์์์ ์์ ์์ ์ค p ์์๋ฅผ ๋ชจ๋ ์ ํ */
div > p { ... }

/* ์ธ์  ํ์  ์ ํ์ : p ์์์ ํ์  ์์ ์ค์ p ์์ ๋ฐ๋ก ๋ค์ ์์นํ๋ ul ์์๋ฅผ ์ ํ */
p + ul { ... }

/* ์ผ๋ฐ ํ์  ์ ํ์ : p ์์์ ํ์  ์์ ์ค์ p ์์ ๋ค์ ์์นํ๋ ul ์์๋ฅผ ๋ชจ๋ ์ ํ */
p ~ ul { ... }

/* ๊ฐ์ ํด๋์ค ์ ํ์ : hover ์ํ์ธ a ์์๋ฅผ ๋ชจ๋ ์ ํ */
a:hover { ... }

/* ๊ฐ์ ์์ ์ ํ์ : p ์์์ ์ฝํ์ธ ์ ์์ ์์นํ๋ ๊ณต๊ฐ์ ์ ํ
                      ์ผ๋ฐ์ ์ผ๋ก content ํ๋กํผํฐ์ ํจ๊ป ์ฌ์ฉ๋๋ค. */
p::before { ... }
```

- CSS ์ ํ์๋ ์คํ์ผ์ ์ ์ฉํ๊ณ ์ ํ๋ HTML ์์๋ฅผ ํน์ ํ  ๋ ์ฌ์ฉํ๋ ๋ฌธ๋ฒ

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
		// class ์ดํธ๋ฆฌ๋ทฐํธ ๊ฐ์ด 'banana' ์ธ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค.
		const $elem = document.querySelector(".banana");

		// ์ทจ๋ํ ์์ ๋ธ๋์ style.color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		$elem.style.color = "red";
	</script>
</html>
```

- `Document.prototype/Element.prototype.querySelector` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ CSS ์ ํ์๋ฅผ ๋ง์กฑ์ํค๋ ํ๋์ ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํ
  - ์ธ์๋ก ์ ๋ฌํ CSS ์ ํ์๋ฅผ ๋ง์กฑ์ํค๋ **์์ ๋ธ๋๊ฐ ์ฌ๋ฌ ๊ฐ์ธ ๊ฒฝ์ฐ ์ฒซ ๋ฒ์ฌ ์์ ๋ธ๋๋ง ๋ฐํ**
  - ์ธ์๋ก ์ ๋ฌ๋ CSS ์ ํ์๋ฅผ ๋ง์กฑ์ํค๋ **์์ ๋ธ๋๊ฐ ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ null ๋ฐํ**
  - ์ธ์๋ก ์ ๋ฌ๋ CSS ์ ํ์๊ฐ **๋ฌธ๋ฒ์ ๋ง์ง ์๋ ๊ฒฝ์ฐ DOMException ์๋ฌ ๋ฐ์**

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
		// ul ์์์ ์์ ์์์ธ li ์์๋ฅผ ๋ชจ๋ ํ์ํ์ฌ ๋ฐํํ๋ค.
		const $elems = document.querySelectorAll("ul > li");

		// ์ทจ๋ํ ์์ ๋ธ๋๋ค์ NodeList ๊ฐ์ฒด์ ๋ด๊ฒจ ๋ฐํ๋๋ค.
		console.log($elems); // NodeList(3) [li.apple, li.banana, li.orange]

		// ์ทจ๋ํ ๋ชจ๋  ์์ ๋ธ๋์ style.color ํ๋กํผํฐ ๊ฐ์ ๋ณ๊ฒฝํ๋ค.
		// NodeList ๋ forEach ๋ฉ์๋๋ฅผ ์ ๊ณตํ๋ค.
		$elems.forEach((elem) => {
			elem.style.color = "red";
		});
	</script>
</html>
```

- `Document.prototype/Element.prototype.querySelectorAll` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ CSS ์ ํ์๋ฅผ ๋ง์กฑ์ํค๋ **๋ชจ๋  ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํ**
  <br>

- ์ฌ๋ฌ ๊ฐ์ ์์ ๋ธ๋ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ `NodeList ๊ฐ์ฒด๋ฅผ ๋ฐํ`
- ์ธ์๋ก ์ ๋ฌ๋ CSS ์ ํ์๋ฅผ ๋ง์กฑ์ํค๋ ์์๊ฐ ์กด์ฌํ์ง ์๋ ๊ฒฝ์ฐ ๋น NodeList ๊ฐ์ฒด๋ฅผ ๋ฐํ

  - NodeList ๊ฐ์ฒด๋ `์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด์ด๋ฉด์ ์ดํฐ๋ฌ๋ธ`
    <br>

- ์ธ์๋ก ์ ๋ฌํ CSS ์ ํ์๊ฐ ๋ฌธ๋ฒ์ ๋ง์ง ์๋ ๊ฒฝ์ฐ DOMException ์๋ฌ ๋ฐ์
  <br>

- HTML ๋ฌธ์์ ๋ชจ๋  ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ ค๋ฉด querySelectorAll ๋ฉ์๋์ ์ธ์๋ก ์ ์ฒด ์ ํ์ \* ๋ฅผ ์ ๋ฌ
  <br>

#### 5) ํน์  ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ  ์ ์๋์ง ํ์ธ

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

		// #apple ๋ธ๋๋ '#fruits > li.apple' ๋ก ์ทจ๋ํ  ์ ์๋ค.
		console.log($apple.matches("#fruits > li.apple")); // true

		// #apple ๋ธ๋๋ '#fruits > li.banana' ๋ก ์ทจ๋ํ  ์ ์๋ค.
		console.log($apple.matches("#fruits > li.banana")); // false
	</script>
</html>
```

- `Element.prototype.matches` ๋ฉ์๋๋ ์ธ์๋ก ์ ๋ฌํ CSS ์ ํ์๋ฅผ ํตํด ํน์  ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ  ์ ์๋ ์ง ํ์ธ
  <br>

#### 6) HTMLCollection ๊ณผ NodeList

- DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ HTMLCollection ๊ณผ NodeList ๋ **DOM API ๊ฐ ์ฌ๋ฌ ๊ฐ์ ๊ฒฐ๊ณผ๊ฐ์ ๋ฐํํ๊ธฐ ์ํ DOM ์ปฌ๋ ์ ๊ฐ์ฒด**
  <br>

- HTMLCollection ๊ณผ NodeList ๋ชจ๋ `์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด์ด๋ฉด์ ์ดํฐ๋ฌ๋ธ`
  - for... of ๋ฌธ์ผ๋ก ์ํํ  ์ ์์ผ๋ฉฐ ์คํ๋ ๋ ๋ฌธ๋ฒ์ ์ฌ์ฉํ์ฌ ๊ฐ๋จํ ๋ฐฐ์ด๋ก ๋ณํ ๊ฐ๋ฅ
    <br>
- HTMLCollection ๊ณผ NodeList ๋ **๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณํ๋ฅผ ์ค์๊ฐ์ผ๋ก ๋ฐ์ํ๋ ์ด์์๋ ๊ฐ์ฒด** ๋ผ๋ ๊ฒ
  - HTMLCollection ์ ์ธ์ ๋ live ๊ฐ์ฒด๋ก ๋์
  - NodeList ๋ ๋๋ถ๋ถ์ ๊ฒฝ์ฐ non-live ๊ฐ์ฒด๋ก ๋์ํ์ง๋ง ๊ฒฝ์ฐ์ ๋ฐ๋ผ ์ข์ข live ๊ฐ์ฒด๋ก ๋์

<br>

##### HTML Collection

- getElementsByTagName, getElementsByClassName ๋ฉ์๋๊ฐ ๋ฐํํ๋ `HTMLCollection ๊ฐ์ฒด๋ ๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณํ๋ฅผ ์ค์๊ฐ์ผ๋ก ๋ฐ์ํ๋ ์ด์์๋ DOM ์ปฌ๋ ์ ๊ฐ์ฒด`

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
		// class ๊ฐ์ด 'red' ์ธ ์์ ๋ธ๋๋ฅผ ๋ชจ๋ ํ์ํ์ฌ HTMLCollection ๊ฐ์ฒด์ ๋ด์ ๋ฐํํ๋ค.
		const $elems = document.getElementByClassName(".red");

		// ์ด ์์ ์ HTMLCollection ๊ฐ์ฒด์๋ 3๊ฐ์ ์์ ๋ธ๋๊ฐ ๋ด๊ฒจ ์๋ค.
		console.log($elems); // HTMLCollection(3) [li.red, li.red, li.red]

		// HTMLCollection ๊ฐ์ฒด์ ๋ชจ๋  ์์์ class ๊ฐ์ 'blue' ๋ก ๋ณ๊ฒฝํ๋ค.
		for (let i = 0; i < $elems.length; i++) {
			$elems[i].className = "blue";
		}

		// HTMLCollection ๊ฐ์ฒด์ ์์๊ฐ 3๊ฐ์์ 1๊ฐ๋ก ๋ณ๊ฒฝ๋์๋ค.
		console.log($elems);
	</script>
</html>
```

- ์ ์์ ๋ฅผ ์คํํด๋ณด๋ฉด ๋ ๋ฒ์งธ li ์์๋ง class ๊ฐ์ด ๋ณ๊ฒฝ๋์ง ์์.
  - HTMLCollection ๊ฐ์ฒด๋ ์ค์๊ฐ์ผ๋ก ๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณ๊ฒฝ์ ๋ฐ์ํ์ฌ ์์๋ฅผ ์ ๊ฑฐํ  ์ ์๊ธฐ ๋๋ฌธ

<br>

```js
// ์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด์ด๋ฉด์ ์ดํฐ๋ฌ๋ธ์ธ HTMLCollection ์ ๋ฐฐ์ด๋ก ๋ณํํ์ฌ ์ํ
[...$elems].forEach((elem) => (elem.className = "blue"));
```

- for ๋ฌธ์ ์ญ๋ฐฉํฅ์ผ๋ก ์ํํ๊ฑฐ๋ ์์ ๊ฐ์ด while ๋ฌธ์ ์ฌ์ฉํ๋ฉด ์ฌ๋ฐ๋ฅด๊ฒ ๋ณ๊ฒฝํ  ์ ์์.
  <br>

#### NodeList

- `querySelectAll` ๋ฉ์๋๋ DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ NodeList ๊ฐ์ฒด๋ฅผ ๋ฐํ
  - NodeList ๊ฐ์ฒด๋ ๋๋ถ๋ถ์ ๊ฒฝ์ฐ ์ค์๊ฐ์ผ๋ก ๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณ๊ฒฝ์ ๋ฐ์ํ์ง ์๋ non-live ๊ฐ์ฒด๋ก ๋์
  - `๋จ, childNodes ํ๋กํผํฐ๊ฐ ๋ฐํํ๋ NodeList ๊ฐ์ฒด๋ ์ค์๊ฐ์ผ๋ก ๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณ๊ฒฝ์ ๋ฐ์ํ๋ live ๊ฐ์ฒด๋ก ๋์`
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

		// childNodes ํ๋กํผํฐ๋ NodeList ๊ฐ์ฒด (live) ๋ฅผ ๋ฐํํ๋ค.
		  const { childNodes } = $fruits;
		  console.log(childNodes instanceof NodeList);    // true

		  // $fruits ์์์ ์์ ๋ธ๋๋ ๊ณต๋ฐฑ ํ์คํธ ๋ธ๋๋ฅผ ํฌํจํ์ฌ ๋ชจ๋ 5๊ฐ์ด๋ค.
		  console.log(childNodes);/ /  NodeList(5) [text, li, text, li, text]

		  for (let i = 0; i < childNodes.length; i++) {
		    // removeChild ๋ฉ์๋๋ $fruits ์์์ ์์ ๋ธ๋๋ฅผ DOM ์์ ์ญ์ ํ๋ค.
		    // removeChild ๋ฉ์๋๊ฐ ํธ์ถ๋  ๋๋ง๋ค NodeList ๊ฐ์ฒด์ธ childNodes ๊ฐ ์ค์๊ฐ์ผ๋ก ๋ณ๊ฒฝ๋๋ค.
		    // ๋ฐ๋ผ์ ์ฒซ ๋ฒ์งธ, ์ธ ๋ฒ์งธ, ๋ค์ฏ ๋ฒ์งธ ์์๋ง ์ญ์ ํ๋ค.
		    $fruits.removeChild(childNodes[i]);
		  }

		  // ์์๊ณผ ๋ค๋ฅด๊ฒ $fruits ์์์ ๋ชจ๋  ์์ ๋ธ๋๊ฐ ์ญ์ ๋์ง ์๋๋ค.
		  console.lgg(childNodes); // NodeList(2) [li, li]
	</script>
</html>
```

- ์ด์ฒ๋ผ HTMLCollection ์ด๋ NodeList ๊ฐ์ฒด๋ ์์๊ณผ ๋ค๋ฅด๊ฒ ๋์ํ  ๋๊ฐ ์์ด ๋ค๋ฃจ๊ธฐ ๊น๋ค๋กญ๊ณ  ์ค์ํ๊ธฐ ์ฌ์.
  <br>
- ๋ฐ๋ผ์ ๋ธ๋ ๊ฐ์ฒด์ ์ํ ๋ณ๊ฒฝ๊ณผ ์๊ด์์ด ์์ ํ๊ฒ DOM ์ปฌ๋ ์์ ์ฌ์ฉํ๋ ค๋ฉด **HTMLCollection ์ด๋ NodeList ๊ฐ์ฒด๋ฅผ ๋ฐฐ์ด๋ก ๋ณํํ์ฌ ์ฌ์ฉํ๋ ๊ฒ์ ๊ถ์ฅ**
  - `HTMLCollection ์ด๋ NodeList ๊ฐ์ฒด๋ ๋ชจ๋ ์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด์ด๋ฉด์ ์ดํฐ๋ฌ๋ธ`
  - `์คํ๋ ๋ ๋ฌธ๋ฒ์ด๋ Array.from ๋ฉ์๋๋ฅผ ์ฌ์ฉํ์ฌ ๊ฐ๋จํ ๋ฐฐ์ด๋ก ๋ณํ ๊ฐ๋ฅ`

<br>

## 39.3 ๋ธ๋ ํ์

- `๋ธ๋ ํ์ ํ๋กํผํฐ๋ ๋ชจ๋ ์ ๊ทผ์ ํ๋กํผํฐ`
  - setter ์์ด getter ๋ง ์กด์ฌํ์ฌ ์ฐธ์กฐ๋ง ๊ฐ๋ฅํ `์ฝ๊ธฐ ์ ์ฉ ์ ๊ทผ์ ํ๋กํผํฐ`
  - ์ฝ๊ธฐ ์ ์ฉ ํ๋กํผํฐ์ ๊ฐ์ ํ ๋นํ๋ฉด ์๋ฌด๋ฐ ์๋ฌ ์์ด ๋ฌด์๋จ.
    <br>

#### 1) ๊ณต๋ฐฑ ํ์คํธ ๋ธ๋

- HTML ์์ ์ฌ์ด์ ์คํ์ด์ค, ํญ, ์ค๋ฐ๊ฟ ๋ฑ์ ๊ณต๋ฐฑ ๋ฌธ์๋ ํ์คํธ ๋ธ๋๋ฅผ ์์ฑ
  - ์ด๋ฅผ `๊ณต๋ฐฑ ํ์คํธ ๋ธ๋` ๋ผ๊ณ  ๋ถ๋ฆ.

<br>

<img style="height:400px;" src="https://media.vlpt.us/images/ash3767/post/dfad1cfb-be19-45c9-8029-cd4a9d51a03a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-05-01%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.27.42.png">

<br>

- HTML ๋ฌธ์์ ๊ณต๋ฐฑ ๋ฌธ์๋ ๊ณต๋ฐฑ ํ์คํธ ๋ธ๋๋ฅผ ์์ฑํ๊ธฐ ๋๋ฌธ์ ๋ธ๋๋ฅผ ํ์ํ  ๋ ์ฃผ์ํด์ผ ํจ.

<br>

#### 2) ์์ ๋ธ๋ ํ์

##### \*\* **`์์ ๋ธ๋๋ฅผ ํ์ํ  ๋ ์ฌ์ฉํ๋ ๋ธ๋ ํ์ ํ๋กํผํฐ`**

| ํ๋กํผํฐ                            | ์ค๋ช                                                                                                                                                                        |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.childNodes           | ์์ ๋ธ๋๋ฅผ ๋ชจ๋ ํ์ํ์ฌ DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ NodeList ์ ๋ด์ ๋ฐํํ๋ค. childNodes ํ๋กํผํฐ๊ฐ ๋ฐํํ NodeList ์๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋๋ ํฌํจ๋์ด ์์ ์ ์๋ค. |
| Element.prototype.children          | ์์ ๋ธ๋ ์ค์์ ์์ ๋ธ๋๋ง ๋ชจ๋ ํ์ํ์ฌ DOM ์ปฌ๋ ์ ๊ฐ์ฒด์ธ HTMLCollection ์ ๋ด์ ๋ฐํํ๋ค. children ํ๋กํผํฐ๊ฐ ๋ฐํํ HTMLCollection ์๋ ํ์คํธ ๋ธ๋๊ฐ ํฌํจ๋์ง ์๋๋ค. |
| Node.prototype.firstChild           | ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ฅผ ๋ฐํํ๋ค. firstChild ํ๋กํผํฐ๊ฐ ๋ฐํํ ๋ธ๋๋ ํ์คํธ ๋ธ๋์ด๊ฑฐ๋ ์์ ๋ธ๋๋ค.                                                                            |
| Node.prototype.lastChild            | ๋ง์ง๋ง ์์ ๋ธ๋๋ฅผ ๋ฐํํ๋ค. lastChild ํ๋กํผํฐ๊ฐ ๋ฐํํ ๋ธ๋๋ ํ์คํธ ๋ธ๋์ด๊ฑฐ๋ ์์ ๋ธ๋๋ค.                                                                              |
| Element.prototype.firstElementChild | ์ฒซ ๋ฒ์งธ ์์ ์์ ๋ธ๋๋ฅผ ๋ฐํํ๋ค. firstElementChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.                                                                                       |
| Element.prototype.lastElementChild  | ๋ง์ง๋ง ์์ ์์ ๋ธ๋๋ฅผ ๋ฐํํ๋ค. lastElementChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.                                                                                         |

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
		// ๋ธ๋ ํ์์ ๊ธฐ์ ์ด ๋๋ #fruits ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ค.
		const $fruits = document.getElementById("fruits");

		// #fruits ์์์ ๋ชจ๋  ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// childNodes ํ๋กํผํฐ๊ฐ ๋ฐํํ NodeList ์๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋๋ ํฌํจ๋์ด ์๋ค.
		console.log($fruits.childNodes);
		// NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

		// #fruits ์์์ ๋ชจ๋  ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// childNodes ํ๋กํผํฐ๊ฐ ๋ฐํํ HTMLCollection ์๋ ์์ ๋ธ๋๋ง ํฌํจ๋์ด ์๋ค.
		console.log($fruits.children);
		// HTMLCollection(3) [li.apple, li.banana, li.orange]

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// firstChild ํ๋กํผํฐ๋ ํ์คํธ ๋ธ๋๋ฅผ ๋ฐํํ  ์๋ ์๋ค.
		console.log($fruits.firstChild); // #text

		// #fruits ์์์ ๋ง์ง๋ง ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// firstChild ํ๋กํผํฐ๋ ํ์คํธ ๋ธ๋๋ฅผ ๋ฐํํ  ์๋ ์๋ค.
		console.log($fruits.lastChild); // #text

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// firstElementChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.
		console.log($fruits.firstElementChild); // li.apple

		// #fruits ์์์ ๋ง์ง๋ง ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// lastElementChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.
		console.log($fruits.lastElementChild); // li.orange
	</script>
</html>
```

<br>

#### 3) ์์ ๋ธ๋ ์กด์ฌ ํ์ธ

- `Node.prototype.hasChildNodes` ๋ฉ์๋๋ ์์ ๋ธ๋๊ฐ ์กด์ฌํ๋์ง ํ์ธํ์ฌ ๋ถ๋ฆฌ์ธ ๊ฐ ๋ฐํ
- ๋จ, hasChildNodes ๋ฉ์๋๋ childNodes ํ๋กํผํฐ์ ๋ง์ฐฌ๊ฐ์ง๋ก **ํ์คํธ ๋ธ๋๋ฅผ ํฌํจํ์ฌ ์์ ๋ธ๋์ ์กด์ฌ๋ฅผ ํ์ธ**

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
		// ๋ธ๋ ํ์์ ๊ธฐ์ ์ด ๋๋ #fruits ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ค.
		const $fruits = document.getElementById("fruits");

		// #fruits ์์์ ์์ ๋ธ๋๊ฐ ์กด์ฌํ๋์ง ํ์ธํ๋ค.
		// hasChildNodes ๋ฉ์๋๋ ํ์คํธ ๋ธ๋๋ฅผ ํฌํจํ์ฌ ์์ ๋ธ๋์ ์กด์ฌ๋ฅผ ํ์ธํ๋ค.
		console.log($fruits.hasChildNodes()); // true
	</script>
</html>
```

<br>

#### 4) ์์ ๋ธ๋์ ํ์คํธ ๋ธ๋ ํ์

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
		// ์์ ๋ธ๋์ ํ์คํธ ๋ธ๋๋ firstChild ํ๋กํผํฐ๋ก ์ ๊ทผํ  ์ ์๋ค.
		console.log(document.getElementById("foo").firstChild); // #text
	</script>
</html>
```

- ์์ ๋ธ๋์ ํ์คํธ ๋ธ๋๋ ์์ ๋ธ๋์ ์์ ๋ธ๋์ด๋ฏ๋ก `firstChild` ํ๋กํผํฐ๋ก ์ ๊ทผ ๊ฐ๋ฅ
  - firstChild ํ๋กํผํฐ๊ฐ ๋ฐํํ ๋ธ๋๋ ํ์คํธ ๋ธ๋์ด๊ฑฐ๋ ์์ ๋ธ๋

<br>

#### 5) ๋ถ๋ชจ ๋ธ๋ ํ์

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
		// ๋ธ๋ ํ์์ ๊ธฐ์ ์ด ๋๋ .banana ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ค.
		const $banana = document.querySelector(".banana");

		// .banana ์์ ๋ธ๋์ ๋ถ๋ชจ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		console.log($banana.paraetNode); // ul#fruits
	</script>
</html>
```

- ๋ถ๋ชจ ๋ธ๋๋ฅผ ํ์ํ๋ ค๋ฉด `Node.prototype.parentNode` ํ๋กํผํฐ ์ฌ์ฉ
- ํ์คํธ ๋ธ๋๋ DOM ํธ๋ฆฌ์ ์ต์ข๋จ ๋ธ๋์ธ ๋ฆฌํ ๋ธ๋์ด๋ฏ๋ก ๋ถ๋ชจ ๋ธ๋๊ฐ ํ์คํธ ๋ธ๋์ธ ๊ฒฝ์ฐ X

<br>

#### 6) ํ์  ๋ธ๋ ํ์

##### \*\* **`๋ถ๋ชจ ๋ธ๋๊ฐ ๊ฐ์ ํ์  ๋ธ๋๋ฅผ ํ์ ํ  ๋ ์ฌ์ฉํ๋ ๋ธ๋ ํ์ ํ๋กํผํฐ`**

<br>

| ํ๋กํผํฐ                                  | ์ค๋ช                                                                                                                                                                       |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Node.prototype.previousSibling            | ๋ถ๋ชจ ๋ธ๋๊ฐ ๊ฐ์ ํ์  ๋ธ๋ ์ค์์ ์์ ์ ์ด์  ํ์  ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค. previousSibling ํ๋กํผํฐ๊ฐ ๋ฐํํ๋ ํ์  ๋ธ๋๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋์ผ ์๋ ์๋ค. |
| Node.prototype.nextSibling                | ๋ถ๋ชจ ๋ธ๋๊ฐ ๊ฐ์ ํ์  ๋ธ๋ ์ค์์ ์์ ์ ๋ค์ ํ์  ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค. nextSibling ํ๋กํผํฐ๊ฐ ๋ฐํํ๋ ํ์  ๋ธ๋๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋์ผ ์๋ ์๋ค.     |
| Element.prototype.previousElementsSibling | ๋ถ๋ชจ ๋ธ๋๊ฐ ๊ฐ์ ํ์  ์์ ๋ธ๋ ์ค์์ ์์ ์ ์ด์  ํ์  ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค. previousElementsSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.                            |
| Element.prototype.nextElementsSibling     | ๋ถ๋ชจ ๋ธ๋๊ฐ ๊ฐ์ ํ์  ์์ ๋ธ๋ ์ค์์ ์์ ์ ๋ค์ ํ์  ์์ ๋ธ๋๋ฅผ ํ์ํ์ฌ ๋ฐํํ๋ค. nextElementsSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.                                |

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
		// ๋ธ๋ ํ์์ ๊ธฐ์ ์ด ๋๋ #fruits ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ๋ค.
		const $fruits = document.ElementById("fruits");

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// firstChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋๋ฅผ ๋ฐํํ  ์๋ ์๋ค.
		const { firstChild } = $fruits;
		console.log(firstChild); // #text

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ๋ธ๋ (ํ์คํธ ๋ธ๋) ์ ๋ค์ ํ์  ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// nextSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋๋ฅผ ๋ฐํํ  ์๋ ์๋ค.
		const { nextSibling } = firstChild;
		console.log(nextSibling); // li.apple

		// li.apple ์์์ ์ด์  ํ์  ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// previousSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ฟ๋ง ์๋๋ผ ํ์คํธ ๋ธ๋๋ฅผ ๋ฐํํ  ์๋ ์๋ค.
		const { previousSibling } = nextSibling;
		console.log(previousSibling); // #text

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// firstElementChild ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.
		const { firstElementChild } = $fruits;
		console.log(firstElementChild); // li.apple

		// #fruits ์์์ ์ฒซ ๋ฒ์งธ ์์ ์์ ๋ธ๋ (li.apple) ์ ๋ค์ ํ์  ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// nextElementSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.
		const { nextElementSibling } = firstElementChild;
		console.log(nextElementSibling); // li.banana

		// li.banana ์์์ ์ด์  ํ์  ์์ ๋ธ๋๋ฅผ ํ์ํ๋ค.
		// previousElementsSibling ํ๋กํผํฐ๋ ์์ ๋ธ๋๋ง ๋ฐํํ๋ค.
		const { previousElementsSibling } = nextElementSibling;
		console.log(previousElementsSibling); // li.apple
	</script>
</html>
```

<br>

## 39.4 ๋ธ๋ ์ ๋ณด ์ทจ๋

##### \*\* **`๋ธ๋ ๊ฐ์ฒด์ ๋ํ ์ ๋ณด๋ฅผ ์ทจ๋ํ  ๋ ์ฌ์ฉํ๋ ๋ธ๋ ์ ๋ณด ํ๋กํผํฐ`**

<br>

| ํ๋กํผํฐ                | ์ค๋ช                                                                                               |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| Node.prototype.nodeType | ๋ธ๋ ๊ฐ์ฒด์ ์ข๋ฅ, ์ฆ ๋ธ๋ ํ์์ ๋ํ๋ด๋ ์์๋ฅผ ๋ฐํํ๋ค. ๋ธ๋ ํ์ ์์๋ Node ์ ์ ์๋์ด ์๋ค. |
|                         | - `Node.ELEMENT_NODE` : ์์ ๋ธ๋ ํ์์ ๋ํ๋ด๋ ์์ 1์ ๋ฐํ                                    |
|                         | - `Node.TEXT_NODE` : ํ์คํธ ๋ธ๋ ํ์์ ๋ํ๋ด๋ ์์ 3์ ๋ฐํ                                     |
|                         | - `Node.DOCUMENT_NODE` : ๋ฌธ์ ๋ธ๋ ํ์์ ๋ํ๋ด๋ ์์ 9๋ฅผ ๋ฐํ                                   |
| Node.prototype.nodeName | ๋ธ๋์ ์ด๋ฆ์ ๋ฌธ์์ด๋ก ๋ฐํํ๋ค.                                                                   |
|                         | - `์์ ๋ธ๋` : ๋๋ฌธ์ ๋ฌธ์์ด๋ก ํ๊ทธ ์ด๋ฆ("UL", "LI" ๋ฑ) ์ ๋ฐํ                                   |
|                         | - `ํ์คํธ ๋ธ๋` : ๋ฌธ์์ด "#text" ๋ฅผ ๋ฐํ                                                           |
|                         | - `๋ฌธ์ ๋ธ๋` : ๋ฌธ์์ด "#document" ๋ฅผ ๋ฐํ                                                         |

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
		// ๋ฌธ์ ๋ธ๋์ ๋ธ๋ ์ ๋ณด๋ฅผ ์ทจ๋ํ๋ค.
		console.log(document.nodeType); // 9
		console.log(document.nodeName); // #document

		// ์์ ๋ธ๋์ ๋ธ๋ ์ ๋ณด๋ฅผ ์ทจ๋ํ๋ค.
		const $foo = document.getElementById("foo");
		console.log($foo.nodeType); // 1
		console.log($foo.nodeName); // DIV

		// ํ์คํธ ๋ธ๋์ ๋ธ๋ ์ ๋ณด๋ฅผ ์ทจ๋ํ๋ค.
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeType); // 3
		console.log($textNode.nodeName); // #text
	</script>
</html>
```

<br>

## 39.5 ์์ ๋ธ๋์ ํ์คํธ ์กฐ์

### nodeValue

- ๋ธ๋ ๊ฐ์ฒด์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐํ๋ฉด **๋ธ๋ ๊ฐ์ฒด์ ๊ฐ์ ๋ฐํ**

- Node.prototype.nodeValue ํ๋กํผํฐ๋ **setter์ getter ๋ชจ๋ ์กด์ฌํ๋ ์ ๊ทผ์ ํ๋กํผํฐ**

<br>

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello</div>
	</body>
	<script>
		// ๋ฌธ์ ๋ธ๋์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐ
		console.log(document.nodeValue); // null

		// ์์ ๋ธ๋์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐ
		const $foo = document.getElementById("foo");
		console.log($foo.nodeValue); // null

		// ํ์คํธ ๋ธ๋์ nodeValue ํ๋กํผํฐ ์ฐธ์กฐ
		const $textNode = $foo.firstChild;
		console.log($textNode.nodeValue); // Hello
	</script>
</html>
```

- ํ์คํธ ๋ธ๋์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐํ  ๋๋ง ํ์คํธ ๋ธ๋์ ๊ฐ์ ๋ฐํ
  <Br>
- ํ์คํธ ๋ธ๋๊ฐ ์๋ ๊ฐ์ฒด์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐํ๋ฉด null์ ๋ฐํ
  <br>

- ์์ ๋ธ๋์ ํ์คํธ๋ฅผ ๋ณ๊ฒฝํ๋ ค๋ฉด ๋ค์๊ณผ ๊ฐ์ ์์์ ์ฒ๋ฆฌ๊ฐ ํ์ํจ

  1. `ํ์คํธ๋ฅผ ๋ณ๊ฒฝํ  ์์ ๋ธ๋๋ฅผ ์ทจ๋ํ ๋ค์, ์ทจ๋ํ ์์ ๋ธ๋์ ํ์คํธ ๋ธ๋๋ฅผ ํ์ํจ.`

     - ํ์คํธ ๋ธ๋๋ ์์ ๋ธ๋์ ์์ ๋ธ๋์ด๋ฏ๋ก firstChild ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ์ฌ ํ์
       <Br>

  2. `ํ์ํ ํ์คํธ ๋ธ๋์ nodeValue ํ๋กํผํฐ๋ฅผ ์ฌ์ฉํ์ฌ ํ์คํธ ๋ธ๋์ ๊ฐ์ ๋ณ๊ฒฝ`

<br>

```js
const $textNode = document.getElementById("foo").firstChild;

$textNode.nodeValue = "World";

console.log($textNode.nodeValue); // World
```

<br>

### textContent

> ์์ ๋ธ๋์ textContent ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐํ๋ฉด ์์ ๋ธ๋์ ์ฝํ์ธ  ์์ญ(์์ ํ๊ทธ์ ์ข๋ฃ ํ๊ทธ ์ฌ์ด) ๋ด์ ํ์คํธ๋ฅผ ๋ชจ๋ ๋ฐํ

<br>

- Node.prototype.textContent ํ๋กํผํฐ๋ setter์ getter ๋ชจ๋ ์กด์ฌํ๋ ์ ๊ทผ์ ํ๋กํผํฐ๋ก์ ์์ ๋ธ๋์ ํ์คํธ์ ๋ชจ๋  ์์ ๋ธ๋์ ํ์คํธ๋ฅผ ์ทจ๋ํ๊ฑฐ๋ ๋ณ๊ฒฝ
  <Br>

```html
<!DOCTYPE html>
<html>
	<body>
		<div id="foo">Hello <span>world!</span></div>
	</body>
	<script>
		// #foo ์์ ๋ธ๋์ ํ์คํธ๋ฅผ ๋ชจ๋ ์ทจ๋
		console.log(document.getElementById("foo").textContent); // Hello world!
	</script>
</html>
```

- textContent ํ๋กํผํฐ์ ์ ์ฌํ ๋์์ ํ๋ `innerText` ํ๋กํผํฐ๊ฐ ์์.
- innerText ํ๋กํผํฐ๋ ๋ค์๊ณผ ๊ฐ์ ์ด์ ๋ก ์ฌ์ฉํ์ง ์๋ ๊ฒ์ด ์ข์.

  - innerText ํ๋กํผํฐ CSS์ ์์ข์ 
    - CSS์ ์ํด ๋นํ์(visibility: hidden;)๋ก ์ง์ ๋ ์์ ๋ธ๋์ ํ์คํธ๋ฅผ ๋ฐํํ์ง ์์.
  - innerText ํ๋กํผํฐ๋ CSS๋ฅผ ๊ณ ๋ คํด์ผ ํ๋ฏ๋ก textContent ํ๋กํผํฐ๋ณด๋ค ๋๋ฆผ.
    <br>

## 39.6 DOM ์กฐ์

## 39.7 ์ดํธ๋ฆฌ๋ทฐํธ

## 39.8 ์คํ์ผ

## 39.9 DOM ํ์ค

- W3C๊ณผ WHATWG์ด๋ผ๋ ๋ ๋จ์ฒด์์ ๊ณตํต๋ ํ์ค์ ๋ง๋ฆ.

- DOM์ ํ์ฌ ๋ค์๊ณผ ๊ฐ์ด 4๊ฐ์ ๋ฒ์  ์กด์ฌ

| ๋ ๋ฒจ        | ํ์ค ๋ฌธ์ URL                          |
| ----------- | -------------------------------------- |
| DOM Level 1 | https://www.w3.org/TR/REC-DOM-Level-1  |
| DOM Level 2 | https://www.w3.org/TR/DOM-Level-2-Core |
| DOM Level 3 | https://www.w3.org/TR/DOM-Level-3-Core |
| DOM Level 4 | https://dom.spec.whatwg.org            |

<br><br>
\*\* ์ด๋ฏธ์ง ์ถ์ฒ : https://velog.io/@hangem422/js-web-node
https://velog.io/@lechuck/DOM-API
https://yung-developer.tistory.com/74
https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html
https://velog.io/@hangem422/js-web-node
