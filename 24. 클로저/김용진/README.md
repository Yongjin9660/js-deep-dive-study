# 24. 클로저

> 💡 클로저는 함수와 그 함수가 선언된 렉시컬 활경과의 조합

## 24.1 렉시컬 스코프

> 💡 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정

```js
const x = 1;

function foo() {
	const x = 10;
	bar();
}

function bar() {
	console.log(x);
}

foo(); // 1
bar(); // 1
```

## 24.2 함수 객체의 내부 슬롯 \[[Environment]]

> 💡 함수는 자신의 내부 슬롯 \[[Environment]] 에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장

**함수 객체의 내부 슬롯 \[[Environment]]**

- 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프
- 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 `외부 렉시컬 환경에 대한 참조`가 저장될 참조값

👉 상위 스코프를 기억

```js
const x = 1;

function foo() {
	const x = 10;

	// 상위 스코프는 함수 정의 환경에 따라 결정
	// 함수 호출 위치와 상위 스코프는 관련이 없음
	bar();
}

// 자신의 상위 스코프
// 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억
function bar() {
	console.log(x);
}

foo(); // 1
bar(); // 1
```

## 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

function outer() {
	const x = 10;
	const inner = function () {
		console.log(x);
	};
	return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환
// outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거
const fun = outer();
fun(); // 10
```

- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우
- 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있음
- 이러한 중첩함수를 **클로저**라고 부름

## 24.4 클로저의 활용

> 💡 상태를 안전하게 변경하고 유지하기 위해 사용

```js
const counter = (function () {
	let num = 0;
	return {
		plus() {
			return ++num;
		},
		minus() {
			return --num;
		},
	};
})();

console.log(counter.plus()); // 1;
console.log(counter.plus()); // 2;
console.log(counter.minus()); // 1;
```

👉 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용

## 24.5 캡슐화와 정보 은닉

> 💡 객체의 특정 프로퍼티나 메서드를 감추기 위해 사용

```js
function Person(name, age) {
	this.name = name; // public
	let _age = age; // private

	// 인스턴스 메서드
	this.sayHi = function () {
		console.log(`Hi! My name is ${this.name}. I am ${_age}`);
	};
}

const me = new Person('Kim', 20);
me.sayHi(); // Hi! My name is Kim. I am 20.
console.log(me.name); // Kim
console.log(me._age); // undefined
```

- name 프로퍼티는 자유롭게 참조하거나 변경 가능
- \_age 변수는 Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경할 수 없음
  - \_age 변수는 private

**sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성됨**

```js
function Person(name, age) {
	this.name = name; // public
	let _age = age; // private
}

Person.prototype.sayHi = function () {
	// Person 생성자 함수의 지역 변수 _age를 참조할 수 없음
	console.log(`Hi! My name is ${this.name}. I am ${_age}`);
};
```

👉 **따라서 즉시 실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모아야 함**

```js
const Person = (function () {
	let _age = 0; // private

	// 생성자 함수
	function Person(name, age) {
		this.name = name; // public
		_age = age;
	}

	// 프로토타입 메서드
	Person.prototype.sayHi = function () {
		console.log(`Hi! My name is ${this.name}. I am ${_age}`);
	};

	// 생성자 함수를 반환
	return Person;
})();

const me = new Person('Kim', 20);
me.sayHi(); // Hi! My name is Kim. I am 20.
console.log(me.name); // Kim
console.log(me._age); // undefined
```

하지만 위의 코드도 문제가 있음

- Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 \_age 변수의 상태가 유지되지 않음

```js
const me = new Person('Kim', 20);
me.sayHi(); // Hi! My name is Kim. I am 20.

const you = new Person('Lee', 30);
you.sayHi(); // Hi! My name is Lee. I am 30.

// _age 변수 값이 변경
me.sayHi(); // Hi! My name is Kim. I am 30.
```

- Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상

## 24.6 자주 발생하는 실수

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
	funcs[i] = function () {
		return i;
	};
}

for (var j = 0; j < funcs.length; j++) {
	console.log(funcs[j]()); // 3 3 3
}
```

- for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 전역 변수
- 전역 변수 i 에는 0, 1, 2가 순차적으로 할당됨
- 함수를 호출하면 전역 변수 i 를 참조하여 i의 값 3이 출력

**클로저를 사용해 수정**

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
	funcs[i] = (function (id) {
		return function () {
			return id;
		};
	})(i);
}

for (var i = 0; funcs.length < 3; i++) {
	console.log(funcs[i]()); // 0 1 2
}
```

- var 키워드로 선언한 변수가 전역 변수가 되기 때문에 발생하는 현상임

**ES6의 let 키워드를 사용하면 해결**

```js
const funcs = [];

for (let i = 0; i < 3; i++) {
	funcs[i] = function () {
		return i;
	};
}

for (let i = 0; i < funcs.length; i++) {
	console.log(funcs[i]); // 0 1 2
}
```

for 문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 실행될 때마다 for 문 코드 블록의새로운 렉시컬 환경이 생성
