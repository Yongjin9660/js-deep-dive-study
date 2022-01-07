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

`프로퍼티` : 객체의 상태를 나타내는 값 (data)

`메서드` : 프로퍼티를 참조하고 조작할 수 있는 동작 (behavior)

## 10.2 객체 리터럴에 의한 객체 생성

**객체 생성 방법**

- **객체 리터럴** (가장 일반적)
- Object 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

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

- 프로퍼티 키
  - 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  - 암묵적 타입변환을 통해 문자열이 됨
  - 식별자 네이밍 규칙
    - 따른다면 따옴표 생략 가능
    - 따르지 않는다면 반드시 따옴표 사용
- 프로퍼티 값
  - 자바스크립트에서 사용할 수 있는 모든 값

## 10.4 메서드

> 💡 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라고 부름

## 10.5 프로퍼티 접근

프로퍼티에 접근하는 방법

- 마침표 표기법
- 대괄호 표기법

```js
var person = {
  name:'Lee'
};

// 마침표 표기법
console.log(person.name);

// 대괄호 표기법
console.log(person['name]);
```

## 10.6 프로퍼티 값 갱신

> 이미 존재하는 프로퍼티에값을 할당하면 프로퍼티 값 갱신

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

- 프로퍼티 축약 표현

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

- 계산된 프로퍼티 이름

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

- 메서드 축약 표현

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
