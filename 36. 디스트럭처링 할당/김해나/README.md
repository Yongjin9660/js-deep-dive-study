# 36. 디스트럭처링 할당

> 구조화된 배열과 같은 이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 개별적으로 할당하는 것
> → 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용

<br>

## 36.1 배열 디스트럭처링 할당

\*\* **`ES5 에서의 배열 디스트럭처링 할당`**

```jsx
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3
```

<br>

\*\* **`ES5 에서의 배열 디스트럭처링 할당`**

```jsx
const arr = [1, 2, 3];

// 변수 one, two, three 를 선언하고 배열 arr 을 디스트럭처링하여 할당
// 이때 할당 기준은 배열의 인덱스
const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

- 배열 디스트럭처링 `할당의 대상 (할당문의 우변) 은 이터러블`이어야 한다.
- 이때 `할당 기준은 배열의 인덱스`, 즉 순서대로 할당된다.
  <br>

```jsx
// 할당 연산자 왼쪽에 값을 할당받을 변수 선언
// 이때 변수는 배열 리터럴 형태로 선언
const [x, y] = [1, 2];

// 우변이 이터러블이 아니라면 에러 발생
const [x, y];   // SyntaxError
const [a, b] = {}   // TypeError

// 선언과 할당 분리 가능
// 단, const 키워드로 변수 선언 X
let x, y;
[x, y] = [1, 2];
```

- `값을 할당받을 변수는 배열 리터럴로 선언`
- 우변이 이터러블이 아니라면 에러 발생
- 배열 디스트럭처링 할당의 변수 선언문은 선언과 할당 분리 가능
  - 단, const 키워드로 변수 선언 X
    <br>

```jsx
// 배열 디스트럭처링 할당 기준 -> 배열의 인덱스
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3

// 변수에 기본값 설정
const [x, y, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// 기본값보다 할당된 값이 우선
const [x, y = 10, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// 변수에 Rest 요소 사용
const [x, ...y] [1, 2, 3];
console.log(x, y);    // 1 [ 2, 3 ]
```

- 배열 디스트럭처링 할당의 기준은 `배열의 인덱스, 즉 순서대로 할당된다.`
  - 이때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요 X
    <br>
- `할당되는 변수에 기본값 설정 가능`

  - 기본값보다 할당된 값이 우선
    <br>

- `할당되는 변수에 Rest 요소 사용 가능`

  - Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 함
    <br>

- 배열 디스트럭처링 할당은 `이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때 유용`
  <br>

## 36.2 객체 디스트럭처링 할당

\*\* **`ES5 에서의 객체 디스트럭처링 할당`**

```jsx
// ES5
var user = { firstName: "Ungmo", lastName: "Lee" };

var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Ungmo Lee
```

<br>

\*\* **`ES5 에서의 객체 디스트럭처링 할당`**

```jsx
const user = { firstName: "Ungmo", lastName: "Lee" };

// 변수 lastName, firstName 을 선언하고 user 객체를 디스트럭처링하여 할당
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루지며 순서는 의미 X
const { lastName, firstName } = user;
console.log(firstName, lastName); // Ungmo Lee
```

- 객체 디스트럭처링 `할당의 대상 (할당문의 우변) 은 객체`여야 한다.
  - 이때 `할당 기준은 프로퍼티 키`, 즉 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.
    <br>

```jsx
// 변수는 객체 리터럴로 선언
const { lastName, firstName } = { firstName : 'Ungmo', lastName : 'Lee' }

// 우변에 객체 또는 객체로 평가될 수 있는 표현식이 아니라면 에러 발생
const { lastName, firstName };    // SyntaxError
const { lastName, firstName } = null;   // TypeError

// 변수에 기본값 설정
const { firstName = 'Ungmo', lastName } = { lastName : 'Lee' };
console.log(firstName, lastName);   // Ungmo Lee

// 변수에 Rest 프로퍼티 사용
const { x, ...rest } = { x : 1, y : 2, z : 3 };
console.log(x, rest);   // 1, { y : 2, z : 3 }

// 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당하는 경우
const user = { firstName : 'Ungmo', lastName : 'Lee' };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName 인 프로퍼티 값을 ln 에 할당하고,
// 프로퍼티 키가 firstName 인 프로퍼티 값을 fn 에 할당한다.
const { lastName : ln, firstName : fn } = user;
console.log(fn, ln);
```

- 프로퍼티 값을 할당받을 변수는 `객체 리터럴 형태로 선언`해야 한다.
  <br>
- `우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러 발생`
  <br>
- `변수에 기본값 설정 가능`
  <br>
- `변수에 Rest 프로퍼티 사용 가능`
  - Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 함.
    <br>
- 객체 디스트럭처링 할당은 `프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용`
  <br>

```jsx
const str = "Hello";
// String 래퍼 객체로부터 length 프로퍼티만 추출
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: HTML, completed: true };
// todo 객체로부터 id 프로퍼티만 추출
const { id } = todo;
console.log(id); // 1

// 함수 매개변수에 객체 디스트럭처링 할당 사용
function printTodo({ content, completed }) {
	console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}

console.log({ id: 1, content: "HTML", completed: true }); // 할일 HTML은 완료 상태입니다.
```

- 객체 디스트럭처링 할당은 인수로 객체를 전달받는 함수 매개변수에도 사용 가능
  <br>

```jsx
// 중첩 객체인 경우
const user = {
	name: "Lee",
	address: {
		zipCod: "123",
		city: "Seoul",
	},
};

// address 프로퍼티 키로 객체를 추출하고, 이 객체의 city 프로퍼티 키로 값을 추출
const {
	address: { city },
} = user;
console.log(city); // 'Seoul'
```
