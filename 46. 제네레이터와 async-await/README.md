# 46. 제네레이터와 async/await

## 46.1 제너레이터란?

> 💡 ES6 에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

<br>

#### \*\* 제너레이터와 일반 함수의 차이

**1) 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**

- `일반 함수`를 호출하면 제어권이 함수에게 넘어가기 때문에 함수 호출자는 함수를 호출한 이후에 함수 실행을 제어할 수 없음.
- `제너레이터 함수`는 함수 호출자에게 함수의 제어권을 양도할 수 있어 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있음.
  <br>

**2) 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**

- `일반 함수`를 호출하면 매개변수를 통해 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없음.
- `제너레이터 함수`는 함수 호출자에게 상태를 전달할 수 있고, 함수 호출자로부터 상태를 전달받을 수도 있음.
  <br>

**3) 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.**

- `일반 함수`를 호출하면 함수 코드를 일괄 실행하고 값을 반환
- `제너레이터 함수`를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환
  <br>

## 46.2 제너레이터 함수의 정의

- 제너레이터 함수는 `function*` 키워드로 선언
- 하나 이상의 yield 표현식을 포함
- 이 둘을 제외하면 일반 함수를 정의하는 방법과 동일

<br>

```jsx
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
class MyClass {
	*getClsMethod() {
		yield 1;
	}
}
```

<br>

```jsx
// 아래 4가지 제너레이터 함수는 모두 유효

function* getFunc() {
	yield 1;
}

function* getFunc() {
	yield 1;
}

function* getFunc() {
	yield 1;
}

function* getFunc() {
	yield 1;
}
```

- 애스터리스크 (\*) 의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관 X
  - 하지만 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장

<br>

```jsx
// 제너레이터 함수는 화살표 함수로 정의할 수 없음.
const getArrowFunc = * () => {
  yield 1;
}; // SyntaxError

// 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없음.
function* getFunc() {
  yield 1;
}

new getFunc(); // TypeError
```

- **제너레이터 함수는 화살표 함수로 정의하거나 new 연산자와 함께 생성자 함수로 호출 X**

<br>

## 46.3 제너레이터 객체

```jsx
// 제너레이터 함수
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = getFunc();
```

- 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 **`제너레이터 객체를 반환`**
  <br>

```jsx
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체
console.log(Symbol.iterator in generator); // true

// 이터레이터는 next 메서드를 갖는다.
console.log("next" in generator); // true
```

- 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터
  - value, done 프로퍼티를 갖는 **`이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터`**
  - Symbol.iterator 메서드를 호출해서 별도로 이터레이터를 생성할 필요 X

<br>

---

<br>

##### - 제너레이터 객체는 next 메서드뿐만 아니라 **이터레이터에는 없는 return, throw 메서드를 별도로 가짐.**

<br>

- `next 메서드`를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하며 value 프로퍼티 값으로 `yield 된 값`을, done 프로퍼티 값으로 `false` 를 갖는 이터레이터 리절트 객체를 반환
  <br>
- `return 메서드`를 호출하면 value 프로퍼티 값으로 `인수로 전달받은 값`을, done 프로퍼티 값으로 `true` 를 갖는 이터레이터 리절트 객체를 반환
  <br>
- `throw 메서드`를 호출하면 인수로 전달받은 에러를 발생시키고 value 프로퍼티 값으로 `undefined` 를, done 프로퍼티 값으로 `true` 를 갖는 이터레이터 리절트 객체를 반환

<br>

```jsx
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

// 1) next 메서드
console.log(generator.next()); // { value : 1, done : false }

// 2) return 메서드
console.log(generator.return("End!")); // { value : "End!", done : true }

// 3) throw 메서드
console.log(generator.throw("Error!")); // { value : undefined, done : true }
```

<br>

## 46.4 제너레이터의 일시 중지와 재개

- 제너레이터는 `yield` 키워드와 `next` 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있음.
  <br>

- **제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행**

  - 단, 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 `yield 표현식까지만 실행`
    <br>

- **yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환**

<br>

```jsx
// 제너레이터 함수
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = getFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value : 1, done : false }

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value : 2, done : false }

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지
console.log(generator.next()); // { value : 3, done : false }

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행
console.log(generator.next()); // { value : undefined, done : true }
```

- **제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지됨.**
  - 이때 함수의 제어권이 호출자로 양도되는 것

<br>

- **제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환**
  - `value 프로퍼티` : yield 표현식에서 yield 된 값이 할당
  - `done 프로퍼티` : 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당

<br>

```js
function* getFunc() {
	// 처음 next를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지
	// 이때 yield 된 값 1 은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당
	// x 변수에는 아직 아무것도 할당되지 않았음. x 변수의 값은 next 메서드가 두번째 호출될 때 결정됨.
	const x = yield 1;

	// 두 번째 next 메서드를 호출할 때 할당된 10 은 첫 번째 yield 표현식을 할당받는 x 변수에 할당됨.
	// 즉, const x = yield 1; 은 두 번째 next 메서드를 호출했을 때 완료됨.
	// 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지됨.
	// 이때 yield 된 값 x + 10 은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당됨.
	const y = yield x + 10;

	return x + y;
}

// 제너레이터 함수 호출 -> 제너레이터 객체 반환
const generator = getFunc();

// 처음 next 메서드에는 인수를 전달하지 않으며, 인수를 전달해도 무시됨.
let res = generator.next();
console.log(res); // { value: 1, done: false }

// next 메서드에 인수로 전달한 10 은 getFunc 함수의 x 변수에 할당됨.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield 된 값 20 이 할당됨.
res = generator.next(10);
console.log(res); // { value: 20, done: false }

// next 메서드에 인수로 전달한 20 은 getFunc 함수의 y 변수에 할당됨.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30 이 할당됨.
res = generator.next(20);
console.log(res); // { value: 30, done: true }
```

- 이터레이터의 next 메서드와 달리 **제너레이터 객체의 next 메서드에는 인수 전달이 가능**

  - `next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당됨.`
    <Br>

- **제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있음.**
  - 이러한 특성을 활용하여 비동기 처리를 동기 처리처럼 구현 가능
    <br>

## 46.5 제너레이터의 활용

#### 1) 이터러블의 구현

> 💡 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수하여 이터러블을 생성하는 방식보다 더 간단하게 구현 가능

<br>

##### \*\* 무한 피보나치 수열을 생성하는 일반 함수 구현

```jsx
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() {
			return this;
		},
		next() {
			[pre, cur] = [cur, pre + cur];
			// 무한 이터러블이므로 done 프로퍼티를 생략
			return { value: cur };
		},
	};
})();

// infiniteFibonacci 는 무한 이터러블
for (const num of infiniteFinbonacci) {
	if (num > 10000) break;
	console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

<Br>

##### \*\* 무한 피보나치 수열을 생성하는 제너레이터 함수 구현

```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
	let [pre, cur] = [0, 1];

	while (true) {
		[pre, cur] = [cur, pre + cur];
		yield cur;
	}
})();

// infiniteFibonacci 는 무한 이터러블
for (const num of infiniteFinbonacci) {
	if (num > 10000) break;
	console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

<Br>

#### 2) 비동기 처리

> 💡 제너레이터 함수의 next 메서드와 yield 표현식을 통해 프로미스의 후속 처리 메서드 then/catch/finally 없이도 비동기 처리 결과를 반환하도록 구현 가능

<br>

```js
// node-fetch 는 Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지
const fetch = require("node-fetch");

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
		const url = "https://jsonplaceholder.typicode.com/todos/1";

		const response = yield fetch(url);
		const todo = yield response.json();
		console.log(todo);
	})()
);
```

- 제너레이터를 사용해서 비동기 처리를 동기 처리처럼 동작하도록 구현했지만 가독성 ↓
  → `ES8 에서 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await 도입`

## 46.6 async/await

> 💡 프로미스의 후속 처리 메서드 then/catch/finally 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능
> `→ async/await 는 프로미스를 기반으로 동작`

<br>

```js
const fetch = require("node-fetch");

async function fetchTodo() {
	const url = "https://jsonplaceholder.typicode.com/todos/1";

	const response = await fetch(url);
	const todo = await response.json();

	console.log(todo);
	// { userId : 1, id : 1, title : 'delectus aut autem', conpleted : false }
}

fetchTodo();
```

<br>

#### 1) async 함수

- async 키워드를 사용해 정의하며 언제나 프로미스를 반환
- async 함수가 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve 하는 프로미스를 반환
  <br>

```jsx
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

// async 메서드
const obj = {
	async foo(n) {
		return n;
	},
};
obj.foo(4).then((v) => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
	async bar(n) {
		return n;
	}
}
const myClass = new MyClass();
MyClass.bar(5).then((v) => console.log(v)); // 5
```

<br>

```jsx
class MyClass {
  async constructor() { }
  // SyntaxError
}

const myClass = new MyClass();
```

- 클래스의 constructor 메서드는 async 메서드가 될 수 없음.
  - 클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환하기 때문
    <br>

#### 2) await 키워드

- await 키워드는 반드시 async 함수 내부에서 사용해야 하며, 프로미스 앞에서 사용해야 함.
- await 키워드는 프로미스가 settled 상태 (비동기 처리가 수행된 상태) 가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 처리 결과를 반환

<br>

```js
const fetch = require("node-fetch");

const getGithubUserName = async (id) => {
	const res = await fetch(`https://api.github.com/users/${id}`);
	const { name } = await res.json();
	console.log(name); // Ungmo Lee
};

getGithubUserName("ungmo2");
```

<br>

#### 3) 에러 처리

- 에러는 호출자 방향으로 전파됨.
  - 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try... catch 문으로 에러 캐치 X
    <br>
- async/await 에서 에러 처리는 try... catch 문 사용 가능
  - 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 **프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확함.**
    <br>

```js
const fetch = require("node-fetch");

const foo = async () => {
	try {
		const wrongUrl = "https://wrong.url";

		const response = await fetch(wrongUrl);
		const data = await response.json();
		console.log(data);
	} catch (err) {
		console.log(err); // TypeError
	}
};

foo();
```

- 위 예제의 foo 함수는 네트워크 에러뿐 아니라 try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치 O

<br>

```js
const fetch = require("node-fetch");

const foo = async () => {
	const wrongUrl = "https://wrong.url";

	const response = await fetch(wrongUrl);
	const data = await response.json();
	return data;
};

foo().then(console.log).catch(console.error); // TypeError
```

- **async 함수 내에서 catch 문을 사용하여 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환**
  <Br>
- async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용하여 에러를 캐치하는 것도 가능
