# 21. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

자바스크립트 객체는 다음과 같이 크게 3개의 객체로 분류 가능

`표준 빌트인 객체`

- **ECMAScript 사양에 정의된 객체**를 말하며, 애플리케이션 전역의 공통 기능을 제공
- 자바스크립트 실행 환경(브라우저 또는 Node.js 환경)과 관계없이 언제나 사용 가능
- 전역 객체의 프로퍼티로서 제공됨

`호스트 객체`

- ECMAScript 사양에 정의되어 있지 않지만 **자바스크립트 실행 환경(브라우저 환경 또는 Node.js 환경)에서 추가로 제공하는 객체**

`사용자 정의 객체`

- **사용자가 직접 정의한 객체**

## 21.2 표준 빌트인 객체

> 💡 자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개의 표준 빌트인 객체를 제공

- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체
- `생성자 함수 객체인 표준 빌트인 객체`는 프로토타입 메서드와 정적 메서드를 제공
- `생성자 함수 객체가 아닌 표준 빌트인 객체`는 정적 메서드만 제공

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Kim"); // String {'Kim'}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype 임
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5);
// Number.prototype 메서드 사용 가능
console.log(numObj.toFixed()); // 2
```

## 21.3 원시값과 래퍼 객체

> 💡 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표(또는 대괄호 표기법)으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환함

```js
const str = "hello";

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

`래퍼 객체(wrapper object)`

- 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체

```js
const str = "hi";

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌림
console.log(typeof str); // string
```

- 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 **String.prototype**의 메서드를 상속받아 사용 가능
- 래퍼 객체의 처리가 종료되면 래퍼 객체의 \[[StringData]] 내부 슬롯에 할당된 원시값으로 되돌림
- 래퍼 객체는 가비지 컬렉션의 대상이 됨

```js
const str = "hello";

// str은 암묵적으로 생성된 래퍼 객체를 가리킴
// str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당
str.name = "Kim";

// 식별자 str은 다시 문자열, 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시 값을 가짐
// 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 됨

// 식별자 str은 새롭게 암묵적으로 생성된 래퍼 객체를 가리킴
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않음
console.log(str.name); // undefined

// 식별자 str은 다시 문자열, 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 가짐
// 생성된 래퍼 객체는 가비지 컬렉션의 대상이 됨
console.log(typeof str); // string
```

## 21.4 전역 객체

> 💡 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않는 최상위 객체

`전역 객체`

- 표준 빌트인 객체(Object, String, Number, Function, Array 등)와 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 가짐
- 어떤 객체의 프로퍼티도 아님
- 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유
- 의도적으로 생성할 수 없음
- 참조 시 window(또는 global)을 생략할 수 있음

```js
window.parseInt("F", 16); // 15

parseInt("F", 16); // 15

window.parseInt === parseInt; // true

// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역이라 함
// 전역 변수가 아니라 전역 객체의 프로퍼티
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// 전역 함수
function baz() {
	return 3;
}
console.log(window.baz()); // 3
```

### 빌트인 전역 프로퍼티

> 💡 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미

- Infinity, NaN, undefined

```js
console.log(window.Infinity === Infinity); // true
console.log(window.NaN); // NaN
console.log(window.undefined); // undefined
```

### 빌트인 전역 함수

> 💡 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드

**eval**

```js
/**
 * 주어진 문자열 코드를 런타임에 평가 또는 실행
 * @param {string} code - 코드를 나타내는 문자열
 * @returns {*} 문자열 코드를 평가/실행한 결과값
 */
eval(code);
```

```js
// 표현식
eval("1 + 2;"); // 3

// 표현식이 아닌 문
eval("var x = 5;"); //undefined

console.log(x); // 5
```

- 인수로 전달받은 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 물을 실행한 후, 마지막 결과값을 반환
- 호출된 위치에 해당하는 스코프를 동적으로 수정

**isFinite**

```js
/**
 * 전달받은 인수가 유한수인지 확인하고 그 결과를 반환
 * @param {number} value - 검사 대상 값
 * @returns {boolean} 유한수 여부 확인 결과
 */
isFinite(value);
```

```js
// true
isFinite(0);
isFinite(2e64);
isFinite("10");
isFinite(null);

// false
// 인수가 무한수 또는 NaN
isFinite(Infinity); // false
isFinite(-Infinity); // false
isFinite(NaN); // false
isFinite("hello"); // false
```

**isNaN**

```js
/**
 * 전달받은 숫자가 NaN인지 확인하고 그 결과를 반환
 * @param {number} value - 검사 대상 값
 * @returns {boolean} NaN 여부 확인 결과
 */
isNaN(value);
```

```js
// 숫자
isNaN(NaN); // true
isNaN(10); // false

// 문자열
isNaN("asdf"); // true
isNaN("10"); // false
isNaN(""); // false: '' => 0
isNaN(" "); // false: ' ' => 0
```

**parseFloat**

```js
/**
 * 전달받은 문자열 인수를 실수로 해석하여 반환
 * @param {string} string - 변환 대상 값
 * @returns {number} 반환 결과
 */
parseFloat(value);
```

```js
parseFloat("3.14"); // 3.14
parseFloat("10.00"); // 10
// 공백으로 구분된 문자열은 첫번째 문자열만 반환
parseFloat("34 45"); // 34
parseFloat("40 years"); // 40

// 앞 뒤 공백은 무시
parseFloat(" 60"); // 60
```

**parseInt**

```js
/**
 * 전달받은 문자열 인수를 정수로 해석하여 반환
 * @param {string} string - 변환 대상 값
 * @param {number} [radix] - 진법을 나타내는 기수(2 ~ 36, 기본값 10)
 * @returns {number} 변환 결과
 */
parseInt(value);
```

```js
parseInt("10"); // 10
parseInt("10.123"); // 10

// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt("10", 2); // 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt("10", 8); // 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt("10", 16); // 16

const x = 15;
// 10진수 15를 2진수로 변환하여 그 결과를 문자열로 반환
x.toString(2); // '1111'

// 문자열 '1111'을 2진수로 해석하고 그 결과를 10진수 정수로 반환
parseInt(x.toString(2), 2);
```

**encodeURI / decodeURI**

```js
/**
 * 완전한 URI을 문자열로 전달받아 이스케이프 처리를 위해 인코딩함
 * @param {string} uri - 완전한 uri
 * @returns {string} 인코딩된 URI
 */
encodeURI(uri);
```

`인코딩`

- URI 문자들을 이스케이프 처리하는 것
  - 아스키 문자로 변환하는 것

```js
const uri = "http://example.com?name=이웅모&job=programmer&teacher";
// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D....A8&job=programmer&teacher
```

```js
/**
 * 인코딩된 URI를 전달받아 이스케이프 처리 이전으로 인코딩
 * @param {string} encodedURI - 인코딩된 URI
 * @returns {string} 디코딩된 URI
 */
decodeURI(encodedURI);
```

```js
const uri = "http://example.com?name=이웅모&job=programmer&teacher";

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D....A8&job=programmer&teacher

const dec = decodeURI(enc);
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```

**encodeURIComponent / decodeURIComponent**

```js
/**
 * URI의 구성요소를 전달받아 이스케이프 처리를 위해 인코딩
 * @param {string} uriComponent - URI의 구성요소
 * @returns {string} 인코딩된 URI의 구성요소
 */
encodeURIComponent(uriComponent);

/**
 * 인코딩된 URI의 구성요소를 전달받아 이스케이프 처리 이전으로 디코딩
 * @param {string} encodedURIComponent - 인코딩된 URI의 구성요소
 * @returns {string} 디코딩된 URI의 구성요소
 */
decodeURIComponent(encodedURIComponent);
```

- encodeURIComponent 함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주
- 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩

```js
// URI의 쿼리 스트링
const uriComp = "name=이웅모&job=programmer&teacher";

let enc = decodeURIComponent(uriComp);
console.log(enc);
// name%3DEC...A8%26job%Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// 이웅모&job=programmer&teacher
```

### 암묵적 전역

```js
var x = 10; // 전역 변수

function foo() {
	// 선언하지 않은 식별자에 값을 할당
	y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조 가능
console.log(x + y); // 30
```

- delete 연산자로 삭제 가능
