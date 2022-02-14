# 34. 이터러블

## 34.1 이터레이션 프로토콜

> 💡 ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 자료구조를 만들기 위해 ECMAScript 사용에 정의하여 미리 약속한 규칙

- ES6 이전의 순회 가능한 데이터 컬렉션은 통일된 규약 없이 각자 나름의 구조를 가지고 for문, for ... in 문, forEach 메서드 등 다양한 방법으로 순회할 수 있었음
  - 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등

**⇒ ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for ... of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화**

이터레이션 프로토콜에는 `이터러블 프로토콜`과 `이터레이터 프로토콜`이 있음

`이터러블 프로토콜`

- Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환
- 이러한 프로토콜을 준수한 객체를 **이터러블**이라 함
  - for ... of 문으로 순회 가능
  - 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능

`이터레이터 프로토콜`

- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환
- 이터레이터는 next 메서드를 소유
  - next 메서드를 호출하면 이터러블을 순회하면 value와 done 프로퍼티를 갖는 iterator result 객체를 반환
- 이러한 규약을 **이터레이터 프로토콜**이라 함
- **이터레이터 프로토콜을 준수한 객체를 이터레이터라 함**

### 이터러블

> 💡 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체

```js
const isIterable = (v) => v != null && typeof v[Symbol.iterator] === 'function';

// 배열, 문자열, Map, Set 등은 이터러블
isIterable([]); // true
isIterable(''); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false
```

- 이터러블은 for ... of 문으로 순회할 수 있고, 스프레드 문법과 디스트럭처링 할당의 대상으로 사용 가능

```js
const array = [1, 2, 3];

console.log(Symbol.iterator in array); // true

// 이터러블인 배열은 for ... of 문으로 순회 가능
for (const item of array) {
	console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있음
consol.log([...array]); // [1, 2, 3]

const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

### 이터레이터

> 💡 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환

- 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 가짐

```js
// 배열은 이터러블 프로토콜은 준수한 이터러블
const arrray = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 가짐
console.log('next' in iterator); // true
```

- 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터 역할을 함
- next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 결과를 나타내는 **iterator result object**를 반환

```js
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

> 💡 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공

| 빌트인 이터러블 | Symbol.iterator 메서드                                                          |
| --------------- | ------------------------------------------------------------------------------- |
| Array           | Array.prototype[Symbol.iterator]                                                |
| String          | String.prototype[Symbol.iterator]                                               |
| Map             | Map.prototype[Symbol.iterator]                                                  |
| Set             | Set.prototype[Symbol.iterator]                                                  |
| TypedArray      | TypedArray.prototype[Symbol.iterator]                                           |
| arguments       | arguments.prototype[Symbol.iterator]                                            |
| DOM 컬렉션      | NodeList.prototype[Symbol.iterator] / HTMLCollection.prototype[Symbol.iterator] |

## 34.3 for ... of 문

> 💡 이터러블을 순회하면서 이터러블 요소를 변수에 할당

```js
for (변수선언문 in 객체) { ... }
```

- for ... in 문과 유사
  - 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입 프로퍼티 중에서 프로퍼티 어트리뷰트 \[[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거
  - 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않음

```js
for (변수선언문 of 이터러블) { ... }
```

- for ... of 문
  - 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회
  - next 메서드가 반환한 이터레이터 리절트 객체의 value 값을 for ... of 문의 변수에 할당
  - 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단

```js
// 이터러블
const iterable = [1, 2, 3];

for (const item of iterable) {
	// iterm 변수에 순차적으로 1, 2, 3이 할당
	console.log(item);
}

// 이터러블의 Symbol.iterator 메서드를 호출하여 이터레이터를 생성
const iterator = iterable[Symbol.iterator]();

for (;;) {
	// 이터레이터의 next 메서드를 호출하여 이터러블 순회
	// 이때 next 메서드는 이터레이터 리절트 객체를 반환
	const res = iterator.next();

	if (res.done) {
		break;
	}
	const item = res.value;
	console.log(item); // 1 2 3
}
```

## 34.4 이터러블과 유사 배열 객체

> 💡 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말함

`유사 배열 객체`

- length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있음
- 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 접근 가능

```js
// 유사 배열 객체
const arrayList = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

for (let i = 0; i < arrayList.length; i++) {
	console.log(arrayList[i]); // 1 2 3
}
```

- 유사 배열 객체는 이터러블이 아닌 일반 객체이므로 for ... of 문으로 순회할 수 없음
- arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블
- Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환

```js
const arrayList = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

## 34.5 이터레이션 프로토콜의 필요성

> 💡 데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할

- 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 하기 위함

<img src="https://user-images.githubusercontent.com/55246584/150628428-bdd551dd-3553-48ce-b328-a2f007261ced.png" height=350px>

## 34.6 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

> 💡 이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 됨

```js
const fibonacci = {
	// Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
	[Symbol.iterator]() {
		let [pre, cur] = [0, 1];
		const max = 10;

		return {
			next() {
				[pre, cur] = [cur, pre + cur];
				return { value: cur, done: cur >= max };
			},
		};
	},
};

for (const num of fibonacci) {
	console.log(num);
}
```

### 이터러블을 생성하는 함수

```js
const fibonacciFunc = function (max) {
	let [prev, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return {
				next() {
					[pre, cur] = [cur, pre + cur];
					return { value: cur, done: cur >= max };
				},
			};
		},
	};
};

for (const num of fibonacciFunc(10)) {
	console.log(num);
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수

```js
const fibonacciFunc = function (max) {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return this;
		},
		// next 메서드는 이터레이터 리절트 객체를 반환
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur, done: cur >= max };
		},
	};
};

let iter = finonacciFunc(10);

for (const num of iter) {
	console.log(num);
}
```

### 무한 이터러블과 지연 평가

```js
const fibonacciFunc = function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return this;
		},
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur };
		},
	};
};

for (const num of fibonacciFunc()) {
	if (num > 10000) {
		break;
	}
	console.log(num);
}
```

`지연평가`

- 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법
- 평가 결과가 필요할 때까지 평가를 늦추는 기법
  - next 호출 시 데이터 생성
- 빠른 실행속도
- 불필요한 메모리 소비를 줄임

### 참고

- https://poiemaweb.com/es6-iteration-for-of
