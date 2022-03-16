# 46. 제너레이터와 async/await

## 46.1 제너레이터란?

> 💡 ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

**제너레이터와 일반 함수의 차이**

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있음
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있음
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환

## 46.2 제너레이터 함수의 정의

> 💡 제너레이터 함수는 function\* 키워드로 선언

```js
// 제너레이터 함수 선언문
function* getDecFunc() {
	yield 1;
}

// 제너레이터 함수 표현식
const getExpFunc = function* () {
	yield 1;
};

// 제너레이터 메서드
const obj = {
	*getObjMethod() {
		yield 1;
	},
};

// 제너레이터 클래스 메서드
class Myclass {
	*getClsMethod() {
		yield 1;
	}
}
```

- 애스터리스크(\*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없음
- 하지만 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장
- 제너레이터 함수는 화살표 함수로 정의할 수 없음

```js
const genArrowFunc = () = * () => {
  yield 1;
}; // SyntaxError: Unexpected token '*'
```

- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없음

```js
function* getFunc() {
	yield 1;
}

new getFunc(); // TypeError: getFunc is not a constructor
```

## 46.3 제너레이터 객체

> 💡 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환

**제너레이터 함수가 반환한 제너레이터 객체는 `이터러블`이면서 동시에 `이터레이터`**

- Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터

```js
// 제너레이터 함수
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = getFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 가짐
console.log('next' in generator); // true
```

**제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에 없는 return, throw 메서드를 가짐**

- `next` 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환
- `return` 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로 true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환

```js
function* getFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		console.error(e);
	}
}

const generator = getFunc();

console.log(generator.next()); // { value: 1, done: false }
console.log(generator.return('End!')); // { value: "End!", done: true }
```

- `throw` 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환

```js
function* getFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		conole.error(e);
	}
}

const generator = getFunc();

console.log(generator.next()); // { value: 1, done: false }
console.log(generator.throw('Error!')); // { value: undefined, done: true }
```

## 46.4 제너레이터의 일시 중지와 재개

> 💡 yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환

```js
// 제너레이터 함수
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 가짐
const generator = getFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value: 1, done: false }

// next를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value: 2, done: false }

// next를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value: 3, done: false }

// next를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행
console.log(generator.next()); // { value: undefined, done: true }
```

- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지됨
  - 함수의 제어권이 호출자로 양도
  - value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환

<br />

**제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당**

```js
function* getFunc() {
	// 처음 next를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지
	// 이때 yield 된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당
	// x 변수에는 아직 아무것도 할당되지 않음. x 변수의 값은 next가 두번째 호출될 때 결정
	const x = yield 1;

	// 두 번째 next 메서드를 호출할 때 할당된 10은 첫 번째 yield 표현식을 할당받는
	// x 에 저장
	const y = yield x + 10;

	return x + y;
}

const generator = getFunc();

// 처음 next 메서드에는 인수를 전달하지 않음
// 처음 호출하는 next에 인수를 전달하면 무시됨
let res = generator.next();
console.log(res); // { value: 1, done: false }

res = generator.next(10);
console.log(res); // { value: 20, done: false }

// next 메서드에 인수로 전달한 20은 getFunc 함수의 y 변수에 할당
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당
res = generator.next(20);
console.log(res); // { value: 30, done: true }
```

👉 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있음

## 46.5 제너레이터의 활용

### 이터러블의 구현

**이터레이션 프로토콜을 준수하여 무한 피보나치 수열을 생성하는 함수**

```js
const infiniteFibonacci = (function () {
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
})();

for (const num of infiniteFibonacci) {
	if (num > 10000) break;
	console.log(num); // 1 2 3 5 8 ...
}
```

**제너레이터를 사용하여 무한 피보나치 수열을 생성하는 함수**

```js
const infiniteFibonacci = (function* () {
	let [pre, cur] = [0, 1];

	while (true) {
		[pre, cur] = [cur, pre + cur];
		yield cur;
	}
})();

for (const num of infiniteFibonacci) {
	if (num > 10000) break;
	console.log(num); // 1 2 3 5 8 ...
}
```

### 비동기 처리

```js
const fetch = require('node-fetch');

// 제너레이터 실행기
const async = (generatorFunc) => {
	const generator = generatorFunc();

	const onResolved = (arg) => {
		const result = generator.next(arg);

		return result.done ? result.value : result.value.then((res) => onResolved(res));
	};
};

async(
	(function* fetchTodo() {
		const url = 'https://jsonplaceholder.typicode.com/todos/1';

		const response = yield fetch(url);
		const todo = yield response.json();
		console.log(todo);
	})()
);
```

## 46.6 async/await

> 💡 ES8에서 간단하고 가독성 좋게 비동기 처리를 동기처럼 동작하도록 async/await가 도입됨

```js
const fetch = require('node-fetch');

async function fetchTodo() {
	const url = 'https://jsonplaceholder.typicode.com/todos/1';

	const response = await fetch(url);
	const todo = await response.json();
	console.log(todo);
}
```

### async 함수

> 💡 async 함수는 언제나 프로미스를 반환

```js
// async 함수 선언문
async function foo(n) {
	return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
	return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3
```

### await 키워드

> 💡 await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환

```js
const fetch = require('node-fetch');

const getGithubUserName = async (id) => {
	const res = await fetch(`https://api.github.com/users/${id}`);
	const { name } = await res.json();
	console.log(name);
};
```

### 에러 처리

> 💡 async/await에서 에러 처리는 try ... catch 문을 사용 가능

```js
const fetch = require('node-fetch');

const foo = () => {
	try {
		const wrongUrl = 'https://wrong.url';

		const response = await fetch(wrongUrl);
	} catch (err) {
		console.log(err); // TypeError: Failed to fetch
	}
};

foo();
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환
