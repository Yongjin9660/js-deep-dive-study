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

## 19.9 프로토타입의 교체

## 19.10 instanceof 연산자

## 19.11 직접 상속

## 19.12 정적 프로퍼티/메서드

## 19.13 프로퍼티 존재 확인

## 19.14 프로퍼티 열거
