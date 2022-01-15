# 17. 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

> 💡 new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환

```js
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Kim';
person.sayHello = function () {
	console.log('Hello ' + this.name);
};

console.log(person); // { name: 'Kim', sayHello: f }
```

- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말함
- 생성자 함수에 의해 생성된 객체를 인스턴스라고 함
- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등 빌트인 생성자 함수를 제공

## 17.2 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

> 💡 단 하나의 객체만 생성
> => 동일한 프로퍼티를 갖는 객체를 생성기에는 비효율적

```js
const circle1 = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	},
};

const circle2 = {
	radius: 10,
	getDiameter() {
		return 2 * this.radius;
	},
};
```

- 객체의 프로퍼티는 객체 고유의 상태를 표현
- 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현

> 프로퍼티 값은 객체마자 다를 수 있지만 메서드는 동일한 경우가 대부분

### 생성자 함수에 의한 객체 생성 방식의 장점

> 💡 마치 객체(인스턴스)를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있음

```js
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

- `생성자 함수`는 이름 그대로 객체(인스턴스)를 생성하는 함수
  - 일반 함수와 동일한 방법으로 생성자 함수를 정의
  - **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작**

### 생성자 함수의 인스턴스 생성 과정

> 💡 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로서 동작
> ⇒ 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화 (인스턴스 프로퍼티 추가 및 초기값 할당)

**1. 인스턴스 생성과 this 바인딩**

- **암묵적으로 빈 객체가 생성**
  ⇒ (아직 완성 X) 생성자 함수가 생성한 인스턴스
- **빈 인스턴스는 this에 바인딩됨**
  ⇒ 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스르 가리키는 이유

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}
}
```

**2. 인스턴스 초기화**

- **생성자 함수의 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화**
  ⇒ this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가
  ⇒ 생성자 함수의 인수로 전달받은 값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}

	// 2. this에 바인딩되어 있는 인스턴스를 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}
```

**3. 인스턴스 반환**

- **생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환**
  - 생성자 함수 내부에서 return 문을 반드시 생략해야 함

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}

	// 2. this에 바인딩되어 있는 인스턴스를 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
	// 명시적으로 원시 값을 반환하면 무시됨
	return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환
const circle = new Circle(1);
console.log(circle); // Circle { radius: 1, getDiameter: f }
```

### 내부 메서드 \[[Call]]과 \[[Construct]]

- **함수는 객체**
  - 객체와 동일하게 동작

```js
// 함수는 객체
function foo() {}

foo.prop = 1;
foo.method = function () {
	console.log(this.prop);
};

foo.method(); // 1
```

- **일반 객체는 호출할 수 없지만 함수는 호출할 수 있음**
  ⇒ 일반 객체가 가지는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위한 함수 객체만을 위한 \[[Environment]], \[[FormalParameters]] 등의 내부 슬롯과 \[[Call]], \[[Construct]] 같은 내부 메서드를 추가로 가짐

<br>

- 일반 함수로서 호출되면 함수 객체의 내부 메서드 \[[Call]]이 호출
- new 연산자와 함께 생성자 함수로 호출되면 \[[Construct]]가 호출

```js
function foo() {}

// 일반적인 함수로서 호출: [[Call]] 이 호출
foo();

// 생성자 함수로서 호출: [[Construct]] 가 호출
new foo();
```

`callable`

- 내부 메서드 \[[Call]]을 갖는 함수 객체
- 호출할 수 있는 객체 ⇒ 함수

`constructor`

- 내부 메서드 \[[Constructor]]을 갖는 함수 객체
- 생성자 함수로서 호출할 수 있는 함수

`non-constructor`

- 내부 메서드 \[[Constructor]]을 갖지 않는 함수 객체
- 생성자 함수로서 호출할 수 없는 함수

> 💡 함수 객체는 **callable이면서 constructor** 이거나 **callable 이면서 non-constructor**

### constructor와 non-constructor의 구분

`constructor`

- 함수 선언문, 함수 표현식, 클래스(클래스도 함수)

`non-constructor`

- 메서드 (ES6 메서드 축약 표현), 화살표 함수

```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반함수로 정의된 함수
// 메서드가 아님
const baz = {
	x: function () {},
};

// 일반 함수로 정의된 함수만이 constructor
new foo();
new bar();
new baz.x(); // x {}

// 화살표 함수
const arrow = () => {};
new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의
const obj = {
	x() {},
};

new obj.x(); // TypeError: obj.x is not a constuctor
```

> ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미

```js
function foo() {}

// 일반 함수로서 호출
// [[Call]]이 호출됨 => 모든 함수 객체는 [[Call]]이 구현되어 있음
foo();

// 생성자 함수로서 호출
// [[Construct]] 가 호출됨. 이땐 [[Construct]]를 갖지 않는다면 에러 발생
new foo();
```

### new 연산자

> 💡 new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작

함수 객체의 내부 메서드 \[[Call]]이 호출되는 것이 아니라 \[[Constructor]]가 호출됨

```js
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
	return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하게 않았으므로 반환문이 무시
// 빈 객체를 반환
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
	return { name, role };
}

// new 와 함께 호출
inst = new createUser('Kim', 'user');
// 함수가 생성한 객체를 반환
console.log(inst); // {name: 'Kim', role: 'user'}
```

- new 연산자 없이 생성자함수를 호출하면 일반 함수로 호출됨
  - \[[Construct]]가 호출되는 것이 아니라 \[[Call]] 이 호출

```js
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function () {
		return 2 ** this.radius;
	};
}

// new 연산자 없이 생성자 함수 호출
// 일반 함수로서 호출됨
const circle = Circle(5);
console.log(circle); // undefined

// 일반함수의 내부에는 this에는 전역 객체를 가리킴
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read 'getDiameter' of undefined
```

### new.target

> 💡 this와 유사하게 constructor인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라 불림

- 함수 내부에서 **new.target** 을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인 가능
  - new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킴
  - new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined

```js
// 생성자 함수
function Circle(radius) {
	// new 연산자와 함께 호출되지 않았다면
	// new.target은 undefined 이다
	if (!new.target) {
		// new 연산자와 함께 생성자 함수를 재귀 호출함
		return new Circle(radius);
	}
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}
```

- **스코프 세이프 생성자 패턴**

```js
function Circle(radius) {
	/*
  생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈 객체를 생성
  this에 바인딩을 함 => 이때 this와 Circle은 프로토타입에 의해 연결됨
	*/
	/*
  이 함수가 new 연산자와 함께 호출되지 않았다면
  this는 전역 객체 window 를 가리킴
  즉, this와 Circle은 프로토타입에 의해 연결되지 않음
  */
	if (!(this instanceof Circle)) {
		return new Circle(radius);
	}
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}
```
