# 13. 스코프

## 13.1 스코프

> 💡 모든 식별자(변수 이름, 함수 이름, 클래스 이름 등)가 참조될 수 있는 유효한 범위
>
> - 식별자를 검색하는 규칙
> - 네임스페이스

**`렉시컬 환경` :** 코드가 어디서 실행되며 주변에 어떤 코드가 있는지

⇒ **실행 컨텍스트** _모든 코드는 실행 컨텍스트에서 평가, 실행_

```jsx
function add(x, y) {
	// 매개변수는 함수 몸체 내부에서만 참조 가능
	// => 매개변수의 스코프 (유효범위) : 함수 몸체 내부
	console.log(x, y); // 2 5
	return x + y;
}

add(2, 5);

// 매개변수는 함수 몸체 내부에서만 참조 가능하기 때문에
// 함수 외부에서 참조하려고 하면 오류 발생
console.log(x, y); // -> ReferenceError
```

⇒ 매개변수의 스코프 : 함수 몸체 내부

```jsx
var var1 = 1; // 가장 바깥 영역에 선언한 변수

if (true) {
	var var2 = 2; // 코드 블록 내에서 선언한 변수
	if (true) {
		var var3 = 3; // 중첩된 코드 블록 내에서 선언한 변수
	}
}

function foo() {
	var var4 = 4; // 함수 내에서 선언한 변수

	function bar() {
		var var5 = 5; // 중첩된 함수 내에서 선언한 변수
	}
}

console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
console.log(var4); // ReferenceError
console.log(var5); // ReferenceError
```

```jsx
// 코드 바깥 영역과 foo() 함수 내부에 같은 이름을 갖는 x 변수 선언

var x = 'global';

function foo() {
	var x = 'local';
	console.log(x); // local
}

foo();

console.log(x); // global
```

- 코드의 가장 바깥 영역에 선언된 x 변수는 어디서든 참조 가능 => `전역 스코프`

- foo 함수 내부에서 선언된 x 변수는 foo 함수 내부에서만 참조 가능 => `함수 스코프`

- <em> 즉, 두 개의 x 변수는 식별자 이름이 동일하지만 <u> 스코프가 다른 별개의 변수</u> </em>  
  <br>

`** 식별자 결정 **`

&ensp; : 자바스크립트 엔진이 스코프를 통해 어떤 변수를 참조할 것인지 결정하는 것.  
<br>

```jsx
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 허용 O
function foo() {
	var x = 1;
	var x = 2;
	console.log(x); // 2
}

foo();

// let 이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 허용 X
function bar() {
	var y = 1;
	var y = 2; // SyntaxError
}

bar();
```

- `var 키워드로 선언된 변수`

  - 같은 스코프 내에서 <u>중복 선언 허용 O</u>

- `let 또는 const 키워드로 선언된 변수`
  - 같은 스코프 내에서 <u>중복 선언 허용 X</u>

## 13.2 스코프의 종류

| 구분 | 설명                  | 스코프      | 변수      | 특징                                           |
| ---- | --------------------- | ----------- | --------- | ---------------------------------------------- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 | 어디서든 참조 가능                             |
| 지역 | 함수 몸체 내부        | 지역 스코프 | 지역 변수 | 자신의 스코프와 하위 지역 스코프에서 참조 가능 |

![img](https://media.vlpt.us/images/yunsungyang-omc/post/8508f171-6f55-4e43-ab72-2978fd3efa5b/image.png)

- inner 함수 내부에서 선언된 x 변수를 참조
  - 이름이 같은 전역 변수 x를 참조 X
  - inner 함수 내부에서 선언된 x 변수 참조 O

> 자바스크립트 엔진이 **스코프 체인**을 통해 참조할 변수를 검색했기 때문

## 13.3 스코프 체인

> 💡 모든 스코프는 하나의 계층적 구조로 연결

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZsB6H%2FbtqZqqeXNc7%2Fufs7kKZUEci9ynrAyWFkJk%2Fimg.png)

- `전역 스코프` : 모든 지역 스코프의 최상위 스코프
- 변수를 참조할 때, 자바스크립트 엔진은 `스코프 체인`은 통해 변수를 참조하는 코드의 스코프 ⇒ 상위 스코프 로 이동하며 선언된 변수를 검색

> <em> 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조 가능
> 하위 스코프에서 유효한 변수를 상위 스코프에서 참조 불가능 </em>

## 13.4 함수 레벨 스코프

> 💡 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성됨

`블록 레벨 스코프` : 함수 몸체 이외에도 모든 코드 블록 (if, for, while, try/catch 등)이 지역 스코프를 만듦

`함수 레벨 스코프` : var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정

```jsx
var i = 10;
for (var i = 0; i < 5; i++) {
	console.log(i); // 0 1 2 3 4
}
console.log(i); // 5

//var는 함수 레벨 스코프만을 지원하기 때문에 for문 안의 var로 선언된 i 는 전역 변수가 된다.
```

> **_ES6에서 도입된 let, const 키워드는 블록 레벨 스코프를 지원_**

## 13.5 렉시컬 스코프

> 💡 자바스크립트는 렉시컬 스코프를 따름

`렉시컬 스코프` : 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정

→ 함수가 호출되는 위치는 상위 스코프 결정에 영향을 주지 않음

↔ `동적 스코프` : 함수가 호출되는 시점에 동적으로 상위 스코프를 결정

```jsx
var x = 1;

function foo() {
	var x = 10;
	bar();
}

function bar() {
	console.log(x);
}

foo(); // 1
bar(); // 1
```

- **동적 스코프**를 적용한다면

  - bar 함수의 상위 스코프는 foo 함수의 지역 스코프와 전역 스코프

- **렉시컬 스코프 (정적 스코프)** 를 적용한다면

  - bar 함수의 상위 스코프는 전역 스코프

## 참조

- [https://debugmode.net/2013/09/17/variable-scope-chain-in-javascript/](https://debugmode.net/2013/09/17/variable-scope-chain-in-javascript/)
- https://velog.io/@yunsungyang-omc/JS-%EC%A0%84%EC%97%AD-%EB%B3%80%EC%88%98%EC%9D%98-%EB%AC%B8%EC%A0%9C%EC%A0%90
- https://codingfarm.tistory.com/455
