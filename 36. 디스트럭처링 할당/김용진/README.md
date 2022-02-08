# 36. 디스트럭처링 할당

> 💡 구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말함

## 36.1 배열 디스트럭처링 할당

> 💡 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스

```js
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3
```

```js
// ES6
const arr = [1, 2, 3];

const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3

// 기본값
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

// 기본값보다 할당된 값이 우선시
const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3

// Rest 요소
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

## 36.2 객체 디스트럭처링 할당

> 💡 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체이어야 하며, 할당 기준은 프로퍼티 키

```js
// ES5
var user = { firstName: 'Ungmo', lastName: 'Kim' };
var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Ungmo Kim
```

```js
const user = { firstName: 'Ungmo', lastName: 'Kim' };

const { lastName, firstName } = user;
console.log(firstName, lastName); // Ungmo Kim

const str = 'Hello';
// String 래퍼 객체로부터 length 프로퍼티만 추출
const { length } = str;
console.log(length); // 5

// 중첩 객체
const user = {
	name: 'Kim',
	address: {
		zipCode: '03068',
		city: 'Seoul',
	},
};

const {
	address: { city },
} = user;

// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1 { y: 2, z: 3 }
```
