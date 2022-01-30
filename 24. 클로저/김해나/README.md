# 24. 클로저

## 24.1 렉시컬 스코프

> 함수를 어디서 호출했는지가 아닌 <em> `어디에 정의했는지에 따라 상위 스코프를 결정`</em> 하는 것.

<br>

```jsx
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

- `함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않음.`

  - foo 함수와 bar 함수 모두 전역에서 정의된 전역 함수  
    <br>

- '함수의 상위 스코프를 결정한다'
  &ensp; == '렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다'
  <br>

##### \*\* 렉시컬 스코프란?

> 렉시컬 환경의 '외부 렉시컬 환경에 대한 참조'에 저장할 참조값, 즉 `상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경 (위치) 에 의해 결정됨.`

<br>

## 24.2 함수 객체의 내부 슬롯 \[[Environment]]

> 함수는 자신의 내부 슬롯 \[[Environment]] 에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장함.  
> -> \[[Environment]] 에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킴.

<br>

```jsx
const x = 1;

function foo() {
	const x = 10;

	// 상위 스코프는 함수 정의 환경 (위치) 에 따라 결정된다.
	// 함수 호출 위치와 상위 스코프는 아무런 관계 X
	bar();
}

// 함수 bar 는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]] 에 저장하여 기억한다.
function bar() {
	console.log(x);
}

foo(); // 1
bar(); // 1
```

<br>

![img](https://media.vlpt.us/images/chappi/post/d91aedfb-17d8-4761-a5d7-1e65cbf2e975/1.png)

<br>

- 함수 객체는 내부 슬롯 \[[Environment]] 에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억함.  
  <br>

- `'외부 렉시컬 환경에 대한 참조' 에는 함수 객체의 내부 슬롯 [[Environment]] 에 저장된 렉시컬 환경의 참조가 할당됨.`
  <br>

- `함수 객체의 내부 슬롯 [[Environment]] 에 저장된 렉시컬 환경의 참조가 함수의 상위 스코프 의미`
  <br>

&ensp; => 함수 정의 위치에 따라 상위 스코프를 결정하는 렉시컬 스코프의 실체
<br>

## 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

// 1번
function outer() {
	const x = 10;
	const inner = function () {
		// 2번
		console.log(x);
	};
	return inner;
}

// outer 함수를 호출하면 중첩 함수 inner 를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트에서 팝되어 제거된다.
const innerFunc = outer(); // 3번
innerFunc(); // 4번 -> 10
```

- outer 함수를 호출 (3번) 하면 outer 함수는 중첩 함수 inner 를 반환하고 생명 주기가 마감됨.
  <br>
- outer 함수의 지역 변수 x 또한 생명 주기가 마감됐지만 4번 실행 결과는 outer 함수의 지역 변수 x값인 10
  <br>
- 이미 생명 주기가 종료되어 실행 컨텍스트 스택에서 제거된 outer 함수의 지역 변수 x가 다시 동작하고 있는 것처럼 보임.
  <br>

##### \*\* 클로저란?

> `외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있는데, 이러한 중첩 함수를 '클로저' 라고 부름.`

<br>

#### \*\* 전역 함수와 중첩 함수의 상위 스코프 결정

![img](https://media.vlpt.us/images/chappi/post/76621820-0295-497a-a030-a9daf8f15a69/10.png)

<br>

- `outer 함수`가 평가되어 함수 객체를 생성할 때 (1번) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 `전역 렉시컬 환경을 outer 함수 객체의 \[[Environment]] 내부 슬롯에 상위 스코프로서 저장함.`
  <br>

- `함수 표현식으로 정의된 중첩 함수 inner` 는 런타임에 자신이 평가되면 (2번) 자신의 \[[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 `outer 함수의 렉시컬 환경을 상위 스코프로서 저장함.`

<br>

#### \*\* outer 함수의 실행 컨텍스트가 제거되어도 outer 함수의 렉시컬 환경은 유지

![img](https://media.vlpt.us/images/chappi/post/43d1d7fc-018b-4b32-af08-116613d0254a/11.png)
<br>

- outer 함수의 실행이 종료되면 inner 함수를 반환하면서 outer 함수의 생명 주기가 종료됨. (3번)
  <br>

- outer 함수의 렉시컬 환경은 inner 함수의 \[[Environment]] 내부 슬롯에 의해 참조되고 있고, inner 함수는 전역 변수 innerFunc 에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않음.
  <br>

- `이때 outer 함수의 실행 컨텍스트는 실행 컨텍스트에서 제거되지만 outer 함수의 렉시컬 환경은 소멸되지 않음.`

<br>

#### \*\* 외부 함수가 소멸해도 반환된 중첩함수는 외부 함수의 변수 참조 가능

![img](https://media.vlpt.us/images/chappi/post/4fcf977d-db29-4b34-ad51-2d703a741174/12.png)

<br>

- outer 함수가 반환한 inner 함수를 호출 (4번) 하면 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트에 푸시됨.
  <br>

- 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 \[[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당됨.
  <br>

- `외부 함수 (outer) 보다 더 오래 생존한 중첩 함수 (inner) 는 외부 함수의 생존 여부 (실행 컨텍스트의 생존 여부) 와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억함.`
  <br>

- `중첩 함수 inner 내부에서는 상위 스코프의 식별자를 참조하거나 식별자 값 변경 가능`
  <br>

#### \*\* 클로저가 아닌 경우

##### 1) 상위 스코프의 어떤 식별자도 참조하지 않는 함수

```jsx
function foo() {
	const x = 1;
	const y = 2;

	// 클로저 X
	function bar() {
		// 상위 스코프의 식별자 참조 X
		const z = 3;
		console.log(z);
	}
	return bar;
}

const bar = foo();
bar();
```

<br>

- 중첩 함수 bar 는 외부 함수 foo 보다 더 오래 유지되지만 `상위 스코프의 어떤 식별자도 참조하지 않으므로 bar 함수는 클로저 X`
  <br>

##### 2) 외부 함수보다 일찍 소멸되는 함수

```jsx
function foo() {
	const x = 1;

	// bar 함수는 클로저였지만 곧바로 소멸
	// 클로저 X
	function bar() {
		// 상위 스코프의 식별자 참조 O
		console.log(x);
	}
	bar();
}
foo();
```

- 중첩 함수 bar 는 상위 스코프의 식별자 x 를 참조하고 있으므로 클로저였지만 외부 함수 foo 의 외부로 중첩 함수 bar 가 반환되지 않음.
  <br>

- 외부 함수 foo 보다 생명 주기가 짧아 일찍 소멸되기 때문에 클로저 X
  <br>

#### \*\* 클로저 조건

```jsx
function foo() {
	const x = 1;
	const y = 2;

	// 클로저 O
	// 중첩 함수 bar 는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자 참조
	function bar() {
		console.log(x);
	}
	return bar;
}
const bar = foo();
bar();
```

- 중첩 함수가 <u> 상위 스코프의 식별자를 참조 </u> 하고 있고, <u>중첩 함수가 외부 함수보다 더 오래 유지</u>되는 경우에 한정
  <br>

- 클로저는 `상위 스코프의 식별자 중에서 기억해야 할 식별자만 기억함.`
  - 클로저인 중첩 함수 bar 는 상위 스코프의 x, y 식별자 중 x 만 참조하므로 x 변수만 기억함.
    <br>

##### \*\* 자유 변수란?

> 클로저에 의해 참조되는 상위 스코프의 변수 (위 예제의 경우 foo 함수의 x 변수)
> -> 클로저는 '자유 변수에 묶여 있는 함수'

<br>

## 24.4 클로저의 활용

> 클로저는 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용

<br>

```jsx
// 카운트 상태 변경 함수
const increase = (function () {
	// 카운트 상태 변수
	let num = 0;

	// 클로저
	return function () {
		// 카운트 상태를 1만큼 증가
		return ++num;
	};
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

1. 카운트 상태 (num 변수값) 는 increase 함수가 호출되기 전까지만 변경되지 않고 유지되어야 함.

   - `즉시 실행 함수 increase 는 한 번만 실행되므로 increase 가 호출될 때마다 num 변수가 재차 초기화될 일 X`
     <br>

2. 이를 위해 카운트 상태 (num 변수값) 는 increase 함수만이 변경할 수 있어야 함.
   - 누군가에 의해 의도치 않게 카운트 상태가 변경되지 않도록 `num 변수를 전역 변수가 아닌 increase 함수의 지역 변수로 선언`

<br>

```jsx
const counter = (function () {
	// 카운트 상태 변수
	let num = 0;

	// 클로저인 메서드를 갖는 객체를 반환한다.
	// 객체 리터럴은 스코프를 만들지 않는다.
	// 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.

	return {
		// num : 0,     -> 프로퍼티는 public 하므로 은닉되지 않는다.
		increase() {
			return ++num;
		},

		decrease() {
			return num > 0 ? --num : 0;
		},
	};
})();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

- 즉시 실행 함수가 반환하는 객체 리터럴은 즉시 실행 함수의 실행 단계에서 평가되어 객체가 됨.
- 이때 객체의 메서드도 함수 객체로 생성되는데, 객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프 생성 X

- increase, decrease 메서드의 상위 스코프는 increase, decrease 메서드가 평가되는 시점에 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경
- increase, decrease 메서드가 언제 어디서 호출되든 상관없이 increase, decrease 함수는 즉시 실행 함수의 스코프의 식별자 참조 가능
  <br>

## 24.5 캡슐화와 정보 은닉

> `'캡슐화'`는 프로퍼티와 메서드를 하나로 묶는 것을 의미
> -> 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데, 이를 `'정보 은닉'` 이라고 부름.

<br>

- 자바스크립트는 public, private, protected 와 같은 접근 제한자 X
- 자바스크립트 객체의 모든 프로퍼티/메서드는 기본적으로 public

```jsx
const Person = (function () {
	let _age = 0; // private

	// 생성자 함수
	function Person(name, age) {
		this.name = name; // public
		_age = age;
	}

	// 프로토타입 메서드
	Person.prototype.sayHi = function () {
		console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
	};

	// 생성자 함수 반환
	return Person;
})();

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined
```

- `name 프로퍼티`

  - 현재 외부로 공개되어 있으므로 외부에서 자유롭게 참조 및 변경 가능
  - public
    <br>

- `_age 변수`

  - Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조 및 변경 불가능
  - private
    <br>

- 즉시 실행 함수가 반환하는 Person 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호출한 Person.prototype.sayHi 메서드는 즉시 실행 함수가 종료된 이후 호출됨.
  <br>

- Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 \_age 를 참조할 수 있는 클로저
  <br>

```jsx
const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경됨
me.sayHi(); // Hi! My name is Lee. I am 30.
```

- 하지만 위와 같이 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 \_age 변수의 상태가 유지되지 않음.

- `Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상`

  - Person.prototype.sayHi 메서드는 즉시 실행 함수가 호출될 때 생성됨
  - 이때 Person.prototype.sayHi 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 \[[Environment]] 에 저장하여 기억함
  - 따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 `Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용함.`
    <br>

## 24.6 자주 발생하는 실수

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
	funcs[i] = function () {
		// 1번
		return i;
	};
}

for (var j = 0; j < funcs.length; j++) {
	console.log(funcs[j]()); // 2번 -> 3 출력
}
```

`1번 : 첫 번째 for 문의 코드 블록 내에서 함수가 funcs 배열의 요소로 추가됨.`

`2번 : 두 번째 for 문의 코드 블록 내에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출함.`

-> 이때 funcs 배열의 요소로 추가된 3개의 함수는 0, 1, 2 반환 X

-> for 문의 변수 선언문에서 <em> var 키워드로 선언한 i 변수는 전역 변수</em> 이기 때문에 0, 1, 2 가 순차적으로 할당됨.
(블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문)

-> funcs 배열의 요소로 추가한 함수를 호출하면 <em> 전역 변수 i 를 참조하여 i의 값 3 출력</em>
<br>

```jsx
const func = [];

for (let i = 0; i < 3; i++) {
	funcs[i] = function () {
		return i;
	};
}

for (let i = 0; i < funcs.length; i++) {
	console.log(funcs[i]()); // 0 1 2
}
```

<br>

![img](https://media.vlpt.us/images/hangem422/post/9d5ec5c9-17f4-4155-8b6c-78bbaf9089d1/javascript-closure03.png)
<br>

`let 키워드로 선언한 변수를 사용하면 for 문의 코드 블록이 반복 실행될 때마다 for 문 코드 블록의 새로운 렉시컬 환경 생성`

-> for 문의 코드 블록 내에서 정의한 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 생성된 for 문 코드 블록의 새로운 렉시컬 환경

-> for 문이 반복될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값 유지

<br><br>
\*\* 이미지 출처 :
https://velog.io/@chappi/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-24%EC%9D%BC%EC%B0%A8-%ED%81%B4%EB%A1%9C%EC%A0%80Closures
https://velog.io/@hangem422/js-closure
