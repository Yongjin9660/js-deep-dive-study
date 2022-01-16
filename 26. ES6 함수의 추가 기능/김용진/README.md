# 26 ES6 함수의 추가 기능

## 26.1 함수의 구분

> 💡 ES6 이전까지 자바스크립트 함수는 별다른 구분 없이 다양한 목적으로 사용

```js
var foo = function () {
	return 1;
};

// 일반적인 함수 호출
foo(); // 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // 1
```

> 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출될 수 있음
> ⇒ 모든 함수가 callable이면서 constructor

ES6 에서의 함수

| ES6 함수의 구분     | constructor | prototype | super | argumets |
| ------------------- | :---------: | :-------: | :---: | :------: |
| 일반 함수 (Normal)  |      O      |     O     |   X   |    O     |
| 메서드 (Method)     |      X      |     X     |   O   |    O     |
| 화살표 함수 (Arrow) |      X      |     X     |   X   |    X     |

`일반 함수`

- 함수 선언문이나 함수 표현식으로 정의한 함수
- ES6 이전의 함수와 차이가 없음

## 26.2 메서드

> 💡 ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수를 말함

```js
const obj = {
	x: 1,
	// foo는 메서드
	foo() {
		return this.x;
	},
	// bar는 일반 함수
	bar: function () {
		return this.x;
	},
};

new obj.foo(); // TypeError: obj.foo is not a constructor
new obj.bar(); // bar {}
```

**ES6 사양에서 정의한 메서드**

- 인스턴스를 생성할 수 없는 non-contructor
- prototype 프로퍼티도 없고 프로토타입을 생성하지 않음
- 자신을 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]` 를 가짐
  - super 키워드 사용 가능

```js
const base = {
	name: 'Kim',
	sayHi() {
		return `Hi, ${this.name}`;
	},
};

const derived = {
	__proto__: base,
	sayHi() {
		return `${super.sayHi()}.`; // super 키워드 사용 가능
	},
};

console.log(derived.sayHi()); // Hi, Kim.
```

## 26.3 화살표 함수

### 화살표 함수의 정의

**함수 정의**

> 💡 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 함

```js
const add = (x, y) => x + y;
add(2, 3); // 5
```

**매개변수 선언**

> 💡 매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언 (한 개인 경우 생략 가능, 없는 경우 생략 불가능)

```js
const add = (x, y) => x + y;
add(2, 3); // 5

const arrow = () => {
	console.log('arrow');
};
```

**함수 몸체 정의**

> 💡 함수 몸체가 하나의 문이라면 중괄호 {} 생략 가능

```js
const power = (x) => x ** 2;
power(2); // 4

// 객체 리터럴을 반환하는 경우
// 소괄호 () 로 감쌈
const create = (id, content) => ({ id, content });
```

### 화살표 함수와 일반 함수의 차이

**1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor**

```js
const foo = () => {};

// prototype 프로퍼티도 없고 프로토타입도 생성하지 않음
Foo.hasOwnProperty('prototype'); // false

// 화살표 함수는 생성자 함수로서 호출할 수 없음
new Foo(); // TypeError: Foo is not a constructor
```

**2. 중복된 매개변수 이름을 선언할 수 없음**

- 일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않음
  - strict mode 에서는 에러 발생

```js
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

**3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않음**

> 💡 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 검색

### this

> 💡 함수를 호출할 때 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정

> 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 상위 스코프의 this를 참조

### super

> 💡 함수 자체의 super 바인딩을 갖지 않기 때문에 상위 스코프의 super를 참조

```js
class Base {
	constructor(name) {
		this.name = name;
	}
	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	// 화살표 함수의 super는 상위 스코프인 constructor의 super을 가리킴
	say = () => `${super.sayHi()}`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee
```

### arguments

> 💡 함수 자체의 arguments 바인딩을 갖지 않기 때문에 상위 스코프의 arguments를 참조

## 26.4 Rest 파라미터

**1. 기본 문법**

> 💡 Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받음

```js
function foo(...rest) {
	console.log(rest); // [1, 2, 3]
}

foo(1, 2, 3);

function bar(param, ...rest) {
	console.log(param); // [1]
	console.log(rest); // [2,3]
}

foo(1, 2, 3);

// 함수 객체의 length 프로퍼티에 영향을 주지 않음
console.log(foo.length); // 0
console.log(bar.length); // 1
```

**2. Rest 파라미터와 arguments 객체**

> 💡 화살표 함수는 자체의 arguments 객체를 갖지 않기때문에 가변 인자 함수를 구현하려면 Rest 파라미터를 사용해야 함

## 26.5 매개변수 기본값

```js
function sum(x = 0, y = 0) {
	return x + y;
}

console.log(sum(1)); // 1
```

- Rest 파라미터에는 기본값을 지정할 수 없음
