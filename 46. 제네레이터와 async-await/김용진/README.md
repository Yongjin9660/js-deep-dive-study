# 46. 제네레이터와 async/await

## 46.1 제너레이터란?

> 💡 ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

**제너레이터와 일반 함수의 차이**

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있음
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있음
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환

## 46.2 제레이터 함수의 정의

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

> 💡

## 46.5 제너레이터의 활용

> 💡

## 46.6 async/await

> 💡
