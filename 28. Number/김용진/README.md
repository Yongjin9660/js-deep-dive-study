# 28. Number

## 28.1 Number 생성자 함수

> 💡 Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 \[[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성

```js
let numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

## 28.2 Number 프로퍼티

**Number.EPSILON**

> 💡 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같음

```js
0.1 + 0.2; // 0.3000000000000004
0.1 + 0.2 === 0.3; // false

function isEqual(a, b) {
	return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3);
```

**Number.MAX_VALUE**

> 💡 JS에서 표현할 수 있는 가장 큰 양수 값

```js
Number.MAX_VALUE;
Infinity > Number.MAX_VALUE; // true
```

**Number.MIN_VALUE**

> 💡 JS에서 표현할 수 있는 가장 작은 양수 값

```js
Number.MIN_VALUE;
Number.MIN_VALUE > 0; // true
```

**Number.MAX_SAFE_INTEGER**

> 💡 JS에서 안전하게 표현할 수 있는 가장 큰 정수값

```js
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

**Number.MIN_SAFE_INTEGER**

> 💡 JS에서 안전하게 표현할 수 있는 가장 작은 정수값

```js
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

**Number.POSITIVE_INFINITY**

> 💡 양의 무한대

```js
Number.POSITIVE_INFINITY; // Infinity
```

**Number.NEGATIVE_INFINITY**

> 💡 음의 무한대

```js
Number.NEGATIVE_INFINITY; // -Infinity
```

**Number.NaN**

> 💡 숫자가 아님을 나타내는 숫자값

```js
Number.NaN; // NaN
```

## 28.3 Number 메서드

**Number.isFinite**

```js
// 인수가 정상적인 유한수이면 true를 반환
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

// 인수가 무한수이면 false 반환
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

// 인수가 NaN이면 항상 false 반환
Number.isFinite(NaN); // false
```

Number.isFinite 메서드는 빌트인 전역 함수 isFinite와 차이가 있음

```js
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않음
Number.isFinite(null); // false

// 인수를 숫자로 암묵적 타입 변환
isFinite(null); // true
```

**Number.isInteger**

```js
// 인수가 정수이면 true를 반환
Number.isInteger(0); // true

Number.isInteger(1.5); // false
Number.isInteger('123'); // false

// 암묵적 타입 변환 하지 않음
Number.isInteger(false); // false
```

**Number.isNaN**

```js
// 인수가 NaN이면 true를 반환
Number.isNaN(NaN); // true

// 암묵적 타입 변환하지 않음
Number.isNaN(undefined); // false

// 암묵적 타입 변환
// undefined는 NaN으로 암묵적 타입 변환됨
isNaN(undefined); // true
```

**Number.isSafeInteger**

```js
// 안전한 정수인지 검사하여 결과를 불리언 값으로 반환
Number.isSafeInteger(0); // true

Number.isSafeInteger(0.5); // false
Number.isSafeInteger(Infinity); // false
```

**Number.prototype.toExponential**

```js
// 숫자를 지수 표기법으로 변환

(77.1234).toExponential(); // 7.71234e+1
(77.1234).toExponential(4); // 7.7123e+1
(77.1234).toExponential(2); // 7.71e+1
```

**Number.prototype.toFixed**

```js
// 숫자를 반올림하여 문자열로 반환

// 소수점 이하 반올림. 인수 생략시 기본값 0
(12345.6789).toFixed(); // 12346

// 소수점 이하 1 자릿수 유효. 나머지 반올림
(12345.6789).toFixed(1); // 12345.7

// 소수점 이하 2 자릿수 유효. 나머지 반올림
(12345.6789).toFixed(2); // 12345.68

// 소수점 이하 3 자릿수 유효. 나머지 반올림
(12345.6789).toFixed(3); // 12345679
```

**Number.prototype.toPrecision**

```js
// 전체 자릿수 유효, 인수를 생략하면 기본값 0 지정
(12345.6789).toPrecision(); // "12345.6789"

// 전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // "1e+4"

// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // "1.2e+4"

// 전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // "12345.6"
```

**Number.prototype.toString**

```js
// 숫자를 문자열로 변환하여 반환
// 진법을 나타내는 2~36 사이의 정수를 전달 가능

// 인수 생략 시 10진수 문자열을 만듦
(10).toString(); // "10"

// 2 진수 문자열로
(16).toString(2); // "10000"

// 8진수 문자열로
(16).toString(8); // "20"

// 16진수 문자열로
(16).toString(16); // "10"
```
