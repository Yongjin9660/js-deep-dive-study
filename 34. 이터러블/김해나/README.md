# 34. 이터러블

## 34.1 이터레이션 프로토콜

> 💡 ES6 에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙

  <br>

- `ES5 이전의 순회 가능한 데이터 컬렉션`, 즉 배열/문자열/유사 배열 객체/DOM 컬렉션 등은 `통일된 규약없이 각자 나름의 구조를 가지고 for 문, for... in 문, forEach 메서드 등 다양한 방법으로 순회`
  <br>

- `ES6 에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일`하여 for... of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용 가능하도록 일원화
  <br>

#### 1. 이터러블

- 이터러블 프로토콜을 준수한 객체
- Symbol. iterator 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체

<br>

```jsx
const array = [1, 2, 3];

// 배열은 Array.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for... of 문으로 순회 가능
for (const item of array) {
	console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용 가능
console.log([...array]); // [1, 2, 3]

// 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용 가능
const [a, ...rest] = array;
console.log(a, rest); // 1, [ 2, 3 ]
```

- for... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로도 사용 가능
  <br>

```jsx
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 않는다.
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블 X
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for... of 문으로 순회 불가능
for (const item of obj) {
	console.log(item);
}

// 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용 불가능
const [a, b] = obj;

// 스프레드 프로퍼티 체인 (Stage 4) 은 객체 리터럴 내부에서 스프레드 문법 사용 허용
console.log({ ...obj }); // { a: 1, b : 2 }
```

- Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블 X
  <br>

- 일반 객체는 for... of 문으로 순회할 수 없으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 X
  <br>

- TC39 프로세스의 stage 4 단계에 제안되어 있는 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용
  <br>

#### 2. 이터레이터

- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터 반환
- 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 가짐.

<br>

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환한다.
// 이터레이터는 next 메서드를 갖는다.
const iterator = array[Symbol.iterator]();

// next 메서드를 호출하면 이터러블을 순회하며, 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.
// 이터레이터 리절트 객체는 value 와 done 프로퍼티를 갖는 객체이다.
console.log(iterator.next()); // { value : 1, done : false }
console.log(iterator.next()); // { value : 2, done : false }
console.log(iterator.next()); // { value : 3, done : false }
console.log(iterator.next()); // { value : undefined, done : true }
```

- 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할 수행
  <br>
- next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며, 순회 결과를 나타내는 이터레이터 리절트 객체 반환
  <br>
- 이터레이터의 next 메서드가 반환하는 이터레이터 리절트 객체의 value 프로퍼티는 현재 순회 중인 이터러블의 값을 나타내며 done 프로퍼티는 이터러블의 순회 완료 여부를 나타냄.
  <br>

## 34.2 빌트인 이터러블

> 💡 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블 제공

<br>

| 빌트인 이터러블 | Symbol.iterator 메서드                                                          |
| --------------- | ------------------------------------------------------------------------------- |
| Array           | Array.prototype[Symbol.iterator]                                                |
| String          | String.prototype[Symbol.iterator]                                               |
| Map             | Map.prototype[Symbol.iterator]                                                  |
| Set             | Set.prototype[Symbol.iterator]                                                  |
| TypedArray      | TypedArray.prototype[Symbol.iterator]                                           |
| arguments       | arguments.prototype[Symbol.iterator]                                            |
| DOM 컬렉션      | NodeList.prototype[Symbol.iterator] / HTMLCollection.prototype[Symbol.iterator] |

<br>

## 34.3 for... of 문

```jsx
for (변수선언문 of 이터러블) {...}
```

-> 이터러블을 순회하면서 이터러블의 요소를 변수에 할당
<br>

- `for... in 문`

  - 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입 프로퍼티 중에서 프로퍼티 어트리뷰트 \[[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거
  - 이때 프로퍼티 키가 심벌인 프로퍼티는 열거 X
    <br>

- `for... of 문`
  - 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회
  - next 메서드가 반환한 이터레이터 리절트 객체의 value 값을 for ... of 문의 변수에 할당
  - 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단
    <br>

## 34.4 이터러블과 유사 배열 객체

##### \*\* **`유사 배열 객체란?`**

- 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체

<br>

```js
// 유사 배열 객체
const arrayList = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let i = 0; i < arrayList.length; i++) {
	// 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
	console.log(arrayList[i]); // 1 2 3
}

// 유사 배열 객체는 이터러블이 아니기 때문에 for... of 문으로 순회 X
for (const item of arrayLike) {
  console.log(item);    // 1 2 3
}
// -> TypeError

// Array.from 메서드를 통해 유사 배열 객체 또는 이터러블을 배열로 변환
connst arr = Array.from(arrayLike);
console.log(arr);   // [1, 2, 3]
```

- length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근 가능
  <br>

- 유사 배열 객체는 이터러블이 아닌 일반 객체이기 때문에 for... of 문으로 순회 X
  - Array.from 메서드를 사용하여 배열로 간단히 변환 가능
    <br>

##### \*\* **`유사 배열 객체이면서 이터러블인 것`**

- arguments
- NodeList
- HTMLCollection
  <br>

## 34.5 이터레이션 프로토콜의 중요성

- ES6 에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for... of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용 가능하도록 일원화
  <br>

- 이터러블은 for... of 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할 수행
  <br>

![img](https://poiemaweb.com/img/iteration-protocol-interface.png)

<br>

- 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정
  - 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 `데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할 수행`
    <br>

## 34.6 사용자 정의 이터러블

> 사용자 정의 이터러블이란 `이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현한 것`

<br>

#### 1) 사용자 정의 이터러블 구현

```js
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
	// Symbol.iterator 메소드를 구현하여 이터러블 프로토콜을 준수
	[Symbol.iterator]() {
		let [pre, cur] = [0, 1];
		// 최대값
		const max = 10;

		// Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환해야 한다.
		// next 메소드는 이터레이터 리절트 객체를 반환
		return {
			// fibonacci 객체를 순회할 때마다 next 메소드가 호출된다.
			next() {
				[pre, cur] = [cur, pre + cur];
				// 이터레이터 리절트 객체를 반환한다.
				return {
					value: cur,
					done: cur >= max,
				};
			},
		};
	},
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출된다.
for (const num of fibonacci) {
	console.log(num); // 1 2 3 5 8
}
```

- 사용자 정의 이터러블은 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고 Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 함.
  <br>

- 이터레이터의 next 메서드는 done 과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환
  <br>
- for... of 문은 done 프로퍼티가 true 가 될 때까지 반복하며 done 프로퍼티가 true 가 되면 반복 중지
  <br>

- 수열의 최대값은 고정된 값으로 외부에서 전달한 값으로 변경할 방법 X

<br>

#### 2) 이터러블을 생성하는 함수

```js
// 피보나치 수열을 구현한 사용자 정의 이터러블을 반환하는 함수
// 수열의 최대값을 인수로 전달받는다.
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];

	return {
		// Symbol.iterator 메소드를 구현한 이터러블을 반환한다.
		[Symbol.iterator]() {
			return {
				next() {
					[pre, cur] = [cur, pre + cur];
					return {
						value: cur,
						done: cur >= max,
					};
				},
			};
		},
	};
};

// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
// fibonacciFunc(10) 은 이터러블을 반환한다.
for (const num of fibonacciFunc(10)) {
	console.log(num); // 1 2 3 5 8
}
```

- 수열의 최대값을 외부에서 인수로 전달받아 이터러블을 반환하는 함수
  <br>

#### 3) 이터러블이면서 이터레이터인 객체를 생성하는 함수

```jsx
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];

	// Symbol.iterator 메소드와 next 메소드를 소유한
	// 이터러블이면서 이터레이터인 객체를 반환
	return {
		// Symbol.iterator 메소드
		[Symbol.iterator]() {
			return this;
		},
		// next 메소드는 이터레이터 리절트 객체를 반환
		next() {
			[pre, cur] = [cur, pre + cur];
			return {
				value: cur,
				done: cur >= max,
			};
		},
	};
};

// iter는 이터러블이면서 이터레이터이다.
let iter = fibonacciFunc(10);

// iter는 이터레이터이다.
console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 3, done: false}
console.log(iter.next()); // {value: 5, done: false}
console.log(iter.next()); // {value: 8, done: false}
console.log(iter.next()); // {value: 13, done: true}

iter = fibonacciFunc(10);

// iter는 이터러블이다.
for (const num of iter) {
	console.log(num); // 1 2 3 5 8
}
```

- 앞서 살펴본 fibonacciFunc 함수를 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 구현
  <br>

#### 4) 무한 이터러블과 지연 평가

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return this;
		},
		next() {
			[pre, cur] = [cur, pre + cur];
			// done 프로퍼티를 생략한다.
			return { value: cur };
		},
	};
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
	if (num > 10000) break;
	console.log(num); // 1 2 3 5 8...
}

// 무한 이터러블에서 3개만을 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

- 무한 이터러블을 생성하는 함수 구현

  - 이를 통해 무한 수열을 간단하게 구현 가능
    <br>

- 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터 공급
  <br>

- 하지만 위 예제의 이터러블은 `지연 평가` 를 통해 데이터 생성
  <br>

#### \*\* **`지연 평가란?`**

- 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법
  <br>

- 즉, 평가 결과가 필요할 때까지 평가를 늦추는 기법
  - 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성
  - 실행 속도 ↑, 불필요만 메모리 소비 X, 무한 표현 가능
