# 25. 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

> 💡 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 새로운 객체 생성 매커니즘으로 보는 것이 합당

**클래스와 생성자 함수의 차이점**

1. <span style="color: blue">클래스</span>는 new 연산자 없이 호출하면 에러가 발생하지만, <span style="color: red">생성자 함수</span>는 new 연산자 없이 호출하면 일반 함수로서 호출됨
2. <span style="color: blue">클래스</span>는 상속을 지원하는 extends와 super 키워드를 제공하지만, <span style="color: red">생성자 함수</span>는 extends와 super 키워드를 지원하지 않음
3. <span style="color: blue">클래스</span>는 호이스팅이 발생하지 않는 것처럼 동작하지만 <span style="color: red">생성자 함수</span>는 호이스팅이 발생
4. <span style="color: blue">클래스</span> 내의 모든 코드에는 암묵적으로 strict mode가 지정되어 실행되며 해제하 수 없고, <span style="color: red">생성자 함수</span>는 그렇지 않음
5. <span style="color: blue">클래스</span>의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 \[[Enumerable]]의 값은 false이기 때문에 열거되지 않음

## 25.2 클래스 정의

> 💡 class 키워드를 사용하여 정의

- 파스칼 케이스를 사용하는 것이 일반적

```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스는 일급 객체로서 다음과 같은 특징을 가짐

- 무명의 리터럴로 생성할 수 있으므로, 런타임에 생성이 가능
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있음
- 함수의 매개변수에게 전달할 수 있음
- 함수의 반환값으로 사용 가능

```js
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name; // name 프로퍼티는 public
	}

	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi! My name is ${this.name}`);
	}

	// 정적 메서드
	static sayHello() {
		console.log(`Hello`);
	}
}

// 인스턴스 생성
const me = new Person('Kim');

// 인스턴스 프로퍼티 참조
console.log(me.name); // Kim

// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Kim

// 정적 메서드 호출
Person.sayHello(); // Hello
```

## 25.3 클래스 호이스팅

> 💡 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성

- 단, 클래스는 클래스 정의 이전에 참조할 수 없음
  - 변수 선언, 함수 정의와 마찬가지로 호이스팅은 발생함
  - 단, let, const 키워드로 선언한 변수처럼 호이스팅됨
  - TDZ에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작

```js
console.log(Person); // ReferenceError: Cannot access 'Person' before initialization

class Person {}

console.log(typeof Person); // function
```

## 25.4 인스턴스 생성

> 💡 클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스를 생성

```js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

## 25.5 메서드

> 💡 클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드가 있음

### constructor

> 💡 인스턴스를 생성하고 초기화하기 위한 특수한 메서드

```js
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}
}

console.dir(Person);
```

![image](https://user-images.githubusercontent.com/55246584/152629692-a043d716-3323-4754-af80-2c7ce6c5beff.png)

**클래스는 평가되어 함수 객체가 됨**

- 모든 함수 객체가 가지는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킴
  - 이는 클래스가 인스턴스를 생성하는 **생성자 함수**라는 것을 의미
  - new 연산자와 함께 클래스를 호출하면 클래스는 **인스턴스를 생성**

**constructor**

- 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 됨
  - 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성
- 최대 한 개만 존재 가능 (생략 가능)

```js
class Person {
	constructor() {
		// 고정값으로 인스턴스 초기화
		this.name = 'Kim';
		this.address = 'Seoul';
	}
}

// 인스턴스 프로퍼티가 추가됨
const me = new Person();
console.log(me); // Person {name: "kim", address: "Seoul"}
```

```js
class Person {
	constructor(name, address) {
		// 인수로 인스턴스 초기화
		this.name = name;
		this.address = address;
	}
}

// 인수로 초기값을 전달
const me = new Person('Kim', 'Seoul');
console.log(me); // Person {name: "kim", address: "Seoul"}
```

### 프로토타입 메서드

> 💡 클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입 메서드가 됨

```js
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}

	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi! My name is ${this.name}`);
	}
}

const me = new Person('Kim');
me.sayHi(); // Hi! My name is Kim

// me 객체의 프로토타입은 Person.prototype 임
Object.getPrototypeOf(me) === Person.prototype; // true
me instanceof Person; // true

// Person.prototype의 프로토타입은 Object.prototype 임
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
me instanceof Object; // true

// me 객체의 constructor는 Person 클래스
me.constructor === Person; // true
```

- 이처럼 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 됨
  - 인스턴스는 프로토타입 메서드를 상속받아 사용 가능
- 생성자 함수의 역할을 클래스가 함
- 클래스는 생성자 함수와 같이 인스턴스를 생성하는 매커니즘

👉 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 매커니즘

### 정적 메서드

> 💡 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

```js
class Person {
	// 생성자
	constructor(name) {
		this.name = name;
	}
	// 정적 메서드
	static sayHi() {
		console.log('Hi!');
	}
}

Person.sayHi(); // Hi!

// 인스턴스 생성
const me = new Person('Kim');
me.sayHi(); // TypeError
// 정적 메서드는 인스턴스로 호출할 수 없음
// 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문
```

### 정적 메서드와 프로토타입 메서드의 차이

- <span style="color:blue">정적 메서드</span>와 <span style="color:green">프로토타입 메서드</span>는 자신이 속해 있는 프로토타입 체인이 다름
- <span style="color:blue">정적 메서드</span>는 클래스로 호출하고 <span style="color:green">프로토타입 메서드</span>는 인스턴스로 호출
- <span style="color:blue">정적 메서드</span>는 인스턴스 프로퍼티를 참조할 수 없지만 <span style="color:green">프로토타입 메서드</span>는 인스턴스 프로퍼티를 참조할 수 있음

### 클래스에서 정의한 메서드의 특징

- function 키워드를 생략한 메서드 축약 표현을 사용
- 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없음
- 암묵적으로 strict mode가 실행됨
- for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없음
- 내부 메서드 \[[Construct]]를 갖지 않는 non-constructor

## 25.6 클래스의 인스턴스 생성 과정

**1. 인스턴스 생성과 this 바인딩**

- new 연산자와 함께 클래스를 호출하면 constructor의 내부 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성
  - 클래스가 생성할 인스턴스가 됨
  - 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정
- 암묵적으로 생성된 빅 객체는 this에 바인딩됨
- 따라서, constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킴

**2. 인스턴스 초기화**

- this에 바인딩되어 있는 인스턴스를 초기화
- this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화

**3. 인스턴스 반환**

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 암묵적으로 반환

```js
class Person {
	// 생성자
	constructor(name) {
		// 암묵적으로 인스턴스가 생성되고 this에 바인딩
		console.log(this); // Person {}
		console.log(Object.getPrototypeOf(this) === Person.prototype); // true

		// this에 바인딩되어 있는 인스턴스를 초기화
		this.name = name;

		// 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
	}
}
```

## 25.7 프로퍼티

### 인스턴스 프로퍼티

### 접근자 프로퍼티

### 클래스 필드 정의 제안

### private 필드 정의 제안

### static 필드 정의 제안

## 25.8 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

### extends 키워드

### 동적 상속

### 서브클래스의 constructor

### super

### 상속 클래스의 인스턴스 생성 과정

### 표준 빌트인 생성자 함수 확장
