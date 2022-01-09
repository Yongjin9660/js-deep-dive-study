# 10. 객체 리터럴

## 10.1 객체란?

- 자바스크립트는 **객체기반의 프로그래밍 언어**
- 원시 값을 제외한 나머지 값이 모두 객체

**원시 타입**

- immutable value
- 변경 불가능한 값

**객체 타입**

- mutable value
- 변경 가능한 값

```js
var person = {
	// 객체 person 생성
	name: 'Lee', // 프로퍼티 -> 키 : name, 값 : 'Lee'
	age: 20, // 프로퍼티 -> 키 : age, 값 : 20
};
```

```js
var counter = {
	num: 0, // 프로퍼티
	increase: function () {
		// 메서드
		this.num++;
	},
};
```

- 함수도 프로퍼티의 값으로 사용될 수 있는데, 이때 일반 함수와 구분하기 위해 '메서드' 라고 부름.
- 객체는 프로퍼티와 메서드로 구성된 집합체

  `프로퍼티` : 객체의 상태를 나타내는 값 (data)
  `메서드` : 프로퍼티 (상태 데이터) 를 참조하고 조작할 수 있는 동작 (behavior)

> 💡 객체는 객체의 상태를 나타내는 값 (프로퍼티) 과 프로퍼티를 참조하고 조작할 수 있는 동작 (메서드) 을 모두 포함할 수 있기 때문에 상태와 동작을 하나의 단위로 구조화할 수 있어 유용

## 10.2 객체 리터럴에 의한 객체 생성

- **클래스 기반 객체지향 언어 (ex. C++, 자바)**

  - 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스를 생성하는 방식으로 객체 생성

- **프로토타입 기반 객체지향 언어 (ex. 자바스크립트)**

  - 클래스 기반 객체지향 언어와 달리 다양한 객체 생성 방법 지원

**객체 생성 방법**

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

1. **객체 리터럴**

```jsx
var person = {
	name: 'Lee',
	sayHello: function () {
		console.log(`Hello! My name is ${this.name}.`);
	},
};

console.log(typeof person); // object
console.log(person); // {name : 'Lee', sayHello : f}
```

- 객체를 생성하기 위한 표기법으로 가장 일반적인 방법
- 중괄호 { } 내에 0개 이상의 프로퍼티 정의
- 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해서 객체 생성

```js
var empty = {};
console.log(typeof empty); // object
```

- 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체 생성
- 객체 리터럴은 값으로 평가되는 표현식이기 때문에 닫는 중괄호 뒤에 세미콜론 필수

## 10.3 프로퍼티

> 💡 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성

```js
var person = {
	// 프로퍼티 키는 name, 프로퍼티 값은 'Kim'
	name: 'Kim',
	// 프로퍼티 키는 age, 프로퍼티 값은 25
	age: 25,
};
```

- **프로퍼티 키**

  - 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  - 암묵적 타입변환을 통해 문자열이 됨
  - 식별자 네이밍 규칙
    - 따른다면 따옴표 생략 가능
    - 따르지 않는다면 반드시 따옴표 사용

- **프로퍼티 값**
  - 자바스크립트에서 사용할 수 있는 모든 값

```js
var person = {
	firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
	'last-name': 'Lee', // 식별자 네이밍 규칙을 따르지 않는 프로퍼티 키
};

console.log(person); // {firstName : 'Ung-mo', last-name : 'Lee'}
```

```js
var foo = {
	'': '', // 빈 문자열도 프로퍼티 키로 사용 가능
};

console.log(foo); // {"" : ""}
```

```jsx
var obj = {}; // 빈 객체 생성
var key = 'hello';

// ES5 : 프로퍼티 키 동적 생성
obj[key] = 'world';

// ES6 : 계산된 프로퍼티 이름
// var obj = { [key] : 'world' };

console.log(obj); // {hello : "world"}
```

- 문자열 또는 문자열로 평가할 수 있는 표현식을 사용해서 <u> 동적으로 프로퍼티 키 생성 가능 </u>
- 이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호 [ ] 로 묶어줘야 함.

```js
var foo = {
	0: 1,
	1: 2,
	2: 3,
};

console.log(foo); // {0 : 1, 1 : 2, 2 : 3}
```

- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 <u> 암묵적 타입 변환</u> 을 통해 문자열이 됨.
  - ex) 프로퍼티 키로 숫자 리터럴을 사용하게 되면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된 상태

```js
var foo = {
	name: 'Lee',
	name: 'Kim',
};

console.log(foo); // {name : 'Kim'}
```

- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓰게 됨. (오류x)

## 10.4 메서드

## 10.4 메서드

> 💡 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라고 부름

```js
var circle {
  radius : 5,   // 프로퍼티
  getDiameter : function() {      // 메서드
    return 2 * this.radius;       // 여기서 this 는 circle (객체 자신) 의미
  }
};

console.log(circle.getDiameter());    // 10
```

<br>

## 10.5 프로퍼티 접근

- **마침표 표기법**
- **대괄호 표기법**

> - 프로퍼티 키가 식별자 네이밍 규칙을 준수한다면 위 2가지 방법 모두 사용 가능
> - 프로퍼티 키가 식별자 네이밍 규칙을 따르지 않는다면 반드시 대괄호 표기법 사용

```js
var person = {
  name:'Lee'
};

// 마침표 표기법
console.log(person.name);

// 대괄호 표기법
console.log(person['name]);
```

```jsx
var person = {
	name: 'Lee',
};

console.log(person.name); // 마침표 표기법
console.log(person['name']); // 대괄호 표기법
// console.log(person[name]);      -> ReferenceError
//console.log(person.age);         // undefined
```

- <u> 대괄호 표기법에서 프로퍼티 키는 반드시 따옴표로 감싼 문자열 </u> 이어야 함.
- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined 반환 (오류 x)

```js
var person = {
  'last-name' : 'Lee',
  1 : 10
};

person.'last-name';      // SyntaxError
person.last-name;        // 브라우저 환경 : NaN
                         // Node.js 환경 : ReferenceError

person[last-name];       // ReferenceError
person['last-name'];     // "Lee"

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표 생략 가능
person.1;                // SyntaxError
person.'1';              // SyntaxError
person[1];               // 10 : person[1] -> person['1']
person['1'];             // 10
```

## 10.6 프로퍼티 값 갱신

> 이미 존재하는 프로퍼티에값을 할당하면 프로퍼티 값 갱신

```js
var person = {
	name: 'Lee',
};

person.name = 'Kim';
console.log(person); // {name : 'Kim'}
```

## 10.7 프로퍼티 동적 생성

> 존재하지 않는 프로퍼티에 값을 할당 => 프로퍼티가 동적 생성 => 프로퍼티 값 할당

```js
var person = {
	name: 'Kim',
};

// 프로퍼티 동적 생성
person.age = 25;

console.log(person); // {name: 'Kim', age: 25}
```

## 10.8 프로퍼티 삭제

> delete 연산자를 통해 객체의 프로퍼티 삭제

```js
var person = {
	name: 'Kim',
	age: 25,
};

// age 프로퍼티 삭제
delete person.age;

// address 프로퍼티가 없지만
// 에러가 발생하지 않음
delete person.address;

console.log(person); // { name:"Kim" }
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

> ES6 에서는 더욱 간편하고 표현력 있는 객체 리터럴의 확장 기능 제공

1.  **프로퍼티 축약 표현**

```js
let x = 1;
let y = 2;

// ES5
const obj1 = {
	x: x,
	y: y,
};

// ES6
const obj2 = { x, y };
```

- 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키 생략 가능
- 이 때 프로퍼티 키는 변수 이름으로 자동 생성됨.

2. **계산된 프로퍼티 이름**

```js
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

```js
// ES6
const prefix = 'prop';
let i = 0;

const obj = {};

obj[`${prefix}-${++i}`] = i;
obj[`${prefix}-${++i}`] = i;
obj[`${prefix}-${++i}`] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

- ES5 에서는 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호 [ ] 표기법 사용
- ES6 에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 동적으로 프로퍼티 키 생성 가능

3. **메서드 축약 표현**

```js
// ES5
var obj = {
	name: 'Kim',
	sayHello: function () {
		console.log('Hi! ' + this.name);
	},
};

obj.sayHello(); // Hi! Kim
```

ES6에서는 메서드를 정의할 때 function 키워드를 생략할 수 있다.

```js
// ES5
const obj = {
	name: 'Kim',
	sayHello() {
		console.log('Hi! ' + this.name);
	},
};

obj.sayHello(); // Hi! Kim
```

- ES5 에서 메서드를 정의하려면 프로퍼티 값으로 함수 할당
- 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작
