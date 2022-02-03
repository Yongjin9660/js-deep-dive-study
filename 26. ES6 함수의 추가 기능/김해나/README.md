# 26. ES6 함수의 추가 기능

## 26.1 함수의 구분

```jsx
var foo = function () {
	return 1;
};

// 일반적인 함수로서 호출
foo(); // 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // 1
```

- ES6 이전의 함수는 사용 목적에 따라 명확히 구분 X
  - 일반 함수로서 호출할 수 있는 것은 생성자 함수로서도 호출 가능
  - `ES6 이전의 모든 함수는 callable 이면서 constructor`
    <br>

##### \*\* **callable 이란?**

> 호출할 수 있는 함수 객체

##### \*\* **constructor 이란?**

> 인스턴스를 생성할 수 있는 함수 객체

<br>

```jsx
// 프로퍼티 f 에 바인딩된 함수는 callable 이며 constructor
var obj = {
	x: 10,
	f: function () {
		return this.x;
	},
};

// 프로퍼티 f 에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f 에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f 에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

- ES6 이전에 메서드라고 부르던 객체에 바인딩된 함수도 callable & constructor 인 것은 문제 O

  - 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 의미
    <br>

- `ES6 이전의 모든 함수는 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성`

  - 이는 혼란스러우며 실수 유발 가능성 ↑
    <br>

#### \* **ES6 에서는 사용 목적에 따라 함수를 3가지 종류로 명확히 구분**

| ES6 함수의 구분     | constructor | prototype | super | arguments |
| ------------------- | ----------- | --------- | ----- | --------- |
| 일반 함수 (Normal)  | O           | O         | X     | O         |
| 메서드 (Method)     | X           | X         | O     | O         |
| 화살표 함수 (Arrow) | X           | X         | X     | X         |

<br>

## 26.2 메서드

> **ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미**

<br>

```jsx
const obj = {
	x: 1,

	// foo 는 메서드
	foo() {
		return this.x;
	},

	// bar 에 바인딩된 함수는 메서드가 아닌 일반 함수
	bar: function () {
		return this.x;
	},
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

// ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor
new obj.foo(); // TypeError
new obj.bar(); // bar {}

// obj.foo 는 constructor 가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty("prototype"); // false

// obj.bar 는 constructor 인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty("prototype"); // true
```

- **ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor**

  - ES6 메서드는 생성자 함수로서 호출 X
  - ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성 X
    <br>

- **표준 빌트인 객체가 제공하는 프로토타입 메서드/정적 메서드 모두 non-constructor**
  - String.prototype.toUpperCase.prototype → undefined
  - String.fromCharCode.prototype → undefined
    <br>

```jsx
const parent = {
	name: "Lee",
	sayHi() {
		return `Hi! ${this.name}`;
	},
};

const child1 = {
	__proto__: base,

	// sayHi 는 ES6 메서드 -> 내부 슬롯 [[HomeObject]] 를 가진다.
	// sayHi 의 [[HomeObject]] 는 sayHi 가 바인딩된 객체인 child1 을 가리키고
	// super 는 sayHi 의 [[HomeObject]] 의 프로토타입인 parent 를 가리킨다.
	sayHi() {
		return `${super.sayHi()}. how are you doing?`;
	},
};

const child2 = {
	__proto__: base,

	// sayHi 는 ES6 메서드 X -> 내부 슬롯 [[HomeObject]] 를 가지지 않는다.
	// sayHi 는 super 키워드 사용 X
	sayHi: function () {
		return `${super.sayHi()}. how are you doing?`;
		// SyntaxError : 'super' keyword unexpected here
	},
};

console.log(child1.sayHi()); // Hi! Lee. how are you doing?
```

- **ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 \[[HomeObject]] 를 가짐**

  - super 참조는 내부 슬롯 \[[HomeObject]] 를 사용하여 수퍼클래스의 메서드 참조
  - ES6 메서드가 아닌 함수는 \[[HomeObject]] 를 갖지 않기 때문에 super 키워드 사용 X
    <br>

- `ES6 메서드는 본연의 기능 (super) 을 추가하고 의미적으로 맞지 않는 기능 (constructor) 은 제거`
  <br>

## 26.3 화살표 함수

> `function 키워드 대신 화살표 => 를 사용하여 기존의 함수 정의 방식보다 간략하게 함수 정의 가능`
> → 콜백 함수 내부에서 this 가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용

<br>

#### 1) 화살표 함수 정의

##### 함수 정의

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3); // 6
```

- `화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 함.`
  <br>

##### 매개변수 선언

```jsx
// 매개변수가 없는 경우
const arrow = () => { ... };

// 매개변수가 1개인 경우
const arrow = x => { ... };

// 매개변수가 여러 개인 경우
const arrow = (x, y) => { ... };

```

- `매개변수가 여러 개인 경우 소괄호 () 안에 매개변수 선언`
  - 매개변수가 없는 경우 소괄호 생략 X
  - 매개변수가 1개인 경우 소괄호 생략 O
    <br>

##### 함수 몸체 정의

```jsx
// 함수 몸체가 표현식인 문으로 구성된 경우
const power = (x) => x ** 2;
power(2); // 4

// 함수 몸체가 표현식이 아닌 문인 경우
const arrow = () => {
	const x = 1;
};

// 함수 몸체가 여러 개의 문인 경우
const sum = (a, b) => {
	const result = a + b;
	return result;
};

// 객체 리터럴을 반환하는 경우
const create = (id, content) => ({ id, content });
create(1, "JavaScript"); // { id : 1, content : 'JavaScript" }
```

- `함수 몸체가 표현식인 문이라면 함수 몸체를 감싸는 중괄호 생략 O`

  - 함수 몸체의 문이 표현식이 아닌 문이라면 함수 몸체를 감싸는 중괄호 생략 X
  - 함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 생략 X
    <br>

- `객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 () 로 감싸줘야 함.`
  - 소괄호로 감싸지 않으면 객체 리터럴의 중괄호를 함수 몸체를 감싸는 중괄호로 잘못 해석
    <br>

```jsx
// 화살표 함수를 즉시 실행 함수로 사용한 경우
const person = ((name) => ({
	sayHi() {
		return `Hi? My name is ${name}.`;
	},
}))("Lee");

console.log(person.sayHi()); // Hi? My name is Lee.

// 화살표 함수를 고차 함수에 인수로 전달한 경우
// ES5
[1, 2, 3].map(function (v) {
	return v * 2;
});

// ES6
[1, 2, 3].map((v) => v * 2); // [ 2, 4, 6 ]
```

- `화살표 함수는 즉시 실행 함수로도 사용 가능`
- `화살표 함수도 일급 객체이므로 map/filter/reduce 와 같은 고차 함수에 인수로 전달 가능`
  - 콜백 함수로서 정의할 때 유용
    <br>

#### 2) 화살표 함수와 일반 함수의 차이

```jsx
// 화살표 함수는 생성자 함수로서 호출 X
// -> 인스턴스 생성 불가능
const Foo = () => {};
new Foo(); // TypeError

Foo.hasOwnProperty("prototype"); // false
```

- **화살표 함수는 인스턴스를 생성할 수 없는 non-constructor**
  - prototype 프로퍼티가 없고 프로토타입도 생성하지 않음.
    <br>

```jsx
// 일반 함수
function normal(a, a) {
  return a + a;
}
console.log(normal(1, 2));    // 4

// 일반 함수 (strict mode)
'use strict';

function normal(a, a) {
  return a + a;   // SyntaxError
}

// 화살표 함수
const arrow => (a, a) => a + a; // SyntaxError
```

- **화살표 함수는 중복된 매개변수 이름을 선언할 수 없음.**

  - 일반 함수는 strict mode 가 아닌 이상 중복된 매개변수 이름 선언 가능
    <br>

- **화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않음.**
  - 화살표 함수 내부에서 this, arguments, super, new.target 을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target 참조
    <br>

#### 3) this

> `함수 호출 방식에 따라 this 에 바인딩할 객체가 동적으로 결정됨.`

<br>

```jsx
class Prefixer {
	constructor(prefix) {
		this.prefix = prefix;
	}

	add(arr) {
		// add 메서드는 인수로 전달된 배열 arr 을 순회하며 배열의 모든 요소에 prefix 추가
		// 1번

		return arr.map(function (item) {
			return this.prefix + item; // 2번
			// -> TypeError
		});
	}
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

- 1번 this → add 메서드 호출한 prefixer 객체 바인딩
- 2번 this → 일반 함수 내부이므로 undefined 바인딩
- `콜백 함수의 this (2번) 와 외부 함수의 this (1번) 가 서로 다른 값을 가리킴. → TypeError 발생`
  <br>

\*\* **`콜백 함수 내부의 this 바인딩 문제를 해결하기 위해 ES6 화살표 함수 사용`**

```jsx
class Prefixer {
	constructor(prefix) {
		this.prefix = prefix;
	}

	add(arr) {
		return arr.map((item) => this.prefix + item);
	}
}

const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
// ['-webkit-transition', '-webkit-user-select']
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음.

  - `화살표 함수 내부에서 this 를 참조하면 상위 스코프의 this 참조 (lexical this)`
  - `lexical this` : 화살표 함수의 this 가 함수가 정의된 위치에 의해 결정된다는 것을 의미
    <br>

- 화살표 함수를 제외한 모든 함수에는 this 바인딩 존재
  - `ES6 화살표 함수가 도입되기 이전에는 스코프 체인을 통해 this 를 탐색할 필요 X`
    <br>

```jsx
// 전역 함수 foo 의 상위 스코프는 전역
// 화살표 함수 foo 의 this 는 전역 객체를 가리킨다.
const foo = () => console.log(this);
foo(); // window

// increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역
// increase 프로퍼티에 할당한 화살표 함수의 this 는 전역 객체를 가리킨다.
const counter = {
	num: 1,
	increase: () => ++this.num,
};

console.log(counter.increase()); // NaN
```

- **화살표 함수가 전역 함수라면 화살표 함수의 this 는 전역 객체를 가리킴.**

  - 전역 함수의 상위 스코프는 전역이고, 전역에서 this 는 전역 객체를 가리키기 때문
    <br>

- **화살표 함수와 화살표 함수가 중첩**되어 있거나 **프로퍼티에 할당한 화살표 함수** 는

  - 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수의 this 참조
    <br>

```jsx
window.x = 1;

// 일반 함수
const normal = function () {
	return this.x;
};

// 화살표 함수
const arrow = () => this.x;

console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 })); // 1
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 `call/apply/bind 메서드를 사용해도 화살표 함수 내부의 this 교체 X`
  - 화살표 함수가 call/apply/bind 메서드를 호출할 수 없다는 의미는 아님.
    <br>

```jsx
// Bad (메서드를 화살표 함수로 정의)
const person = {
	name: "Lee",
	sayHi: () => console.log(`Hi! ${this.name}`),
};

person.sayHi(); // Hi

// Good (메서드를 ES6 메서드 축약 표현으로 정의)
const person = {
	name: "Lee",
	sayHi() {
		console.log(`Hi! ${this.name}`);
	},
};

person.sayHi(); // Hi Lee
```

```jsx
// Bad (클래스 필드에 화살표 함수 할당)
class Person {
	// 클래스 필드 정의 제안
	name = "Lee";
	sayHi = () => console.log(`Hi! ${this.name}`);
}

const person = new Person();
person.sayHi(); // Hi Lee

// Good (메서드를 ES6 메서드 축약 표현으로 정의)
class Person {
	// 클래스 필드 정의
	name = "Lee";
	sayHi() {
		console.log(`Hi! ${this.name}`);
	}
}

const person = new Person();
person.sayHi(); // Hi Lee
```

```jsx
// Bad (프로토타입 객체의 프로퍼티에 화살표 함수 할당)
function Person(name) {
	this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi! ${this.name}`);

const person = new Person("Lee");

// 브라우저에서 실행하면 this.name 은 빈 문자열을 갖는 window.name 과 같다.
person.sayHi(); // Hi

// Good (프로퍼티를 동적 추가할 때는 일반 함수 할당)
function Person(name) {
	this.name = name;
}

Person.prototype.sayHi = function () {
	console.log(`Hi! ${this.name}`);
};

const person = new Person("Lee");
person.sayHi(); // Hi Lee
```

**[피해야 하는 경우]**

- 메서드를 화살표 함수로 정의하는 경우

  `→ 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드 사용하는 것이 좋음.`
  <br>

- 클래스 필드에 화살표 함수를 할당하는 경우

  `→ 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드 사용하는 것이 좋음.`
  <br>

- 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우
  `→ 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 됨.`
  `→ 프로퍼티를 동적으로 추가할 때는 일반 함수로 정의하는 것이 좋음.`
  <br>

#### 4) super

> 화살표 함수는 함수 자체의 super 바인딩을 갖지 않음.
> `→ 화살표 함수 내부에서 super 를 참조하면 상위 스코프의 super 참조`

<br>

```jsx
class Base {
	constructor(name) {
		this.name = name;
	}

	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	// 화살표 함수의 super 는 상위 스코프인 constructor 의 super 를 가리킨다.
	sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

- super 는 내부 슬롯 \[[HomeObject]] 를 갖는 ES6 메서드 내에서만 사용 가능한 키워드
  - `클래스 필드에 할당한 화살표 함수 내부에서 super 를 참조하면 constructor 내부의 super 바인딩 참조`

<br>

#### 5) arguments

> 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않음.
> `→ 화살표 함수 내부에서 arguments 를 참조하면 상위 스코프의 arguments 참조`

<br>

\*\* **arguments 객체란?**

> `호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체`
> → 함수 내부에서 지역 변수처럼 사용 가능

<br>

```jsx
(function () {
	// 화살표 함수 foo 의 arguments 는 상위 스코프인 즉시 실행 함수의 arguments 를 가리킨다.
	const foo = () => console.log(arguments); // [Arguments] { '0' : 1, '1' : 2 }
	foo(3, 4);
})(1, 2);

// 화살표 함수 foo 의 arguments 는 상위 스코프인 전역의 arguments 를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError
```

- `화살표 함수에서는 arguments 객체 사용 X`
  - 상위 스코프의 arguments 객체를 참조할 수는 있지만 <em><u>화살표 함수 자신에게 전달된 인수가 아닌 상위 함수에게 전달된 인수 목록만을 참조</u></em> 할 수 있으므로 그다지 도움되지 않음.
    <br>
- `화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터 사용`

<br>

## 26.4 Rest 파라미터

> `Rest 파라미터 (나머지 매개변수) 는 매개변수 이름 앞에 세개의 점 ... 을 붙여서 정의한 매개변수 의미`

<br>

#### 1) 기본 문법

```jsx
function foo(param1, param2, ...rest) {
	console.log(param1); // 1
	console.log(param2); // 2
	console.log(rest); // [3, 4, 5]
}

foo(1, 2, 3, 4, 5);

// Rest 파라미터가 마지막 파라미터가 아닌 경우
function foo(...rest, param1, param2) { }    // SyntaxError

// Rest 파라미터를 여러 개 선언한 경우
function foo(...rest1, ...rest2) { }  // SyntaxError

// length 프로퍼티
function bar(...rest) {}
console.log(bar.length);    // 0

function bar(x, ...rest) {}
console.log(bar.length);    // 1

function bar(x, y, ...rest) {}
console.log(bar.length);    // 2
```

- Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받음.

  - `함수에 전달된 인자들은 일반 매개변수와 Rest 파라미터에 순차적으로 할당됨.`
    <br>

- 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당됨.

  - Rest 파라미터는 반드시 마지막 파라미터여야 함.
    <br>

- Rest 파라미터는 단 하나만 선언 가능
  <br>

- Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 length 프로퍼티에 영향 X
  <br>

#### 2) Rest 파라미터와 arguments 객체

<br>

```jsx
// 매개변수의 개수를 사전에 알 수 없는 가변 인자 함수
function sum() {
	// 가변 인자 함수는 arguments 객체를 통해 인수를 전달받는다.
	console.log(arguments);
}

sum(1, 2); // { length : 2, '0' : 1, '1' : 2 }
```

- ES5 에서는 함수 정의 시 매개변수의 개수를 확정할 수 없는 가변 인자 함수의 경우 매개변수를 통해 인수를 전달받는 것이 불가능
  → arguments 객체를 활용하여 인수를 전달받음.
  <br>

```jsx
function sum() {
	// 유사 배열 객체인 arguments 객체를 배열로 변환
	var array = Array.prototype.slice.call(arguments);

	return array.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

- ES6 이전에는 배열 메서드를 사용하려면 call/apply 메서드를 통해 arguments 객체를 배열로 변환해야 하는 번거로움 O
  <br>

```jsx
function sum(...args) {
	// Rest 파라미터 args 에는 배열 [1, 2, 3, 4, 5] 가 할당된다.
	return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

- ES6 에서는 Rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받음.
  - `유사 배열 객체인 arguments 객체를 배열로 변환하는 번거로움 X`
    <br>
- 함수와 ES6 메서드는 Rest 파라미터와 arguments 객체 모두 사용 가능
  - 화살표 함수는 함수 자체의 arguments 객체를 갖지 않기 때문에 화살표 함수로 가변 인자 함수를 구현할 때는 반드시 Rest 파라미터 사용
    <br>

## 26.5 매개변수 기본값

> 자바스크립트 엔진은 매개변수의 개수와 인수의 개수를 체크하지 않음.
> `→ ES6 에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화 간소화 가능`  
> `→ Rest 파라미터는 기본값 지정 X`

<br>

```jsx
// ES6 이전
function sum(x, y) {
	// 인수가 전달되지 않아 매개변수의 값이 undefined 인 경우 기본값 할당
	x = x || 0;
	y = y || 0;

	return x + y;
}

console.log(sum(1)); // 1

// ES6 에서 도입된 매개변수 기본값
function logName(name = "Lee") {
	console.log(name);
}

logName(); // Lee
logName(undefined); // Lee
logName(null); // null
```

- 매개변수 기본값은 인수를 전달하지 않은 경우와 undefined 를 전달한 경우에만 유효
  <br>

```jsx
function sum(x, y, z = 0) {
	console.log(arguments);
}

console.log(sum.length); // 2

sum(1); // Arguments { '0' : 1 }
sum(1, 2); // Arguments { '0' : 1, '1' : 2 }
```

- 매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 `length 프로퍼티와 arguments 객체에 아무런 영향 X`
