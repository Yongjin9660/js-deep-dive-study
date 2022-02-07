# 25. 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- 자바스크립트는 `프로토타입 기반 객체지향 언어`
- ES5 에서는 클래스 없이도 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속 구현 가능
- `클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 동일하게 동작 X`
  <br>

#### \*\* 클래스와 생성자 함수의 차이

1. `클래스를 new 연산자 없이 호출하면 에러가 발생한다.`
   ↔ 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
   <br>
2. `클래스는 상속을 지원하는 extends 와 super 키워드를 제공한다.`
   ↔ 생성자 함수는 extends 와 super 키워드를 지원하지 않는다.
   <br>
3. `클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.`
   ↔ 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
   <br>
4. `클래스 내의 모든 코드에는 암묵적으로 strict mode 가 지정되어 실행되며 strict mode 를 해제할 수 없다.`
   ↔ 생성자 함수는 암묵적으로 strict mode 가 지정되지 않는다.
   <br>
5. `클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 \[[Enumerable]] 의 값이 false, 즉 열거되지 않는다.`
   <br>

-> 따라서 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕이라고 보기보다는 **새로운 객체 생성 메커니즘**으로 보는 것이 합당함.
<br>

## 25.2 클래스 정의

```jsx
// 클래스 선언문
class Person {}
```

- 클래스는 `class 키워드를 사용하여 정의`하며, 클래스 이름은 주로 파스칼 케이스를 사용

<br>

```jsx
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {}:
```

- 함수와 마찬가지로 `표현식으로 클래스를 정의`할 수 있으며, 이때 클래스는 이름을 가질 수도 있고, 갖지 않을 수도 있음.
- 클래스는 `값으로 사용할 수 있는 일급 객체`이며, 다음과 같은 특징을 가짐.
  - 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  - 변수나 자료구조(객체, 배열 등) 에 저장할 수 있다.
  - 함수의 매개변수에게 전달할 수 있다.
  - 함수의 반환값으로 사용할 수 있다.
    <br>

## 25.3 클래스 호이스팅

> 💡 `클래스는 함수로 평가됨.`

<br>

```jsx
// 클래스 선언문
class Person {}
console.log(typeof Person); // function
```

```jsx
// 클래스 정의 이전에 참조 X
console.log(Person); // ReferenceError

// 클래스 선언문
class Person {}
```

- `클래스 선언문`으로 정의한 클래스는 함수 선언문과 같이 **런타임 이전에 먼저 평가되어 함수 객체를 생성함.**
- 이때 클래스가 평가되어 생성된 함수 객체는 **생성자 함수로서 호출할 수 있는 함수, 즉 constructor**
- 함수 객체를 생성하는 시점에 **프로토타입도 더불어 생성됨.**
- `단, 클래스는 클래스 정의 이전에 참조 X`
  <br>

```jsx
const Person = "";

{
	// 호이스팅이 발생하지 않는다면 '' 이 출력되어야 한다.
	console.log(Person);
	// ReferenceError

	// 클래스 선언문
	class Person {}
}
```

- 클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이지만 그렇지 않다.
- `클래스 선언문도 변수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다.`
  - 단, 클래스는 `let, const 키워드로 선언한 변수처럼 호이스팅된다.`
  - 클래스 선언문 이전에 일시적 사각지대에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.
    <br>

## 25.4 인스턴스 생성

> `클래스는 생성자 함수이며 new 연산자와 함께 호출되어 인스턴스 생성`

<br>

```jsx
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

```jsx
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass 는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자
const you = new MyClass(); // ReferenceError
console.log(MyClass); // ReferenceError
```

- <em> 클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 `반드시 new 연산자와 함께 호출`해야 함.</em>
- `클래스 표현식으로 정의된 클래스의 경우 클래스를 가리키는 식별자로 인스턴스를 생성해야 함.`
  - 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 X
    <br>

## 25.5 메서드

> `클래스 몸체에는 0개 이상의 메서드만 선언 가능`
> → 클래스 몸체에서 정의할 수 있는 메서드는 `constructor (생성자), 프로토타입 메서드, 정적 메서드`

<br>

##### 1) constructor

> 💡 인스턴스를 생성하고 초기화하기 위한 특수한 메서드로, 이름을 변경할 수 없음.

<br>

```jsx
// 생성자 함수
function Person() {
	// 인스턴스 생성 및 초기화
	this.name = name;
}
```

```jsx
// 클래스
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}
}
```

- 클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있음.
- 함수와 동일하게 프로토타입과도 연결되어 있으며 자신의 스코프 체인을 구성함.
- constructor 내부의 this 는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킴.
  <br>

```jsx
// SyntaxError
class Person {
	constructor() {}
	constructor() {}
}
```

```jsx
class Person {
	// constructor 생략 시 빈 constructor 가 암묵적으로 정의된다.
	constructor() {}
}

// 빈 객체가 생성된다.
const me = new Person();
console.log(me); // Person {}
```

- constructor 는 클래스 내에 최대 1개만 존재 가능하며, 생략도 가능함.
  - constructor 를 생략하면 빈 constructor 가 암묵적으로 정의되어 빈 객체가 생성됨.
    <br>

```jsx
class Person {
	constructor() {
		// 고정값으로 인스턴스 초기화
		this.name = "Lee";
		this.address = "Seoul";
	}
}

// 인스턴스 프로퍼티가 추가된다.
const me = new Person();
console.log(me); // Person { name : 'Lee', address : 'Seoul' }
```

```jsx
class Person {
	constructor(name, address) {
		// 인수로 인스턴스 초기화
		this.name = name;
		this.address = address;
	}
}

// 인수로 초기값을 전달한다. 초기값은 constructor 에 전달된다.
const me = new Person("Lee", "Seoul");
console.log(me); // Person { name : 'Lee', address : 'Seoul' }
```

- 인스턴스를 초기화하려면 constructor 생략 X
  <br>

```jsx
class Person {
	constructor(name) {
		this.name = name;

		// 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
		return {};
	}
}

// constructor 에서 명시적으로 반환한 빈 객체가 반환된다.
const me = new Person("Lee");
console.log(me); // {}
```

```jsx
class Person {
	constructor(name) {
		this.name = name;

		// 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this 가 반환된다.
		return 100;
	}
}

// constructor 에서 명시적으로 반환한 빈 객체가 반환된다.
const me = new Person("Lee");
console.log(me); // Person { name : 'Lee' }
```

- `new 연산자와 함께 클래스가 호출되면 암묵적으로 this, 즉 인스턴스를 반환함.`

  - this 가 아닌 다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환됨.
  - 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this 가 반환됨.

- `constructor 내부에서는 return 문 반드시 생략할 것`
  <br>

##### 2) 프로토타입 메서드

> 💡 클래스 몸체에서 정의한 메서드는 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨.

<br>

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.hello = function () {
	console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.hello(); // Hi! My name is Lee
```

```jsx
// 클래스
class Person {
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}

	// 프로토타입 메서드
	hello() {
		console.log(`Hi! My name is ${this.name}`);
	}
}

const me = new Person("Lee");
me.hello(); // Hi! My name is Lee
```

<br>

![img](https://media.vlpt.us/images/chappi/post/1e7d8a9f-328a-424f-9498-aa6f35e44f55/2.png)
<br>

- 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 됨.
- `클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 됨.`
  - 인스턴스는 프로토타입 메서드를 상속받아 사용 가능
    <br>

##### 3) 정적 메서드

> 💡 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

<br>

```jsx
// 생성자 함수
function Person(name) {
	this.name = name;
}

// 정적 메서드
Person.hello = function () {
	console.log(`Hi! My name is ${this.name}`);
};

// 정적 메서드 호출
Person.hello(); // Hi! My name is Lee
```

```jsx
// 클래스
class Person {
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}

	// 정적 메서드
	static hello() {
		console.log(`Hi! My name is ${this.name}`);
	}
}
```

<br>

![img](https://media.vlpt.us/images/chappi/post/a19214ac-4c1c-4bc7-9832-50cdb5eb7e4b/3.png)
<br>

- 클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드가 됨.
- `정적 메서드는 클래스에 바인딩되는 메서드이기 때문에 인스턴스로 호출하지 않고 클래스로 호출함.`
  - 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않기 때문에 인스턴스로 호출 X
    <br>

#### \*\* <em> 정적 메서드와 프로토타입 메서드의 차이</em>

- 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 **프로토타입 체인이 다르다.**
- **정적 메서드는 클래스로 호출하고, 프로토타입 메서드는 인스턴스로 호출한다.**
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.
  <br>

#### \*\* <em>클래스에서 정의한 메서드의 특징</em>

- function 키워드를 생략한 메서드 축약 표현을 사용한다.
- 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
- 암묵적으로 strict mode 로 실행된다.
- for... in 문이나 Object.keys 메서드 등으로 열겨할 수 없다. 즉, 프로퍼티의 열거 가능 여부를 나타내며, 불리언 값을 갖는 프로퍼티 어트리뷰트 \[[Enumerable]] 의 값이 false 이다.
- 내부 메서드 \[[Construct]] 를 갖지 않는 non-constructor 다. 따라서 new 연산자와 함께 호출할 수 없다.
  <br>

## 25.6 클래스의 인스턴스 생성 과정

> new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스 내부 메서드 \[[Construct]] 가 호출됨.  
> → 클래스는 new 연산자 없이 호출 X

<br>

```jsx
class Person {
	// 생성자
	constructor(name) {
		// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩된다.
		console.log(this); // Person {}
		console.log(Object.getPrototypeOf(this) === Person.prototype); // true

		// 2. this 에 바인딩되어 있는 인스턴스를 초기화한다.
		this.name = name;

		// 3. 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환된다.
	}
}
```

1. **인스턴스 생성과 this 바인딩**
   - `new 연산자와 함께 클래스를 호출하면 constructor 내부 코드가 실행되기 전에 암묵적으로 빈 객체가 생성됨.`
   - `빈 객체는 이후에 클래스가 생성할 인스턴스를 가리키며, 이 인스턴스는 this 에 바인딩됨.`
   - constructor 내부의 this 는 클래스가 생성한 인스턴스를 가리킴.
     <br>
2. **인스턴스 초기화**
   - `constructor 내부 코드가 실행되어 this 에 바인딩되어 있는 인스턴스를 초기화함.`
   - this 에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고, constructor 가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티값을 초기화함.
   - 만약 constructor 가 생략되었다면 이 과정도 생략됨.
     <br>
3. **인스턴스 반환**
   - 클래스의 모든 처리가 끝나면 `완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환됨.`
     <br>

## 25.7 프로퍼티

1. **인스턴스 프로퍼티**
   - 인스턴스 프로퍼티는 `constructor 내부에서 정의`해야 함.

<br>

```jsx
class Person {
	constructor(name) {
		// 인스턴스 프로퍼티
		this.name = name;
	}
}

const me = new Person("Lee");
console.log(me); // Person { name : 'Lee' }

// name 프로퍼티는 public
console.log(me.name); // Lee
```

- `constructor 내부에서 this 에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티`
- ES6 의 클래스는 접근 제한자를 지원하지 않기 때문에 `인스턴스 프로퍼티는 언제나 public`
  - private 한 프로퍼티를 정의할 수 있는 사양이 현재 제안 중에 있음. => <em>private 필드 정의 제안</em>
    <br>

2. **접근자 프로퍼티**
   - 접근자 프로퍼티는 자체적으로 값 (\[[Value]] 내부 슬롯) 을 갖지 않고 `다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티`

<br>

```jsx
class Person {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	// fullName 은 접근자 함수로 구성된 접근자 프로퍼티
	// getter 함수
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	}

	// setter 함수
	set fullName(name) {
		// 배열 디스트럭처링 할당
		[this.firstName, this.lastName] = name.split(" ");
	}
}

const me = new Person("Ungmo", "Lee");

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(`${me.firstName} ${me.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName 에 값을 저장하면 setter 함수가 호출된다.
me.fullName = "Heegun Lee";
console.log(me); // { firstName : 'Heegun', lastName : 'Lee' }

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName 에 접근하면 getter 함수가 호출된다.
console.log(me.fullName); // Heegun Lee

// fullName 은 접근자 프로퍼티
// 접근자 프로피는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, "fullName"));
// { get : f, set : f, enumerable : false, configurable : true }
```

<br>

- **접근자 프로퍼티는 getter 함수와 setter 함수로 구성됨.**

  - `getter 함수` : 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용
    - getter 함수는 메서드 이름 앞에 `get 키워드 사용`
    - 무언가를 취득할 때 사용하므로 `반환값 필수`
      <br>
  - `setter 함수` : 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용 - 메서드 이름 앞에 `set 키워드 사용` - 무언가를 프로퍼티에 할당해야 할 때 사용하므로 `매개변수 필수` - setter 는 단 하나의 매개변수만 선언 가능
    <br>

- **클래스의 메서드는 기본적으로 프로토타입 메서드**
  - `클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티`
    <br>

#### \*\* **클래스 필드 정의 제안**

- 클래스 몸체에서 클래스 필드를 정의할 수 있는 `클래스 필드 정의 제안은 아직 ECMAScript 의 정식 표준 사양 X` - 자바스크립트의 클래스 몸체에는 메서드만 선언 가능하므로 클래스 필드를 내부에 선언하면 에러 발생
  <br>

```jsx
// 최신 브라우저 및 최신 Node.js
class Person {
	// 클래스 필드 정의
	name = "Lee";
}

const me = new Person();
console.log(me); // Person { name : 'Lee' }
```

- `최신 브라우저 및 최신 Node.js 에서는 선제적으로 미리 구현해놓았기 때문에 클래스 몸체 내부에 클래스 필드 정의 가능`
  <br>

```jsx
// 1) 클래스 몸체에서 클래스 필드 정의 시 this 에 클래스 필드 바인딩 X
// → this 는 클래스의 constructor 와 메서드 내에서만 유효하기 때문

class Person {
	// this 에 클래스 필드 바인딩
	this.name = '';		// SyntaxError
}
```

```jsx
// 2) 클래스 필드 참조 시 반드시 this 사용

class Person {
	// 클래스 필드
	name = "Lee";

	constructor() {
		console.log(name); // ReferenceError
	}
}

new Person();
```

```jsx
// 3) 클래스 필드에 초기값을 할당하지 않으면 undefined

class Person {
	name;
}

const me = new Person();
console.log(me); // undefined
```

```jsx
// 4) 인스턴스를 생성할 때 외부 초기값으로 클래스 필드를 초기화해야 한다면
// constructor 에서 클래스 필드를 초기화해야 한다.

class Person {
	name;
	constructor(name) {
		// 클래스 필드 초기화
		this.name = name;
	}
}

const me = new Person("Lee");
console.log(me); // Lee

// -----------------------------------------------------------------------

// 인스턴스 생성할 때 클래스 필드를 초기화해야 한다면 constructor 밖에서 클래스 필드 정의할 필요 X
// 클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티가 없다면 자동 추가되기 때문

class Person {
	constructor(name) {
		this.name = name;
	}
}

const me = new Person("Lee");
console.log(me); // Person { name : 'Lee' }
```

<br>

💡 **`모든 클래스 필드는 인스턴스 프로퍼티`**

- 함수는 일급 객체이므로 클래스 필드에 할당 가능
  - But, 이때 생성된 함수도 인스턴스 메서드가 되기 때문에 권장 X
    <br>

💡 **`인스턴스 프로퍼티 정의하는 방식`**

1. `인스턴스 생성 시 외부 초기값으로 클래스 필드 초기화해야 할 때`

   - constructor 에서 인스턴스 프로퍼티 정의하는 기존의 방식
     <br>

2. ` 인스턴스 생성 시 외부 초기값이 필요 없을 때`

   - constructor 에서 인스턴스 프로퍼티 정의하는 기존의 방식
   - 클래스 필드 정의 제안
     <br>

#### \*\* **private 필드 정의 제안**

- 자바스크립트는 private, public, protected 와 같은 접근 제한자 지원 X
- `인스턴스 프로퍼티 및 클래스 필드 모두 기본적으로 public`
- private 필드를 정의할 수 있는 새로운 표준 사양이 제안됨.
  => <em> **private 필드 정의 제안**</em>

<br>

```jsx
class Person {
	// private 필드 정의
	#name = "";

	constructor(name) {
		this.#name = name;
	}
}

const me = new Person("Lee");

// private 필드 #name 은 클래스 외부에서 참조 X
console.log(me.#name); // SyntaxError
```

```jsx
class Person {
	constructor(name) {
		// private 필드는 반드시 클래스 몸체에서 정의
		this.#name = name; // SyntaxError
	}
}
```

- `private 필드 선두에 # 기호를 붙여주며 private 필드를 참조할 때도 # 을 붙여줘야 함.`
- `private 필드는 반드시 클래스 몸체에 정의해야 함.`
  - constructor 내부에 private 필드를 정의하게 되면 에러 발생
    <br>

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | X       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

- `private 필드는 클래스 내부에서만 참조 가능`
- 클래스 외부에서 private 필드에 직접 접근할 수 있는 방법 X
  - `접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 가능`
    <br>

\*\* **접근자 프로퍼티를 통해 private 필드에 접근하는 방법**

```jsx
class Person {
	// private 필드 정의
	#name = "";

	constructor(name) {
		this.#name = name;
	}

	// name 은 접근자 프로퍼티
	get name() {
		// private 필드를 참조하여 trim 한 다음 반환
		return this.#name.trim();
	}
}

const me = new Person(" Lee ");
console.log(me.name); // Lee
```

<br>

#### \*\* **static 필드 정의 제안**

- `클래스에서는 static 키워드를 사용하여 정적 메서드는 정의할 수 있었지만 정적 필드는 정의할 수 없었음.`
- But, 최신 브라우저 및 최신 Node.js 에는 static public/private 필드가 구현되어 있음.
  <br>

```jsx
class MyMath {
	// static public 필드 정의
	static PI = 22 / 7;

	// static private 필드 정의
	static #num = 10;

	// static 메서드
	static increment() {
		return ++MyMath.#num;
	}
}

console.log(MyMath.PI); // 3.1428571...
console.log(MyMath.increment()); // 11
```

<br>

## 25.8 상속에 의한 클래스 확장

#### 1) 클래스 상속과 생성자 함수 상속

- **상속에 의한 클래스 확장**은 `기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것`
  <br>
- **프로토타입 기반 상속**은 `프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념`
  <br>

![img](https://media.vlpt.us/images/chappi/post/e9598797-a970-4fac-a5eb-f429cf0b867d/1.png)
<br>

```jsx
class Animal {
	constructor(age, weight) {
		this.age = age;
		this.weight = weight;
	}

	eat() {
		return "eat";
	}

	move() {
		return "move";
	}
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
	fly() {
		return "fly";
	}
}

const bird = new Bird(1, 5);

console.log(bird); // Bird { age : 1, weight : 5 }
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat()); // eat
console.log(bird.move()); // move
console.log(bird.fly()); // fly
```

- 상속에 의해 확장된 클래스 Bird 를 통해 생성된 인스턴스의 프로토타입 체인은 다음과 같다.
  <br>

![img](https://media.vlpt.us/images/chappi/post/21181983-b538-4a55-ab7a-e76929d05dde/3.png)
<br>

- 클래스는 상속을 통해 다른 클래스를 확장할 수 있는 문법인 `extends` 키워드가 기본적으로 제공됨.
- But, 생성자 함수는 클래스와 같이 상속을 통해 다른 생성자 함수를 확장할 수 있는 문법 제공 X
  <br>

#### 2) extends 키워드

> 💡 상속을 통해 클래스를 확장하기 위해서는 `extends 키워드를 사용하여 상속받을 클래스 정의`
> → `extends` 키워드 역할 : 수퍼 클래스와 서브 클래스 간의 `상속 관계를 설정`하는 역할

<br>

```jsx
// 수퍼 (베이스/부모) 클래스
class Base {}

// 서브 (파생/자식) 클래스
class Derived extends Base {}
```

- `수퍼 클래스 (베이스/부모 클래스)`
  - 서브 클래스에게 상속된 클래스
    <br>
- `서브 클래스 (파생/자식 클래스)`
  - 상속을 통해 확장된 클래스
    <br>

![img](https://media.vlpt.us/images/chappi/post/ecf743a9-c460-42d3-acd9-bceeebee3e18/4.png)
<br>

- 수퍼 클래스와 서브 클래스는 `인스턴스의 프로토타입 체인뿐만 아니라 클래스 간의 프로토타입 체인도 생성함.`
- `이를 통해 프로토타입 메서드, 정적 메서드 모두 상속 가능`
  <br>

#### 3) 동적 상속

> 💡 `extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스 확장하는 것도 가능`
> → 단, extends 키워드 앞에는 반드시 클래스가 와야 함.

<br>

```jsx
// 생성자 함수
function Base(a) {
	this.a = a;
}

// 생성자 함수를 상속받는 서브 클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived { a : 1 }
```

<br>

```jsx
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브 클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

- extends 키워드 다음에는 클래스뿐만 아니라 \[[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능
- `이를 통해 동적으로 상속받을 대상 결정 가능`
  <br>

#### 4) 서브클래스의 constructor

> 💡 클래스에서 constructor 생략 시 비어있는 constructor 가 암묵적으로 정의됨.

<br>

```jsx
constructor(...args) {
	super(...args);
}
```

- 서브 클래스에서 constructor 를 생략하면 위와 같은 constructor 가 암묵적으로 정의됨.
  - args 는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트
    <br>

##### \*\* `Rest 파라미터`

- 매개변수에 ...을 붙이면 Rest 파라미터가 되는데, `함수에 전달된 인수들의 목록을 배열로 전달하는 역할`
  <br>

```jsx
// 수퍼 클래스
class Base {}

// 서브 클래스
class Derived extends Base {}
```

- 위 예제의 클래스에는 아래와 같이 암묵적으로 constructor 가 정의됨.
  <br>

```jsx
// 수퍼 클래스
class Base {
	constructor() {}
}

// 서브 클래스
class Derived extends Base {
	constructor(...args) {
		super(...args);
	}
}

const derived = new Derived();
console.log(derived); // Derived {}
```

- `수퍼 클래스와 서브 클래스 모두 constructor 를 생략하면 빈 객체가 생성됨.`
- 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 함.
  <br>

#### 5) super 키워드

> 💡 super 키워드는 함수처럼 호출할 수도 있고, this 와 같이 식별자처럼 참조할 수 있는 특수한 키워드

<br>

**※ super 호출**

- super 를 호출하면 수퍼 클래스의 constructor 호출
  <br>

```jsx
// 수퍼 클래스
class Base {
	constructor(a, b) {
		this.a = a;
		this.b = b;
	}
}

// 서브 클래스
class Derived extends Base {
	// 다음과 같이 암묵적으로 constructor 가 정의된다.
	// constructor(...args) { super(...args);	}
}

const derived = new Derived(1, 2);
console.log(derived); // Derived { a : 1, b : 2 }
```

- `수퍼 클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브 클래스의 constructor 생략 가능`
- 이때 new 연산자와 함께 서브 클래스를 호출하면서 전달한 인수는 수퍼 클래스의 constructor 에 전달됨.
  - 서브 클래스에서 암묵적으로 정의된 constructor 의 super 를 호출했기 때문
    <br>

```jsx
// 수퍼 클래스
class Base {
	constructor(a, b) {
		this.a = a;
		this.b = b;
	}
}

// 서브 클래스
class Derived extends Base {
	constructor(a, b, c) {
		super(a, b);
		this.c = c;
	}
}

const derived = new Derived(1, 2, 3);
console.log(derived); // Derived { a : 1, b : 2, c : 3 }
```

- `수퍼 클래스에서 추가한 프로퍼티와 서브 클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브 클래스의 constructor 생략 X`
  <br>
- 이때 new 연산자와 함께 서브 클래스를 호출하면서 전달한 인수 중에서 수퍼 클래스의 constructor 에 전달할 필요가 있는 인수는 서브 클래스의 constructor 에서 호출하는 super 를 통해 전달
  <br>

##### \*\* **`super 호출 시 주의사항`**

- 서브 클래스에서 constructor 를 생략하지 않는 경우 서브 클래스의 constructor 에서는 반드시 super 를 호출해야 함.
- 서브 클래스의 constructor 에서 super 를 호출하기 전에는 this 참조 X
- super 는 반드시 서브 클래스의 constructor 에서만 호출한다. 서브 클래스가 아닌 클래스의 constructor 나 함수에서 super 를 호출하면 에러 발생
  <br>

**※ super 참조**

- 메서드 내에서 super 를 참조하면 수퍼 클래스의 메서드 호출 가능
  <br>

```jsx
// 수퍼 클래스
class Base {
	constructor(name) {
		this.name = name;
	}

	sayHi() {
		return `Hi! ${this.name}`;
	}
}

class Derived extends Base {
	sayHi() {
		// __super 는 Base.prototype 을 가리킨다.
		const __super = Object.getPrototypeOf(Derived.prototype);
		return `${__super.sayHi.call(this)} how are you doing?`;
	}
}
```

★★★ 이해안감,,

- call 메서드를 사용하여 this 를 전달하지 않고 Base.prototype.sayHi 를 그대로 호출하는 경우
  - Base.prototype.sayHi 메서드 내부의 this 는 Base.prototype 을 가리킴.
  - Base.prototype.sayHi 메서드는 프로토타입 메서드이기 때문에 내부의 this 는 Base.prototype 이 아닌 인스턴스를 가리켜야 함. => `call 메서드 사용`

<br>

```jsx
// ES6 의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]] 을 가진다.
// [[HomeObject]] 를 가진 함수만 super 참조 가능

const obj = {
	// foo : ES6 의 메서드 축약 표현으로 정의한 메서드 => [[HomeObject]] 소유
	foo() {},

	// bar : 일반 함수 => [[HomeObject]] 소유 X
	bar: function () {},
};
```

```jsx
const base = {
	name: "Lee",
	sayHi() {
		return `Hi! ${this.name}`;
	},
};

const derived = {
	__proto__: base,

	// ES6 메서드 축약 표현으로 정의한 메서드이므로 [[HomeObject]] 를 갖는다.
	sayHi() {
		return `${super.sayHi()}. how are you doing?`;
	},
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

- `ES6 의 메서드 축약 표현으로 정의된 함수만이 내부 슬롯 [[HomeObject]]` 를 가지며, 자신을 바인딩하고 있는 객체를 가리킴.
- `[[HomeObject]] 를 가지는 함수만이 super 참조 가능`

<br>

```jsx
// 수퍼 클래스
class Base {
	static sayHi() {
		return "Hi!";
	}
}

// 서브 클래스
class Derived extends Base {
	static sayHi() {
		// super.sayHi 는 수퍼 클래스의 정적 메서드를 가리킨다.
		return `${super.sayHi()} how are you doing?`;
	}
}

console.log(Derived.sayHi()); // Hi! how are you doing?
```

- 서브 클래스의 정적 메서드 내에서 super.sayHi 는 수퍼 클래스의 정적 메서드 sayHi 를 가리킴.
  <br>

#### 6) 상속 클래스의 인스턴스 생성 과정

<br>

\*\* **`Rectangle 클래스와 상속을 통해 Rectangle 클래스를 확장한 ColorRectangle 클래스 예제`**

```jsx
// 수퍼 클래스
class Rectangle {
	constructor(width, height) {
		// 인스턴스 초기화
		this.width = width;
		this.height = height;
	}

	getArea() {
		return this.width * this.height;
	}

	toString() {
		return `width = ${this.width}, height = ${this.height}`;
	}
}

// 서브 클래스
class ColorRectangle extends Rectangle {
	constructor(width, height, color) {
		super(width, height);
		this.color = color;
	}

	// 메서드 오버라이딩
	toString() {
		return super.toString() + `, color = ${this.color}`;
	}
}

const colorRectangle = new ColorRectangle(2, 4, "red");
console.log(colorRectangle); // ColorRectangle { width : 2, height : 4, color : 'red' }

// 상속을 통해 getArea 메서드 호출
console.log(colorRectangle.getArea()); // 8

// 오버라이딩된 toString 메서드 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```

<br>

\*\* **ColorRectangle 클래스에 의해 생성된 인스턴스의 프로토타입 체인**
<br>

<img src = 'https://media.vlpt.us/images/chappi/post/915bad00-960e-416a-9d83-013e138b177b/5.png'>

<br><br>

#### \*\* **`서브 클래스 ColorRectangle 이 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스 생성`**

##### 1) 서브 클래스의 super 호출

- 자바스크립트 엔진은 클래스를 평가할 때 `수퍼 클래스와 서브 클래스를 구분`하기 위해 내부 슬롯 `[[ConstructorKind]]` 를 가짐.

  - <em>'base'</em> : 다른 클래스를 상속받지 않는 클래스 (또는 생성자 함수) 의 내부 슬롯값
  - <em>'derived'</em> : 다른 클래스를 상속받는 서브 클래스의 내부 슬롯값
    <br>

- 서브 클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼 클래스에게 인스턴스 생성을 맡김.
  - 이것이 바로 `서브 클래스의 constructor 에서 반드시 super 호출`해야 하는 이유
    <br>

##### 2) 수퍼 클래스의 인스턴스 생성과 this 바인딩

```jsx
// 수퍼 클래스
class Rectangle {
	constructor(width, height) {

		// 암묵적으로 빈 객체 생성, 즉 인스턴스가 생성되고 this 에 바인딩된다.
		console.log(this);	// ColorReectangle {}

		// new 연산자와 함께 호출된 함수, 즉 new.target 은 ColorRectangle
		console.log(new.target);		// ColorRectangle

		// 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype 이 설정된다.
		console.log(Object.getPrototypeOf(this) ==== ColorRectangle.prototype);		// true
		console.log(this instanceof ColorRectangle); // true
	  console.log(this instanceof Rectangle);		// true
	}
}
```

- 수퍼 클래스의 constructor 내부 코드가 실행되기 이전에 암묵적으로 빈 객체 생성

  - 빈 객체, 즉 클래스가 생성한 인스턴스는 this 에 바인딩
  - 수퍼 클래스의 constructor 내부의 this 는 생성된 인스턴스를 가리킴.
    <br>

- 생성된 인스턴스의 프로토타입은
  - `수퍼 클래스의 prototype 프로퍼티가 가리키는 객체 (Rectangle.prototype)` (X)
  - `서브 클래스의 prototype 프로퍼티가 가리키는 객체 (ColorRectangle.prototype)` (O)
    <br>

##### 3) 수퍼 클래스의 인스턴스 초기화

- `수퍼 클래스의 constructor 가 실행되어 this 에 바인딩되어 있는 인스턴스 초기화`
  - this 에 바인딩되어 있는 인스턴스에 프로퍼티 추가
  - constructor 가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 초기화
    <br>

##### 4) 서브 클래스 constructor 로의 복귀와 this 바인딩

- super 호출이 종료되고 제어 흐름이 서브 클래스 constructor 로 복귀
  - 이때 super 가 반환한 인스턴스가 this 에 바인딩됨.
- `서브 클래스는 별도의 인스턴스를 생성하지 않고 super 가 반환한 인스턴스를 this 에 바인딩하여 그대로 사용`
  <br>

```jsx
// 서브 클래스
class ColorRectangle extends Rectangle {
	constructor(width, height, color) {
		super(width, height);

		// super 가 반환한 인스턴스가 this 에 바인딩된다.
		console.log(this); // ColorRectangle { width : 2, height : 4 }
	}
}
```

- `super 가 호출되지 않으면 인스턴스가 생성되지 않으며, this 바인딩도 할 수 없음.`
  - 이것이 바로 서브 클래스의 constructor 에서 super 를 호출하기 전에 this 참조할 수 없는 이유
    <br>

##### 5) 서브 클래스의 인스턴스 초기화

- super 호출 이후, 서브 클래스의 constructor 에 기술되어 있는 인스턴스 초기화 실행
  - this 에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고, constructor 가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 초기화
    <br>

##### 6) 인스턴스 반환

```jsx
// 서브 클래스
class ColorRectangle extends Rectangle {
	constructor(width, height, color) {
		super(width, height);

		// super 가 반환한 인스턴스가 this 에 바인딩된다.
		console.log(this); // ColorRectangle { width : 2, height : 4 }

		// 인스턴스 초기화
		this.color = color;

		// 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환된다.
		console.log(this); // ColorRectangle { width : 2, height : 4, color : 'red' }
	}
}
```

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환됨.
  <br>

#### 7) 표준 빌트인 생성자 함수 확장

> 💡 extends 키워드 다음에는 클래스뿐만 아니라 \[[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능
> → String, Number, Array 등과 같은 표준 빌트인 객체도 extends 키워드를 통해 확장 가능

<br>

```jsx
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
	// 중복된 배열 요소를 제거하고 반환 : [1, 1, 2, 3] -> [1, 2, 3]
	uniq() {
		return this.filter((v, i, self) => self.indexOf(v) === i);
	}

	// 모든 배열 요소의 평균 계산 : [1, 2, 3] -> 2
	average() {
		return this.reduce((pre, cur) => pre + cur, 0) / this.length;
	}
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]

// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```

★★★ 이해안감.......ㅎㅎ....

- Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스는 Array.prototype 과 MyArray.prototype 의 모든 메서드 사용 가능
  <br>

- `Array.prototype 메서드 중에서 map, filter 와 같이 새로운 배열을 반환하는 메서드는 MyArray 클래스의 인스턴스를 반환함.`
  - 만약 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환하지 않고, Array 의 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝 X
    <br>

\*\* **메서드 체이닝**
&ensp; : 메서드를 연이어 호출하는 것

```jsx
// 메서드 체이닝
// [1, 1, 2, 3] -> [1, 1, 3] -> [1, 3] -> 2
console.log(
	myArray
		.filter((v) => v % 2)
		.uniq()
		.average()
); // 2
```

- myArray.filter 가 반환하는 인스턴스와 uniq 메서드가 반환하는 인스턴스 모두 MyArray 타입
  - uniq 메서드가 반환하는 인스턴스로 average 메서드를 연이어 호출 (메서드 체이닝) 가능한 것

<br><br>
\*\* 이미지 출처 : https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-25%EC%9D%BC%EC%B0%A8-%ED%81%B4%EB%9E%98%EC%8A%A4class-1%ED%8E%B8-%EB%A9%94%EC%84%9C%EB%93%9C
https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-25%EC%9D%BC%EC%B0%A8-%ED%81%B4%EB%9E%98%EC%8A%A4class-2%ED%8E%B8-%ED%95%84%EB%93%9C-%EB%B3%80%EC%88%98%EC%99%80-%EC%83%81%EC%86%8D
