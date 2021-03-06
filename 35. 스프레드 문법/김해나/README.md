# 35. 스프레드 문법

> **하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만드는 것**
> -> 스프레드 문법을 사용할 수 있는 대상은 for... of 문으로 순회할 수 있는 이터러블에 한정됨.
> -> ex) Array, String, Map, Set, DOM 컬렉션, arguments

<br>

```jsx
// ...[1, 2, 3] 은 [1, 2, 3] 을 개별 요소로 분리 -> 1, 2, 3
console.log(...[1, 2, 3]);    // 1 2 3

// 문자열은 이터러블
console.log(...'Hello');    // H e l l o

// Map 과 Set 은 이터러블
console.log(...new Map([['a', '1'], ['b', '2']]));    // [ 'a', '1' ] [ 'b' , '2' ]
console.log(...new Set([1, 2, 3]));   // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상 X
console.log(...{ a : 1, b  2 });    // TypeError

// 스프레드 문법의 결과는 값이 아니다.
const list = ...[1, 2, 3];    // SyntaxError
```

- 스프레드 문법의 결과는 값이 아닌 `값들의 목록이기 때문에 변수에 할당할 수 없음`
- 다음과 같이` 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용 가능`
  1. 함수 호출문의 인수 목록
  2. 배열 리터럴의 요소 목록
  3. 객체 리터럴의 프로퍼티 목록
     <br>

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

```jsx
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
Math.max([1, 2, 3]); // NaN
```

- `Math.max` 메서드는 매개변수 개수를 확정할 수 없는 가변 인자 함수

  - 개수가 정해져 있지 않은 여러 개의 숫자를 인수를 전달받아 인수 중에서 최대값을 반환
  - <em> 숫자가 아닌 배열을 인수로 전달하면 NaN 반환</em>
  - `배열을 펼쳐서 요소들을 개별적인 값들의 목록으로 만든 후, Math.max 메서드의 인수로 전달해야 함. `
    <br>

```jsx
// 스프레드 문법 제공되기 이전
var arr = [1, 2, 3];

// Function.prototype.apply 사용
var max = Math.max.apply(null, arr); // 3
```

```jsx
// 스프레드 문법 도입 이후
const arr = [1], 2, 3];

// 더욱 간결하고 가독성 ↑
const max = Math.max(...arr);   // 3
```

<br>

\*\* 스프레드 문법은 Rest 파라미터와 형태가 동일하기 때문에 주의할 필요가 있음.

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 전달받음
function foo(...rest) {
	console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만듦
foo(...[1, 2, 3]); // [1, 2, 3] -> 1, 2, 3
```

- `Rest 파라미터`

  - **함수에 전달된 인수들의 목록을 배열로 전달받기 위해** 매개변수 이름 앞에 ... 을 붙임.
    <br>

- `스프레드 문법`

  - 여러 개의 값이 하나로 뭉쳐 있는 **배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만듦.**
    <br>

- Rest 파라미터 ↔ 스프레드 문법 (서로 반대 개념)
  <br>

## 35.2 배열 리터럴 내부에서 사용하는 경우

> 스프레드 문법을 사용하면 ES5 에서 사용하던 기존의 방식보다 더욱 간결하고 가독성 좋게 표현 가능

<br>

##### \*\* concat

> 2개의 배열을 1개의 배열로 결합하고 싶은 경우

<br>

```jsx
// ES5 : concat
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6 : 스프레드 문법
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

<br>

##### \*\* splice

> 배열 중간에 다른 배열의 요소들을 추가하거나 제거하고 싶은 경우

<br>

```jsx
// ES5 : splice
var arr1 = [1, 4];
var arr2 = [2, 3];

// splice 메서드의 3번째 인수에 배열을 전달하면 배열 자체가 추가됨.
arr1.splice(1, 0, arr2); // [1, [2, 3], 4]

// 요소들을 해체해서 전달하기 위해 apply 메서드를 이용하여 splice 메서드 호출
// arr1[1] 부터 0개의 요소를 제거하고 그 자리에 새로운 요소 (2, 3) 삽입
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]

// ES6 : 스프레드 문법
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

<br>

##### \*\* 배열 복사

> 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본을 생성하고 싶은 경우

<br>

```jsx
// ES5 : slice
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(origin === copy); // false

// ES6 : 스프레드 문법
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(origin === copy); // false
```

<br>

##### \*\* 이터러블을 배열로 변환

> 이터러블 또는 이터러블이 아닌 유사 배열 객체를 배열로 변환하고 싶은 경우

<br>

```jsx
// ES5 : apply/call 메서드를 이용하여 slice 메서드 호출
function sum() {
	// 이터러블이면서 유사 배열 객체인 arguments 를 배열로 변환
	var args = Array.prototype.slice.call(arguments);

	return args.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}

console.log(sum(1, 2, 3)); // 6

// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

const arr = Array.prototype.slice.call(arrayLike); // [1, 2, 3]
console.log(Array.isArray(arr)); // true
```

<br>

```jsx
// ES6 : 스프레드 문법
function sum() {
	// 이터러블이면서 유사 배열 객체인 arguments 를 배열로 변환
	return [...arguments].reduce((pre, cure) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6

// ES6 : Rest 파라미터
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
console.log(sum(1, 2, 3)); // 6

// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

const arr = [...arrayLike]; // TypeError
```

- 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없음.
  <br>

\*\* 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6 에서 도입된 `Array.from` 메서드 사용

```jsx
// Array.from 은 유사 배열 객체 또는 이터러블을 배열로 변환
Array.from(arrayLike); // [1, 2, 3]
```

<br>

## 35.3 객체 리터럴 내부에서 사용하는 경우

> 스프레드 프로퍼티를 사용하면 **객체 리터럴의 프로퍼티 목록에서도 스프레드 문법 사용 가능**
> → `스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용함.`

<br>

```jsx
// 스프레드 프로퍼티
// 객체 복사 (얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };

console.log(copy); // { x : 1, y : 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x : 1, y : 2, a : 3, b : 4}
```

<br>

```jsx
// Object.assign 메서드 사용하는 예제

// 객체 병합
// 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 가짐
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x : 1, y : 10, z : 3}

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y : 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 3 });
console.log(added); // { x : 1, y : 2, z : 3 }
```

- 스프레드 프로퍼티가 제안되기 이전에는 ES6 에서 도입된 <em>Object.assign 메서드 </em> 사용
  → 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 및 추가

<br>

```jsx
// 스프레드 프로퍼티 사용하는 예제

// 객체 병합
// 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 가짐
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x : 1, y : 10, z : 3}

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// const changed = { ...{ x : 1, y : 2 }, ...{ y : 100 } };
console.log(changed); // { x: 1, y : 100 }

// 프로퍼티 추가
const changed = { ...{ x: 1, y: 2 }, z: 3 };
//const added = { ...{ x : 1, y : 2 }, ...{ z : 3 } };
console.log(added); // { x : 1, y : 2, z : 3 }
```

- 스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법
