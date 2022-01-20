# 19. 프로토타입

> 💡 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 **모든 것**이 객체

## 19.1 객체지향 프로그래밍

> 💡 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말함

- 실세계의 실체(사물이나 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도
- **속성**
  - 특징이나 성질
  - 이를 통해 실체를 인식하거나 구별
- **추상화**
  - 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것

```js
const circle = {
	radius: 5, // 반지름
	getDiameter() {
		return 2 * this.radius;
	},
};
```

`객체`

- 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조

## 19.2 상속과 프로토타입

> 💡 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

```js
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getArea = function () {
		return Math.PI * this.radius ** 2;
	};
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
```

- Circle 생성자 함수가 생성하는 모든 객체(인스턴스)는 radius 프로퍼티와 getArea 메서드를 가짐
  - getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용
  - Circle 생성자 함수가 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성함 (메모리 낭비)
    ⇒ **단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직**

```js
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가
Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성하는 모든 인스턴스는 단 하나의 메서드를 공유
console.log(circle1.getArea === circle2.getArea);
```

> 💡 Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입으로 부터 모든 프로퍼티와 메서드를 상속 받음

## 19.3 프로토타입 객체

> 💡 프로토타입 객체란 객체 간 상속을 구현하기 위해 사용됨

- 모든 객체는 \[[Prototype]]이라는 내부 슬롯을 가짐
- \[[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정됨
  - 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype
  - 생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체임

### \_\_proto\_\_ 접근자 프로퍼티

> 💡 모든 객체는 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입(\[[Prototype]])에 간접적으로 접근 가능

**1. \_\_proto\_\_는 접근자 프로퍼티**

```js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x);
```

**2. \_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용됨**

> 💡 \_\_proto\_\_ 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아닌 Object.prototype의 프로퍼티

```js
const person = { name: 'Kim' };

// person 객체는 __proto 프로퍼티를 소유하지 않음
console.log(person.hasOwnProperty('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있음
console.log({}.__proto__ === Object.prototype); // true
```

**3. \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

> 💡 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함

```js
const parent = {};
const child = {};

// child 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

- 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 함

**4. \_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장되지 않음**

> 💡 Object.setPrototypeOf 메서드와 Object.getPrototypeOf 메서드를 사용 권장

```js
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

## 함수 객체의 prototype 프로퍼티

> 💡 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴

```js
// 함수 객체는 prototype 프로퍼티를 소유
(function () {}.hasOwnProperty('prototype')); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않음
({}.hasOwnProperty('prototype')); // false
```

- 모든 객체가 갖는 \_\_proto\_\_ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킴

| 구분                          | 소유        | 값                | 사용 주체   | 사용목적                                                               |
| ----------------------------- | ----------- | ----------------- | ----------- | ---------------------------------------------------------------------- |
| \_\_proto\_\_ 접근자 프로퍼티 | 모든 객체   | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용                |
| prototype 프로퍼티            | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 인스턴스의 프로토타입을 할당하기 위해 사용 |

```js
// 생성자 함수
function Obj(a) {
	this.a = a;
}

const b = new Obj('asdf');

console.log(Obj.prototype === b.__proto__); // true
```

## 프로토타입의 constructor 프로퍼티와 생성자 함수

> 💡 모든 프로토타입은 constructor 프로퍼티를 가짐 => prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴

```js
function Person(name) {
	this.name = name;
}
const me = new Person('Lee');
// me 객체의 생성자 함수는 Person
console.log(me.constructor === Person); // true
```

- Person 생성자 함수는 me 객체를 생성
- me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

> 💡 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| ------------------ | ----------- | ------------------ |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

## 19.5 프로토타입 생성 시점

> 💡 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성됨

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

> 💡 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨

```js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성
console.log(Person.prototype);

// 생성자 함수
function Person(name) {
	this.name = name;
}
```

### 빌트인 생성자 함수와 프로토타입 생성 시점

> 💡 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성됨

`빌트인 생성자 함수`

- Object, String, Number, Function, Array, RegExp, Date, Promise

## 19.6 객체 생성 방식과 프로토타입의 결정

**객체 생성 방법**

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

### 객체 리터럴에 의해 생성된 객체의 프로토타입

> 💡 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype 임

- 객체 리터럴을 평가하여 객체를 생성할 때 추상연산 **OrdinaryObjectCreate**를 호출
- OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype 임

```js
const obj = { x: 1 };

console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

### Object 생성자 함수에 의해 생성된 객체의 프로토타입

> 💡 Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출됨

```js
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받음
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

### 생성자 함수에 의해 생성된 객체의 프로토타입

> 💡 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체

```js
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`Hi! ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');
```

## 19.7 프로토타입 체인

> 💡 객체의 포로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 \[[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색

```js
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`Hi! ${this.name}`);
};

const me = new Person('Lee');

me.hasOwnProperty(); /// true
```

**me.hasOwnProperty('name')** 을 호출하면

1. hasOwnProperty 메서드를 호출한 me 객체에서 hasOwnProperty 메서드를 검색
2. me 객체에는 해당 메서드가 없으므로 프로토타입 체인에 따라, \[[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입으로 이동하여 해당 메서드를 검색
3. Person.prototype에도 해당 메서드가 없으므로 프로토타입 체인에 따라 검색
4. Object.prototype에는 hasOwnProperty 메서드가 존재
5. Object.prototype.hasOwnProperty 메서드를 호출
   - 이때 Object.prototype.hasOwnProperty 메서드의 this에는 me 객체가 바인딩됨

> Object.prototype을 프로토타입 체인의 종점 (end of prototype chain)이라 함

`프로토타입 체인`

- 상속과 프로퍼티 검색을 위한 매커니즘

`스코프 체인`

- 식별자 검색을 위한 매커니즘

```js
me.hasOwnProperty('name');
```

- 먼저 `스코프 체인`을 통해 me 식별자를 검색
- me 식별자를 검색 후
- `프로토타입 체인`에서 hasOwnProperty 메서드를 검색

> 💡 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데에 사용됨

## 19.8 오버라이딩과 프로퍼티 섀도잉

> 💡 상속 관계에 의해 프로퍼티가 가려지는 현상

- 하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능
  - 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 엑세스는 허용되지 않음
- 프로토타입 프로퍼티를 변경 또는 삭제하려면 프로토타입에 직접 접근해야 함

```js
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
	console.log(`Hey! My name is ${this.name}`);
};
me.sayHello();

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

`오버라이딩`

- 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식

`오버로딩`

- 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식

## 19.9 프로토타입의 교체

> 💡 프로토타입은 생성자 함수 또는 인스턴스의 의해 교체될 수 있음

### 생성자 함수에 의한 프로토타입의 교체

> 💡 생성자 함수의 prototype 프로퍼티에 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체함

```js
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
	Person.prototype = {
		sayHello() {
			console.log(`${this.name}`);
		},
	};

	return Person;
})();
```

- Person.prototype에 객체 리터럴을 할당
  - Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것
- 프로토타입으로 교체된 객체 리터럴에는 constructor 프로퍼티가 없음

```js
console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

- 이처럼 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴됨

```js
const Person = (function () {
	function Person(name) {
		this.name = name;
	}

	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
	Person.prototype = {
		// constructor 프로퍼티와 생성자 함수 간의 연결을 설정
		constructor: Person,
		sayHello() {
			console.log(`${this.name}`);
		},
	};

	return Person;
})();

const me = new Person('Lee');
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

### 인스턴스에 의한 프로토타입의 교체

> 💡 \_\_proto\_\_ 접근자 프로퍼티를 통해 프로터타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것

```js
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

// 프로토타입으로 교체할 객체
const parent = {
	sayHello() {
		`${this.name}s`;
	},
};
// me 객체의 프로토타입을 parent 객체로 교체
Object.setPrototypeOf(me, parent);

me.sayHello();

console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

## 19.10 instanceof 연산자

> 💡 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가, 그렇지 않은 경우 false로 평가

```js
// 생성자 함수
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입의 교체
Object.setPrototype(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않음
console.log(Person.prototype === parent); // false
console.log(parent.constructor === Person); // false

// parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩
Person.prototype = parent;

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가
console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```

- 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인

## 19.11 직접 상속

### Object.create에 의한 상속

> 💡 Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성

```js
/**
 * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 반환
 * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
 * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 개체
 * @returns {Object} prototype 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
 */
 Object.create(prototype[, propertiesObject])
```

```js
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못함
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj = {}; 와 동일
obj = Object.create(Object.prototype);
console.log(Object.getPrototype(obj) === Object.prototype); // true

// obj = {x : 1}; 와 동일
obj = Object.create(Object.prototype, {
	x: { value: 1, writable: true, enumerable: true, configurable: true },
});
console.log(Object.getPrototype(obj) === Object.prototype); // true

// 생성자 함수
function Person(name) {
	this.name = name;
}

// obj = new Perosn('Lee'); 와 동일
obj = Object.create(Person.prototype);
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

**장점**

- new 연산자 없이도 객체를 생성 가능
- 프로토타입을 지정하면서 객체를 생성할 수 있음
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있음

### 객체 리터럴 내부에서 \_\_proto\_\_에 의한 직접 상속

```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있음
const obj = {
	y: 20,
	// 객체를 직접 상속
	__proto__: myProto,
};
console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 19.12 정적 프로퍼티/메서드

> 💡 정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말함

```js
// 생성자 함수
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function () {
	console.log('staticMethod');
};

const me = new Person('Lee');

// 정적 프로퍼티/메서드 참조/호출
Person.staticMethod();

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 호출할 수 없음
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 함
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

## 19.13 프로퍼티 존재 확인

### in 연산자

> 💡 in 연산자는 객체 내의 특정 프로퍼티가 존재하는지 여부를 확인

```js
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 */
key in object;
```

- in 연산자는 확인 대상 객체의 프로퍼티 뿐 아니라 확인 대상 객체가 상속받은 모든 프로토파입의 프로퍼티를 확인하므로 주의해야 함

```js
const person = { name: 'Kim', address: 'Seoul' };

console.log('name' in person); // true
console.log('age' in person); // false
console.log('toString' in person); // true
```

### Object.prototype.hasOwnProperty 메서드

> 💡 Object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는 확인 가능

- 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환

```js
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('toString')); // false
```

## 19.14 프로퍼티 열거

### for ... in 문

> 💡 객체의 모든 프로퍼티를 순회하면 열거하려면 for ... in 문을 사용

```js
const person = {
	name: 'Kim',
	address: 'Seoul',
};

// for ... in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당
for (const key in person) {
	console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

- for ... in 문은 객체의 프로퍼티 개수만큼 순회하며 for ... in 문의 변수 선언문에서 선언한 변수에 프로퍼티 키를 할당
- for ... in 문은 in 연산자처럼 순회 대상의 프로퍼티 뿐 아니라 상속받은 프로토타입의 프로퍼티까지 열거
- 프로퍼티를 열거할 때 순서를 보장하지 않음
  - 하지만 대부분의 브라우저는 순서를 보장하고 숫자(사실은 문자열)인 프로퍼티 키에 대해 정렬을 실시

```js
const person = {
	name: 'Kim',
	address: 'Seoul',
};

// in 연산자는 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인
console.log('toString' in person); // true

// for ... in 문도 상속받은 모든 프로토타입의 프로퍼티를 열거
// 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않음
for (const key in person) {
	console.log(key + ': ' + person[key]);
}

// name: Lee
// address: Seoul
```

- toString 메서드가 열거할 수 없도록 정의되어 있는 프로퍼티이기 때문
  - \[[Enumerable]]의 값이 false이기 때문

> 👉 for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 \[[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거

### Object.keys/values/entries 메서드

> 💡 객체 자신의 고유 프로퍼티만을 열거

```js
const person = {
	name: 'Lee',
	address: 'Seoul',
	__proto__: { age: 20 },
};

console.log(Object.keys(person)); // ["name", "address"]
console.log(Object.values(person)); // ["Lee", "Seoul"]
console.log(Object.entries(person)); // [["name", "Lee"], ["address", ""Seoul"]]
```
