# 22. this

## 22.1 this ํค์๋

> ๐ก ์์ ์ด ์ํ ๊ฐ์ฒด ๋๋ ์์ ์ด ์์ฑํ  ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํค๋ **์๊ธฐ ์ฐธ์กฐ ๋ณ์**

- ์๋ฐ์คํฌ๋ฆฝํธ ์์ง์ ์ํด ์๋ฌต์ ์ผ๋ก ์์ฑ โ ์ด๋์๋  ์ฐธ์กฐ ๊ฐ๋ฅ
- **this ๋ฐ์ธ๋ฉ์ ํจ์ ํธ์ถ ๋ฐฉ์์ ์ํด ๋์ ์ผ๋ก ๊ฒฐ์ **

```js
// ๊ฐ์ฒด ๋ฆฌํฐ๋ด
const circle = {
	radius: 5,
	getDiameter() {
		// this๋ ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํด
		return 2 * this.radius;
	},
};
console.log(circle.getDiameter()); // 10
```

- ๊ฐ์ฒด ๋ฆฌํฐ๋ด์ ๋ฉ์๋ ๋ด๋ถ์์์ `this` โ ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํด

```js
// ์์ฑ์ ํจ์
function Circle(radius) {
	// this๋ ์์ฑ์ ํจ์๊ฐ ์์ฑํ  ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํด
	this.radius = radius;
}
Circle.prototype.getDiameter = function () {
	// this๋ ์์ฑ์ ํจ์๊ฐ ์์ฑํ  ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํจ๋ค.
	return 2 * this.radius;
};

// ์ธ์คํด์ค ์์ฑ
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

- ์์ฑ์ ํจ์ ๋ด๋ถ์ `this` โ ์์ฑ์ ํจ์๊ฐ ์์ฑํ  ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํด

## 22.2 ํจ์ ํธ์ถ ๋ฐฉ์๊ณผ this ๋ฐ์ธ๋ฉ

> ๐ก this ๋ฐ์ธ๋ฉ์ ํจ์ ํธ์ถ ๋ฐฉ์, ์ฆ ํจ์๊ฐ ์ด๋ป๊ฒ ํธ์ถ๋์๋์ง์ ๋ฐ๋ผ ๋์ ์ผ๋ก ๊ฒฐ์ ๋จ

##### ํจ์ ํธ์ถ ๋ฐฉ์

- ์ผ๋ฐ ํจ์ ํธ์ถ
- ๋ฉ์๋ ํธ์ถ
- ์์ฑ์ ํจ์ ํธ์ถ
- Function.prototype.apply/call/bind ๋ฉ์๋์ ์ํ ๊ฐ์  ํธ์ถ

### ์ผ๋ฐ ํจ์ ํธ์ถ

> ๐ก ๊ธฐ๋ณธ์ ์ผ๋ก ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ๋ฉ๋จ

```js
function foo() {
	console.log(this); // window
	function bar() {
		console.log(this); // window
	}
	bar();
}
foo();
```

- ๋ฉ์๋ ๋ด์์ ์ ์ํ ์ค์ฒฉ ํจ์๋ ์ผ๋ฐ ํจ์๋ก ํธ์ถ๋๋ฉด this์๋ ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ์ธ๋จ

```js
// var ํค์๋๋ก ์ ์ธํ ์ ์ญ ๋ณ์ value๋ ์ ์ญ ๊ฐ์ฒด์ ํ๋กํผํฐ
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // {value: 100, foo: f}
		console.log(this.value); // 100

		// ๋ฉ์๋ ๋ด์์ ์ ์ํ ์ค์ฒฉ ํจ์
		function bar() {
			console.log(this); // window
			console.log(this.value); // 1
		}

		bar();
	},
};

obj.foo();
```

- ์ด๋ค ํจ์๋ผ๋ ์ผ๋ฐ ํจ์๋ก ํธ์ถ๋๋ฉด this์๋ ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ๋ฉ๋จ

### ๋ฉ์๋ ํธ์ถ

> ๐ก ๋ฉ์๋ ๋ด๋ถ์ this๋ ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด์ ๋ฐ์ธ๋ฉ

```js
const person = {
	name: 'Lee',
	getName() {
		return this.name;
	},
};

console.log(person.getName()); // Lee
```

- ํ๋กํ ํ์ ๋ฉ์๋ ๋ด๋ถ์์ ์ฌ์ฉ๋ this๋ ์ผ๋ฐ ๋ฉ์๋์ ๋ง์ฐฌ๊ฐ์ง๋ก ํด๋น ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด์ ๋ฐ์ธ๋ฉ

```js
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
};

const me = new Perons('Lee');

// getName ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ me
console.log(me.getName()); // Lee
Person.prototype.name = 'Kim';

// getName ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ Person.prototype
console.log(Person.prototype.getName()); // Kim
```

### ์์ฑ์ ํจ์ ํธ์ถ

> ๐ก ์์ฑ์ ํจ์ ๋ด๋ถ์ this์๋ ๋ฏธ๋์ ์์ฑํ  ์ธ์คํด์ค๊ฐ ๋ฐ์ธ๋ฉ๋จ

```js
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

const c1 = new Circle(5);
const c2 = new Circle(10);

console.log(c1.getDiameter()); // 10
console.log(c1.getDiameter()); // 20
```

### Function.prototype.apply/call/bind ๋ฉ์๋์ ์ํ ๊ฐ์  ํธ์ถ

> ๐ก this๋ก ์ฌ์ฉํ  ๊ฐ์ฒด์ ์ธ์ ๋ฆฌ์คํธ๋ฅผ ์ธ์๋ก ์ ๋ฌ๋ฐ์ ํจ์๋ก ํธ์ถ

```js
/**
 * ์ฃผ์ด์ง this ๋ฐ์ธ๋ฉ๊ณผ ์ธ์ ๋ฆฌ์คํธ ๋ฐฐ์ด์ ์ฌ์ฉํ์ฌ ํจ์ ํธ์ถ
 * @param thisArg - this๋ก ์ฌ์ฉํ  ๊ฐ์ฒด
 * @param argsArray - ํจ์์๊ฒ ์ ๋ฌํ ์ธ์ ๋ฆฌ์คํธ์ ๋ฐฐ์ด ๋๋ ์ ์ฌ ๋ฐฐ์ด ๊ฐ์ฒด
 * @returns ํธ์ถํ ํจ์์ ๋ฐํ๊ฐ
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * ์ฃผ์ด์ง this ๋ฐ์ธ๋ฉ๊ณผ ,๋ก ๊ตฌ๋ถ๋ ์ธ์ ๋ฆฌ์คํธ๋ฅผ ์ฌ์ฉํ์ฌ ํจ์ ํธ์ถ
 * @param thisArg - this๋ก ์ฌ์ฉํ  ๊ฐ์ฒด
 * @param arg1, arg2, ... - ํจ์์๊ฒ ์ ๋ฌํ  ์ธ์ ๋ฆฌ์คํธ
 * @returns ํธ์ถ๋ ํจ์์ ๋ฐํ๊ฐ
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

- apply์ call ๋ฉ์๋์ ๋ณธ์ง์ ์ธ ๊ธฐ๋ฅ์ ํจ์ ํธ์ถ

```js
function getThisBinding() {
	console.log(arguments);
	return this;
}

// this๋ก ์ฌ์ฉํ  ๊ฐ์ฒด
const thisArg = { a: 1 };

// getThisBinding ํจ์๋ฅผ ํธ์ถํ๋ฉด์ ์ธ์๋ก ์ ๋ฌํ ๊ฐ์ฒด๋ฅผ getThisBinding ํจ์์ this์ ๋ฐ์ธ๋ฉ
// apply ๋ฉ์๋๋ ํธ์ถํ ํจ์์ ์ธ์๋ฅผ ๋ฐฐ์ด๋ก ๋ฌถ์ด ์ ๋ฌ
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}

// call ๋ฉ์๋๋ ํธ์ถํ  ํจ์์ ์ธ์๋ฅผ ์ผํ๋ก ๊ตฌ๋ถํ ๋ฆฌ์คํธ ํ์์ผ๋ก ์ ๋ฌ
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}
```

| ํจ์ ํธ์ถ ๋ฐฉ์                                             | this ๋ฐ์ธ๋ฉ                                                           |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| ์ผ๋ฐ ํจ์ ํธ์ถ                                             | ์ ์ญ ๊ฐ์ฒด                                                             |
| ๋ฉ์๋ ํธ์ถ                                                | ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด                                                  |
| ์์ฑ์ ํจ์ ํธ์ถ                                           | ์์ฑ์ ํจ์๊ฐ (๋ฏธ๋์) ์์ฑํ  ์ธ์คํด์ค                                |
| Function.prototype.apply/call/bind ๋ฉ์๋์ ์ํ ๊ฐ์  ํธ์ถ | Function.prototype.apply/call/bind ๋ฉ์๋์ ์ฒซ๋ฒ์งธ ์ธ์๋ก ์ ๋ฌํ ๊ฐ์ฒด |
