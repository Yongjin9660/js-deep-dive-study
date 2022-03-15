# 22. this

## 22.1 this 키워드

> 💡 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**

- 자바스크립트 엔진에 의해 암묵적으로 생성 ⇒ 어디서든 참조 가능
- **this 바인딩은 함수 호출 방식에 의해 동적으로 결정**

```js
// 객체 리터럴
const circle = {
	radius: 5,
	getDiameter() {
		// this는 메서드를 호출한 객체를 가리킴
		return 2 * this.radius;
	},
};
console.log(circle.getDiameter()); // 10
```

- 객체 리터럴의 메서드 내부에서의 `this` → 메서드를 호출한 객체를 가리킴

```js
// 생성자 함수
function Circle(radius) {
	// this는 생성자 함수가 생성할 인스턴스를 가리킴
	this.radius = radius;
}
Circle.prototype.getDiameter = function () {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

- 생성자 함수 내부의 `this` → 생성자 함수가 생성할 인스턴스를 가리킴

## 22.2 함수 호출 방식과 this 바인딩

> 💡 this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정됨

##### 함수 호출 방식

- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 일반 함수 호출

> 💡 기본적으로 전역 객체가 바인딩됨

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

- 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 this에는 전역 객체가 바인인됨

```js
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // {value: 100, foo: f}
		console.log(this.value); // 100

		// 메서드 내에서 정의한 중첩 함수
		function bar() {
			console.log(this); // window
			console.log(this.value); // 1
		}

		bar();
	},
};

obj.foo();
```

- 어떤 함수라도 일반 함수로 호출되면 this에는 전역 객체가 바인딩됨

### 메서드 호출

> 💡 메서드 내부의 this는 메서드를 호출한 객체에 바인딩

```js
const person = {
	name: 'Lee',
	getName() {
		return this.name;
	},
};

console.log(person.getName()); // Lee
```

- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩

```js
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
};

const me = new Perons('Lee');

// getName 메서드를 호출한 객체는 me
console.log(me.getName()); // Lee
Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype
console.log(Person.prototype.getName()); // Kim
```

### 생성자 함수 호출

> 💡 생성자 함수 내부의 this에는 미래에 생성할 인스턴스가 바인딩됨

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

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출

> 💡 this로 사용할 객체와 인수 리스트를 인수로 전달받아 함수로 호출

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수 호출
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달한 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출한 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수 호출
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```

- apply와 call 메서드의 본직적인 기능은 함수 호출

```js
function getThisBinding() {
	console.log(arguments);
	return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩
// apply 메서드는 호출한 함수의 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}
```

| 함수 호출 방식                                             | this 바인딩                                                           |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                             |
| 메서드 호출                                                | 메서드를 호출한 객체                                                  |
| 생성자 함수 호출                                           | 생성자 함수가 (미래에) 생성할 인스턴스                                |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |
