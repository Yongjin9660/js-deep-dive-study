# 21. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- ### `표준 빌트인 객체`

  - ECMAScript 사양에 정의된 객체로 **자바스크립트 실행 환경과 관계없이 언제나 사용 가능**

  - 전역 객체의 프로퍼티로서 제공되며, **별도의 선언 없이 전역 변수처럼 언제나 참조 가능**

- ### `호스트 객체`

  - **ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체**

  - 브라우저 환경에서는 클라이언트 사이드 Web API 를 호스트 객체로 제공

  - Node.js 환경에서는 Node.js 고유의 API 를 호스트 객체로 제공

- ### `사용자 정의 객체`
  - **기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체**

<br>

## 21.2 표준 빌트인 객체

> 자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개의 표준 빌트인 객체 제공

<br>

- `Math, Reflect, JSON 을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체`

- 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드 제공

- 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공  
  <br>

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed 는 Number.prototype 의 프로토타입 메서드
// Number.prototype.toFixed 는 소수점 자리를 반올림하여 문자열로 반환
console.log(numObj.toFixed()); // 2

// isInteger 는 Number 의 정적 메서드
// Number.isInteger 는 인수가 정수인지 검사하여 그 결과를 불리언값으로 반환
console.log(Number.isInteger(0.5)); // false
```

- 표준 빌트인 객체인 String, Number, Boolean, Function, Array, Date 는 생성자 함수로 호출하여 인스턴스 생성 가능

- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체

## 21.3 원시값과 래퍼 객체

<br>

### \*\* **래퍼 객체**

> `문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체`  
> -> null 과 undefined 는 래퍼 객체 생성 X

<br>

```jsx
1) 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성함.

2) 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌림.
```

<br>

```jsx
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환됨
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌림
console.log(typeof str); // string
```

- 문자열에 대해 마침표 표기법으로 접근하면

  - 래퍼 객체인 String 생성자 함수의 인스턴스가 생성되고,

  - 문자열은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당됨.

  - 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 되돌리고,

  - 래퍼 객체는 가비지 컬렉션의 대상이 됨.  
    <br>

<img src="https://user-images.githubusercontent.com/31315644/67624621-d62e6800-f86d-11e9-895f-f1d27857397d.jpeg" height="700px">

<br>

## 21.4 전역 객체

> `코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체`  
> -> 어떤 객체에도 속하지 않은 최상위 객체

<br>

\# 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며, 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유함.

\# 전역 객체가 최상위 객체라는 것은 **프로토타입 상속 관계상에서 최상위 객체라는 의미 X**

<br>
 
### ** **전역 객체의 특징**

- 전역 객체는 `개발자가 의도적으로 생성 X`  
  (전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않음)

- 전역 객체의 프로퍼티를 참조할 때 `window (또는 global) 생략 가능`

- `모든 표준 빌트인 객체를 프로퍼티로 가짐`

- 자바스크립트 실행 환경에 따라 `추가적인 프로퍼티와 메서드`를 가짐

  - 브라우저 환경 : 클라이언트 사이드 Web API 를 호스트 객체로 제공

  - Node.js 환경 : Node.js 고유의 API 를 호스트 객체로 제공

- `var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티`  
  <br>

\*\* **globalThis**

> ES11 에서 도입된 globalThis 는 브라우저 환경과 Node.js 환경에서 <u> 전역 객체를 가리키던 다양한 식별자를 통일한 식별자</u>

<br>

```jsx
// 브라우저 환경
globalThis === this; // true
globalThis === window; // true
globalThis === self; // true
globalThis === frames; // true

// Node.js 환경 (12.0.0 이상)
globalThis === this; // true
globalThis === global; // true
```

<br>

```jsx
// var 키워드로 선언한 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar 는 전역 변수가 아니라 전역 객체의 프로퍼티
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// 전역 함수
function baz() {
	return 3;
}
console.log(window.baz()); // 3

let a = 123;
console.log(window.a); // undefined
```

- `let 이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님`

- 브라우저 환경의 모든 자바스크립트 코드는 `하나의 전역 객체 window 를 공유함`
  - 여러 개의 script 태그를 통해 코드를 분리해도 하나의 전역 객체 window 공유  
    <br>

### 1) 빌트인 전역 프로퍼티

> 전역 객체의 프로퍼티 의미

<br>

### \*\* **`Infinity`**

```jsx
// 전역 프로퍼티는 window 생략한 채로 참조 가능
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3 / 0); // Infinity

// 음의 무한대
console.log(-3 / 0); // -Infinity

// Infinity 는 숫자값
console.log(typeof Infinity); // number
```

<br>

### \*\* **`NaN`**

```jsx
console.log(window.NaN); // NaN
console.log(Number('xyz')); // NaN
console.log(typeof NaN); // number
```

### \*\* **`undefined`**

```jsx
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof foo); // undefined
```

<br>

### 2) 빌트인 전역 함수

> 전역 객체의 메서드 의미

<br>

### \*\* **`eval`**

> 전달받은 문자열 코드가 표현식이라면 런타임에 문자열 코드를 평가하여 값을 생성  
> 전달받은 문자열 코드가 표현식이 아닌 문이라면 런타임에 문자열 코드 실행

<br>

- eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약

- eval 함수를 통해 실행되는 코드는 일반적인 코드 실행에 비해 처리 속도가 느림.

- eval 함수는 가급적 사용하지 않는 것이 좋음.  
  <br>

### \*\* **`isFinite`**

> 전달받은 인수가 정상적인 유한수인지 검사하여 그 결과를 불리언값으로 반환  
> -> 인수 타입이 NaN 으로 평가되는 값이라면 false 반환

<br>

### \*\* **`isNaN`**

> 전달받은 인수가 NaN 인지 검사하여 그 결과를 불리언값으로 반환  
> -> 인수 타입이 숫자가 아닌 경우 숫자로 타입 변환 후 검사 수행

<br>

### \*\* **`parseFloat`**

> 전달받은 문자열 인수를 실수로 해석하여 반환

<br>

### \*\* **`parseInt`**

> 전달받은 문자열 인수를 정수로 해석하여 반환

<br>

### \*\* **`encodeURI/decodeURI`**

> encodeURI : 완전한 URI 를 문자열로 전달받아 이스케이프 처리를 위해 인코딩  
> decodeURI : 인코딩된 URI 를 인수로 전달받아 이스케이프 처리 이전으로 디코딩

<br>

\*\* 인코딩
: URI 문자들을 이스케이프 처리하는 것  
<br>

\*\* URI : 인터넷에 있는 자원을 나타내는 유일한 주소 의미  
&ensp; -> URL, URN 등이 URI 의 하위 개념

<br>

### \*\* **`encodeURIComponent/decodeURIComponent`**

> encodeURIComponent : URI 구성 요소를 인수로 전달받아 인코딩  
> decodeURIComponent : 매개변수로 전달된 URI 구성 요소를 디코딩

<br>

### 3) 암묵적 전역

```jsx
// 전역 변수 x 는 변수 호이스팅 발생
console.log(x); // undefined

// 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y 는 변수 호이스팅 X
console.log(y); // ReferenceError

var x = 10; // 전역 변수

function foo() {
	// 선언하지 않은 식별자에 값 할당
	y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조 가능
console.log(x + y); // 30

// 전역 변수는 delete 연산자로 삭제 불가능
delete x;
console.log(window.x); // 10

// 프로퍼티는 delete 연산자로 삭제 가능
delete y;
console.log(window.y); // undefined
```

- `선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작`

  - 젼역 객체의 프로퍼티에서는 **변수 호이스팅 발생 X**

  - 변수가 아닌 프로퍼티이기 때문에 **delete 연산자로 삭제 가능**

<br><br>
\*\* 이미지 출처 : https://hyeok999.github.io/2019/10/28/javascript-preview-272829/
