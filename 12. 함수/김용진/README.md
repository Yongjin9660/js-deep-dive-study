# 12. 함수

## 12.1 함수란?

> 💡 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것

## 12.2 함수를 사용하는 이유

- 코드의 재사용
- 유지보수의 편의성 향상
- 코드의 신뢰성 향상
- 코드의 가독성 향상

## 12.3 함수 리터럴

- 함수도 리터럴로 생성할 수 있다.
- 함수는 객체이다.
- 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.

```js
// 변수에 함수 리터럴을 담당
const f = function add(x, y) {
	return x + y;
};
```

## 12.4 함수 정의

1. **함수 선언문**

```js
// 함수 선언문
function add(x, y) {
	return x + y;
}
```

- 함수 선언문은 함수 리터럴과 형태가 동일
- 함수 선언문은 함수 이름을 생략할 수 없음

2. **함수 표현식**

- 자바스크립트의 함수는 일급 객체
  `일급 객체` : 값처럼 변수에 할당할 수도 있고 프로퍼티 값이 될수도 있고 배열의 요소가 될 수도 있음

  ```js
  // 함수 표현식
  const add = function foo(x, y) {
  	return x + y;
  };
  // 함수 객체를 가리키는 식별자로 호출
  console.log(add(2, 5)); // 7

  // 함수 이름은 함수 몸체  내부에서만 유효
  console.log(foo(2, 5)); // ReferenceError: foo is not defined
  ```

3. **함수 생성 시점과 함수 호이스팅**

```js
// 함수 참조
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
	return x + y;
}

// 함수 표현식
var sub = function (x, y) {
	return x + y;
};
```

- **함수 선언문**
  - 런타임 이전에 함수 객체가 먼저 생성
  - 자바스크립트 엔진은 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성
  - 생성된 함수 객체를 할당
  - 즉, 런타임에는 이미 함수 객체가 생성되어 함수 이름과 동일한 식별자에 할당까지 완료
  - `함수 호이스팅`
- **함수 표현식**
  - 할당문이 실행되는 시점, 런타임에 평가됨
  - 할당문이 실행되는 시점에 평가되어 함수 객체가 됨
  - 변수 호이스팅이 발생

4. **Function 생성자 함수**

```js
var add = new Function('x', 'y', 'return x+y');

console.log(add(2, 5)); // 7
```

- 일반적이지도 않고 바람직하지 않음
  - 클로저를 생성하지 않는 등, 다르게 동작

5. **화살표 함수**

```js
const add = (x, y) => x + y;
console.log(add(2, 5)); // 7
```

- 내부 동작이 간략화되어 있음
  - 생성자 함수로 사용할 수 없음
  - this 바인딩 방식이 다름
  - prototype 프로퍼티가 없음
  - arguments 객체를 생성하지 않음

## 12.5 함수 호출

1. **매개변수와 인수**

```js
// 함수 선언문
function add(x, y) {
	return x + y;
}

// 함수 호출
// 인수 1과 2가 매개변수 x와 y에 순서대로 할당되고 함수 몸체의 문들이 샐행
var result = add(1, 2);
```

- 매개변수는 함수를 정의할 때 선언
- 함수 몸체 내부에서 변수와 동일하게 취급
- 일반 변수와 마찬가지로 undefined 로 초기화된 후 할당

2. **인수 확인**

```js
function add(x, y) {
	return x + y;
}

console.log(add(2)); // NaN
console.log(add('a', 'b')); // 'ab'
```

- 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않음
- 자바스크립트는 동적 타입 언어 => 매개변수의 타입을 사전에 지정할 수 없음

3. **매개변수의 최대 개수**

> ECMAScript 사양에서는 매개변수의 최대 개수에 대해 명시적으로 제한하고 있지 않음

- **이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 함**

4. **반환문**

```js
function multiply(x, y) {
	return x * y; // 반환문
	console.log('실행되지 않음');
}
console.log(multiply(3, 5)); // 15
```

- 반환문은 생략가능 => undefined 반환

## 12.6 참조에 의한 전달과 외부 상태의 변경

```js
function change(primitive, obj) {
	primitive += 100;
	obj.name = 'Kim';
}

// 외부 상태
let num = 100;
const person = { name: 'Lee' };

console.log(num); // 100
console.log(person); // {name: 'Lee'}

// 원시 값은 값 자체가 복사되어 전달
// 객체는 참조 값이 복사되어 전달
change(num, person);

console.log(num); // 100
console.log(person); // {name: 'Kim'}
```

- **원시값**
  - `immutable value`
  - 직접 변경할 수 없기 때문에 재할당을 통해 할당된 원시 값을 새로운 원시 값으로 교체
- **객체**
  - `mutable value`
  - 직접 변경할 수 있기 때문에 재할당 없이 직접 할당된 객체를 변경

## 12.7 다양한 함수의 형태

1. **즉시 실행 함수**

- IIFE, Immediately Invoked Function Expression
- 단 한 번만 호출됨

```js
let result = (function () {
	return 3 * 4;
})();

console.log(result); // 12

result = (function (a, b) {
	return a * b;
})(3, 5);

console.log(result); // 15
```

2. **재귀 함수**

- 자기 자신을 호출하는 함수

3. **중첩 함수**

- `nested function` or `inner function`
- 함수 내부에 정의된 함수
- 일반적으로 외부 함수를 돕는 `헬퍼 함수`로 동작

```js
function outer() {
	var x = 1;

	// 중첩 함수
	function inner() {
		var y = 2;
		// 외부 함수의 변수 참조
		console.log(x + y);
	}

	inner();
}

outer();
```

4. **콜백 함수**

```js
// 외부에서 전달받은 f를 n만큼 반복 호출
function repeat(n, f) {
	for (let i = 0; i < n; i++) {
		f(i); // i 를 전달하면서 호출
	}
}

const logAll = function (i) {
	console.log(i);
};

repeat(5, logAll); // 0 1 2 3 4

const logOdds = function () {
	if (i % 2) console.log(i);
};

repeat(5, logOdds); // 1 3
```

- `콜백함수`
  - Callback Function
  - 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수
  - 헬퍼 함수의 역할
- `고차함수`
  - Higher-Order Function
  - 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수
  - 콜백 함수를 자신의 일부분으로 합성

5. **순수 함수와 비순수 함수**

- **순수 함수**
  - 부수 효과가 없는 함수
  - 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수
  - 외부 상태에 의존하지 않음
- **비순수 함수**
  - 부수 효과가 있는 함수
