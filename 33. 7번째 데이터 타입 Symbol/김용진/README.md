# 33. 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

> 💡 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값

- 다른 값과 중복되지 않는 유일무이한 값

## 33.2 심벌 값의 생성

**Symbol 함수**

> 💡 심벌 값은 Symbol 함수를 호출하여 생성

```js
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없음
console.log(mySymbol); // Symbol()
```

**Symbol.for / Symbol.keyFor 메서드**

> 💡 Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
- 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

> 💡 Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있음

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 지정된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않음
const s2 = Symbol('foo');
Symbol.keyFor(s2); // undefined
```

## 33.3 심벌과 상수

```js
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있음
const Direction = {
	UP: 1,
	DOWN: 2,
	LEFT: 3,
	RIGHT: 4,
};
```

- 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우
  - 상수 값은 변경될 수 있고, 다른 변수와 중복될 수 있음

```js
const Direction = {
	UP: Symbol('up'),
	DOWN: Symbol('down'),
	LEFT: Symbol('left'),
	RIGHT: Symbol('right'),
};
```

## 33.4 심벌과 프로퍼티 키

> 💡 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않음

```js
const obj = {
	// 심벌 값으로 프로퍼티 키를 생성
	[Symbol.for('mySymbol')]: 1,
};

obj[Symbol.for('mySymbol')]; // 1
```

## 33.5 심벌과 프로퍼티 은닉

> 💡 심볼 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없음

```js
const obj = {
	// 심벌 값으로 프로퍼티 키를 생성
	[Symbol.for('mySymbol')]: 1,
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 33.6 심벌과 표준 빌트인 객체 확장

```js
// 표준 빌트인 객체를 확장하는 것은 권장하지 않음
Array.prototype.sum = function () {
	return this.reduce((acc, cur) => acc + cur, 0);
};
```

표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하지 않아야함

- 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있음

```js
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전

Array.prototype[Symbol.for('sum')] = function () {
	return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```

## 33.7 Well-known Symbol

> 💡 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에는 Well-known Symbol이라 부름

![image](https://user-images.githubusercontent.com/55246584/153143526-b1d84872-9116-469b-a0c0-3dc510687c14.png)

- 자바스크립트 엔진의 내부 알고리즘에 사용
- for ... of로 순회 가능한 빌트인 이터러블 이터러블
  - Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection
  - Well-known인 Symbol.iterator를 키로 갖는 메서드를 가짐
  - Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있음

```js
const iterable = {
	// Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
	[Symbol.iterator]() {
		let cur = 1;
		const max = 5;
		// Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
		return {
			next() {
				return { value: cur++, done: cur > max + 1 };
			},
		};
	},
};

for (const num of iterable) {
	console.log(num); // 1 2 3 4 5
}
```

- Symbol.iterator는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않음
- 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해 도입
  - 하위 호환성을 보장하기 위해
