# 29. Math

## 29.1 Math 프로퍼티

**Math.PI**

```js
// 원주율 값을 반환
Math.PI; // 3.141592...
```

## 29.2 Math 메서드

**Math.abs**

```js
// 인수로 전달된 숫자의 절댓값을 반환

Math.abs(-1); // 1
Math.abs('-1'); // 1
Math.abs(''); // 0
Math.abs([]); // 0
Math.abs(null); // 0
Math.abs(undefined); // NaN
Math.abs({}); // NaN
Math.abs('string'); // NaN
Math.abs(); // NaN
```

**Math.round**

```js
// 인수로 전달된 숫자의 소수점 이하르 반올림한 정수를 반환

Math.round(1.4); // 1
Math.round(1.6); // 2
Math.round(-1.4); // -1
Math.round(-1.6); // -2
Math.round(1); // 1
```

**Math.ceil**

```js
// 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환

Math.ceil(1.4); // 2
Math.ceil(1.6); // 2
Math.ceil(-1.4); // -1
Math.ceil(-1.6); // -1
Math.ceil(1); // 1
```

**Math.floor**

```js
// 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환

Math.floor(1.9); // 1
Math.floor(9.1); // 9
Math.floor(-1.9); // -2
Math.floor(1); // 1
```

**Math.sqrt**

```js
// 인수로 전달된 숫자의 제곱근을 반환
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(2); // 1.414...
```

**Math.random**

```js
// 0 ~ 1 의 임의의 숫자 반환
Math.random(); // 0이상 1미만의 랜덤 실수 반환

// 1에서 10 범위의 랜덤 정수 취득
const random = Math.floor(Math.random() * 10 + 1); // 1 ~ 10 범위의 정수
```

**Math.pow**

```js
// 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환
Math.pow(2, 8); // 256
Math.pow(2, -1); // 0.5
Math.pow(2); // NaN

// ES7 지수 연산자
2 ** (2 ** 2); // 16
```

**Math.max**

```js
// 전달받은 인수 중 가장 큰 수를 반환
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3

Math.max(); // Infinity

Math.max(...[1, 2, 3]); // 3
```

**Math.min**

```js
// 전달받은 인수 중 가장 작은 수를 반환
Math.min(1); // 1
Math.min(1, 2); // 1
Math.min(1, 2, 3); // 1

Math.min(); // Infinity

Math.min(...[1, 2, 3]); // 1
```
