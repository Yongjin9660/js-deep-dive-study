# 28. Number

> 💡 표준 빌트인 객체 Number 는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드 제공

<br>

## 28.1 Number 생성자 함수

- Number 객체는 **생성자 함수 객체**
  - new 연산자와 함께 호출하여 Number 인스턴스 생성 가능
    <br>

```jsx
// ※ Number 생성자 함수와 new 연산자를 함께 호출한 경우

// Number 생성자 함수에 인수를 전달하지 않은 경우
const numObj = new Number();
console.log(numObj); // Number {[PrimitiveValue]}: 0}

// Number 생성자 함수에 인수를 전달한 경우
const numObj = new Number(10);
console.log(numObj); // Number {[PrimitiveValue]}: 10}

// Number 생성자 함수의 인수로 숫자가 아닌 값을 전달한 경우
let numObj = new Number("10");
console.log(numObj); // Number {[PrimitiveValue]}: NaN}

let numObj = new Number("Hello");
console.log(numObj); // Number {[PrimitiveValue]}: 10}
```

- `Number 생성자 함수에 인수를 전달하지 않은 경우`

  - \[[NumberData]] 내부 슬롯에 0 을 할당한 Number 래퍼 객체 생성
    <br>

- `Number 생성자 함수에 인수를 전달한 경우`

  - \[[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체 생성
    <br>

- `Number 생성자 함수의 인수로 숫자가 아닌 값을 전달한 경우`
  - 인수를 숫자로 강제 변환한 후, \[[NumberData]] 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체 생성
  - 인수를 숫자로 변환할 수 없다면 NaN 을 \[[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체 생성
    <br>

```jsx
// ※ Number 생성자 함수와 new 연산자를 함께 호출하지 않은 경우

// 문자열 타입 => 숫자 타입
Number("0"); // 0
Number("10.53"); // 10.53

// 불리언 타입 => 숫자 타입
Number(true); // 1
Number(false); // 0
```

- `new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환`
  - 이를 이용하여 `명시적으로 타입 변환`을 하기도 함.
    <br>

## 28.2 Number 프로퍼티

#### Number.EPSILON

> 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이
> → 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용

<br>

#### Number.MAX_VALUE

> 자바스크립트에서 표현할 수 있는 가장 큰 양수 값
> → Number.MAX_VALUE 보다 큰 숫자는 Infinity

<br>

#### Number.MIN_VALUE

> 자바스크립트에서 표현할 수 있는 가장 작은 양수 값
> → Number.MIN_VALUE 보다 작은 숫자는 0

<br>

#### Number.MAX_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값

<br>

#### Number.MIN_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값

<br>

#### Number.POSITIVE_INFINITY

> 양의 무한대를 나타내는 숫자값 (Infinity)

<br>

#### Number.NEGATIVE_INFINITY

> 음의 무한대를 나타내는 숫자값 (-Infinity)

<br>

#### Number.NaN

> 숫자가 아님을 나타내는 숫자값
> → window.NaN 과 동일

<br>

## 28.3 Number 메서드

#### Number.isFinite

> 인수로 전달된 값이 정상적인 유한수, 즉 Infinity 또는 -Infinity 가 아닌지 검사하여 그 결과를 불리언 값으로 반환  
> → 빌트인 전역 함수 isFinite 과 달리, 전달받은 인수를 숫자로 암묵적 타입 변환 X
> → 인수가 NaN 이거나 숫자가 아닌 인수가 주어지면 반환값은 언제나 false

<br>

#### Number.isInteger

> 인수로 전달된 값이 정수인지 검사하여 그 결과를 불리언 값으로 반환
> → 검사하기 전에 인수를 숫자로 암묵적 타입 변환 X

<br>

#### Number.isNaN

> 인수로 전달된 숫자값이 NaN 인지 검사하여 그 결과를 불리언 값으로 반환
> → 빌트인 전역 함수 isNaN 과 달리, 전달받은 인수를 숫자로 암묵적 타입 변환 X
> → 숫자가 아닌 인수가 주어지면 반환값은 언제나 false

<br>

#### Number.isSafeInteger

> 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환
> → 검사하기 전에 인수를 숫자로 암묵적 타입 변환 X

<br>

#### Number.prototype.toExponential

> 숫자를 지수 표기법으로 변환하여 문자열로 반환
> → 인수로 소수점 이하로 표현할 자릿수 전달 가능

<br>

\*\* **`지수 표기법이란?`**

- 매우 크거나 작은 숫자를 표기할 때 주로 사용하며 e 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식

<br>

#### Number.toFixed

> 숫자를 반올림하여 문자열로 반환
> → 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능
> → 인수를 생략하면 기본값 0 지정

<br>

#### Number.toPrecision

> 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
> → 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과 반환
> → 전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달 가능
> → 인수를 생략하면 기본값 0 지정

<br>

#### Number.prototype.toString

> 숫자를 문자열로 변환하여 반환
> → 진법을 나타내는 2~36 사이의 정수값을 인수로 전달 가능
> → 인수를 생략하면 기본값 10진법으로 지정
