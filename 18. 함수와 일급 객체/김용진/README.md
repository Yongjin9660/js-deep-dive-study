# 18. 함수와 일급 객체

## 18.1 일급 객체

> 💡 자바스크립트의 함수는 일급 객체

**일급 객체**

- 무명의 리터럴로 생성 가능
  - 런타임에 생성 가능
- 변수나 자료구조(객체, 배열 등)에 저장 가능
- 함수의 매개변수에 전달 가능
- 함수의 반환값으로 사용 가능

```js
// 1. 무명의 리터럴로 생성 가능
// 2. 변수에 저장 가능
const increase = function (num) {
	return ++num;
};
const decrease = function (num) {
	return ++num;
};

// 2. 객체에 저장 가능
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달 가능
// 4. 함수의 반환값으로 사용 가능
function makeCounter(aux) {
	let num = 0;

	return function () {
		num = aux(num);
		return num;
	};
}

// 3. 함수는 매개변수에게 함수를 전달 가능
const increaser = makeCounter(auxs.increase);
const decreaser = makeCounter(auxs.decrease);
```

## 18.2 함수 객체의 프로퍼티

### arguments 프로퍼티

> 💡 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체

- 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수가 할당

arguments 객체는 매개변수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용

```js
function sum() {
	let res = 0;

	// arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회할 수 있음
	for (let i = 0; i < argumets.length; i++) {
		res += arguments[i];
	}
	return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

- arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 `유사 배열 객체`

`유사 배열 객체` : length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체를 말함

- 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생함
  - 배열메서드를 사용하려면
    - Function.prototype.call
    - Function.prototype.apply

```js
function sum() {
	// arguments 객체를 배열로 반환
	const array = Array.prototype.slice.call(arguments);
	return array.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}
```

- 이러한 번거로움을 해결하기 위해 Rest 파라미터를 도입

```js
function sum(...args) {
	return args.reduce((prev, cur) => prev + cur, 0);
}
```

### caller 프로퍼티

> 💡 함수 자신을 호출한 함수를 가리킴

### length 프로퍼티

> 💡 함수를 정의할 때 선언한 매개변수의 개수

```js
function foo() {}
console.log(foo.length); // 0

function bar(x) {
	return x;
}
console.log(bar.length); // 1

function baz(x, y) {
	return x * y;
}
console.log(baz.length); // 2
```

### name 프로퍼티

> 💡 함수 이름을 나타냄

```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); // foo

// 익명 함수 표현식
var annonymousFunc = function () {};
// ES5 : name 프로퍼티는 빈 문자열을 값으로 가짐
// ES6 : name 프로퍼티는 함수 객체를 가리키는 변수 이름을 값으로 가짐
console.log(annonymousFunc.name); // anonymousFunc

// 함수 선언문 (Function declaration)
function bar() {}
console.log(bar.name); // bar
```

### \_\_proto\_\_ 프로퍼티

> 💡 \[[Prototype]]이라는 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티

```js
const obj = { a: 1 };

console.log(obj.__proto__ === Object.prototype); // true

console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

`hasOwnProperty` : 인수로 전달받은 프로퍼티키가 객체 고유의 프로퍼티 키인 경우에만 true 를 반환

### prototype 프로퍼티

> 💡 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴

```js
// 함수 객체는 prototype 프로퍼티를 소유
(function () {}.hasOwnProperty('prototype')); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않음
({}.hasOwnProperty('prototype')); // false
```
