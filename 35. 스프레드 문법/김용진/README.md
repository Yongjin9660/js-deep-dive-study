# 35. 스프레드 문법

> 💡 ES6에서 도입된 스프레드 문법은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만듦

- Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments와 같이 for ... of 문으로 순회할 수 있는 이터러블에 한정

```js
console.log(...[1, 2, 3]); // 1 2 3

console.log(...'Hello'); // H e l l o

console.log(
	...new Map([
		['a', '1'],
		['b', '2'],
	])
); // ['a', '1'] ['b', '2']
console.log(...new Set([1, 2, 3])); /// 1 2 3
```

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

> 💡 요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 함수의 인수 목록으로 전달해야 하는 경우

```js
const arr = [1, 2, 3];

Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
Math.max(...arr); // 3
```

## 35.2 배열 리터럴 내부에서 사용하는 경우

**concat**

```js
// 2개의 배열을 하나로 결합

// ES5
var arr1 = [1, 2].concat([3, 4]);
console.log(arr1); // [1,2,3,4]

// ES6
const arr2 = [...[1, 2], ...[3, 4]];
console.log(arr2); // [1,2,3,4]
```

**splice**

```js
// 특정 배열의 중간에 다른 배열의 요소를 추가하거나 제거

// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); //[1, 2, 3, 4];

// ES6
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1,2,3,4]
```

**배열 복사**

```js
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false

// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1,2]
console.log(copy === origin); // false
```

**이터러블을 배열로 변환**

```js
// ES5에서 이터러블을 배열로 변환하려면
// Function.prototype.apply 또는 Function.prototype.call 메서드를 사용하여 slice 메서드를 호출해야 함

// ES5
function sum() {
	// 이터러블이면서 유사 배열 객체인 argumetns를 배열로 변환
	var args = Array.prototype.slice.call(arguments);

	return args.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}

// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
	0: 1,
	1: 2,
	2: 3,
	length: 3,
};

const arr = Array.prototype.slice.call(arrayList); // [1, 2, 3]

// ES6

// 스프레드 문법 사용
function sum() {
	return [...argumets].reduce((pre, cur) => pre + cur, 0);
}

// Rest 파라미터
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

```js
// 스프레드 프로퍼티
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };

console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```
