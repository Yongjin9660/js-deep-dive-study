# 15. let, const 키워드와 블록 레벨 스코프

## 15.1 var 키워드로 선언한 변수의 문제점

**1. 변수 중복 선언 허용**

```js
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 스코프 내에서 중복 선언을 허용
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
var x = 100;
// 초기화문이 없는 변수 선언문은 무시
var y;

console.log(x); // 100
console.log(y); // 1
```

<br />

**2. 함수 레벨 스코프**

> 💡 var 키워드로 선언한 변수는 함수의 코드 블록만을 지역 스코프로 인정

```js
var x = 1;
if (true) {
  // x는 전역 변수 => 중복 선언됨
  var x = 10;
}
console.log(x); // 10
```

<br />

**3. 변수 호이스팅**

> 💡 var 키워드로 선언한 변수는 변수 호이스팅에 의해 할당 이전에 참조 시 undefined를 반환

```js
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 할당 (1. 선언 단계)
// 변수 foo는 undefined 로 초기화 (2. 초기화)

console.log(foo); // undefined

// 값 할당 (3. 할당 단계)
foo = 123;

console.log(foo);
// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행
var foo;
```

<br />

## 15.2 let 키워드

**1. 변수 중복 선언 금지**

> 💡 let 키워드로 이름이 같은 변수를 중복 선언하면 SyntaxError가 발생

```js
let foo = 1;
let foo = 2; // SyntaxError: Identifier 'foo' fas already been declared
```

<br />

**2. 블록 레벨 스코프**

> 💡 let 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 가짐

```js
let foo = 1; // 전역 변수
{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}
console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

<br />

**3. 변수 호이스팅**

> 💡 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작

```js
console.log(foo)); // ReferenceError: foo is not defined
let foo;
```

- var 키워드로 선언한 변수는 런타임 이전에 `선언 단계`와 `초기화 단계`가 한번에 진행
- let 키워드로 선언한 변수는 **선언단계와 초기화 단계가 분리되어 진행**

`TDZ(Temporal Dead Zone)` : 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간

```js
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError
  let foo = 2;
}
```

- let 키워드로 선언한 변수 호이스팅이 발생하지 않는다면 위 예제에는 전역 변수 foo의 값을 출력해야함
- 하지만 let 키워드로 선언한 변수도 호이스팅이 발생하기 때문에 ReferenceError가 발생

<br />

**4. 전역 객체와 let**

> 💡 let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님

- var 키워드로 선언한 전역 변수와 전역 함수는 **전역 객체 window**의 프로퍼티가 됨

```js
// 전역 변수
var x = 1;
let y = 2;

console.log(window.x); // 1
console.log(window.y); // undefined
```

<br />

## 15.3 const 키워드

**1. 선언과 초기화**

> 💡 const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함

```js
const foo = 1;
const bar; // SyntaxError: Missing initializer in const declaration
```

- let 과 동일하게 블록 레벨 스코프를 가짐
- 변수 호이스팅이 발생하지 않는 것처럼 동작

<br />

**2. 재할당 금지**

> 💡 const 키워드로 선언한 변수는 재할당이 금지됨

```js
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable
```

<br />

**3. 상수**

> 💡 상수는 재할당이 금지된 변수를 말함

```js
// 변수 이름을 대문자로 선언해 상수임을 명확히 함
// 언더스코어(_)로 구분해서 스네이크 케이스로 표현하는 것이 일반적
const TAX_RATE = 0.1;
```

<br />

**4. const 키워드와 객체**

> 💡 const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음

```js
const person = { name: 'Kim' };

// 객체는 변경 가능한 값
// 재할당 없이 변경이 가능
person.name = 'Lee';
```

- **const 키워드는 재할당을 금지할 뿐 불변을 의미하지는 않음**

<br />

## 15.4 var vs. let vs. const

> 일단 const 사용 => 재할당이 필요하다면 let 키워드 사용

- ES6를 사용한다면 var 키워드는 사용하지 X
- 재할당이 필요한 경우에 한정해 let 키워드를 사용 => 변수의 스코프를 최대한 좁게
- 변경이 발생하지 않고 읽기 전용으로 사용하는 원시 값과 객체에는 const 키워드 사용
