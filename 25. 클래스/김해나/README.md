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

<br><br>

\*\* 이미지 출처 : https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-25%EC%9D%BC%EC%B0%A8-%ED%81%B4%EB%9E%98%EC%8A%A4class-1%ED%8E%B8-%EB%A9%94%EC%84%9C%EB%93%9C
