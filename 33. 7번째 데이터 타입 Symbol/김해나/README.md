# 33. 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

- ES6 에서 도입된 7번째 데이터 타입으로, `변경 불가능한 원시 타입의 값`
- `다른 값과 중복되지 않는 유일무이한 값`
- 이름의 충돌 위험이 없는 `유일한 프로퍼티 키를 만들기 위해 사용`
  <br>

## 33.2 심벌 값의 생성

#### 1. Symbol 함수

```jsx
// Symbol 함수를 호출하여 유일무이한 심벌 값 생성
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인 X
console.log(mySymbol); // Symbol()

// 생성자 함수 X
// → new 연산자와 함께 호출하지 않는다.
new Symbol(); // TypeError
```

- `심벌값은 Symbol 함수를 호출하여 생성`

  - 다른 원시값 (문자열, 숫자, 불리언 등) 은리터럴 표기법을 통해 값 생성
  - 심벌 값은 Symbol 함수를 통해서만 호출하여 생성해야 함.
    <br>

- `심벌 값은 외부로 노출되지 않아 확인할 수 없으며, 다른 값과 절대 중복되지 않는 유일무이한 값`
  - 변경 불가능한 원시 값이기 때문에 인스턴스 생성 X
    <br>

```jsx
// Symbol 함수에는 선택적으로 문자열을 인수로 전달 가능
// 이 문자열은 생성된 심벌 값에 대한 설명에 불과하다.
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

- `Symbol 함수에 전달된 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용된다.`
  - 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값
    <br>

```jsx
const mySymbol = Symbol("mySymbol");

// 심벌도 래퍼 객체 생성
// description 프로퍼티와 toString 메서드는 Symbol.prototype 의 프로퍼티
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

```jsx
const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ""); // TypeError
console.log(+mySymbol); // TypeError
```

```jsx
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인이 가능하다.
if (mySymbol) console.log("mySymbol is not empty.");
```

- 심벌 값도 객체처럼 접근하면 `암묵적으로 래퍼 객체 생성 가능`
- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환 X`
  - 단, 불리언 타입으로는 암묵적으로 타입 변환 O
    <br>

#### 2. Symbol.for/Symbol.keyFor 메서드

<br>

```jsx
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for("mySymbol");

// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

- `Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 `전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값 검색`

  - 검색 성공 → 새로운 심벌 값을 생성하지 않고 **검색된 심벌 값 반환**
  - 검색 실패 → 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 **전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값 반환**
    <br>

- `Symbol 함수 호출 시 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리되지 않음.`
  <br>

- `Symbol.for` 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 `유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유 가능`
  <br>

\*\* **`전역 심벌 레지스트리란?**

- 자바스크립트 엔진이 관리하는 심벌 값 저장소
  <br>

```jsx
// 전역 심벌 레지스트리에 mySymbol 이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for("mySymbol");

// 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("foo");

// 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출
Symbol.keyFor(s2); // undefined
```

- `Symbol.keyFor` 메서드를 사용하면 `전역 심벌 레지스트리에 저장된 심벌 값의 키 추출 가능`
  <br>

## 33.3 심벌과 상수

```jsx
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수 정의
// 이때 값 1, 2, 3, 4 에는 특별한 의미가 없고 상수 이름에 의미가 있다.
const Direction = {
	UP: 1,
	DOWN: 2,
	LEFT: 3,
	RIGHT: 4,
};

// 변수에 상수 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
	console.log("You are going UP");
}
```

- 위 예제는 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우
  - 상수 값 1, 2, 3, 4가 변경될 수 있으며, 다른 변수 값과의 중복 가능성 O
    <br>

```jsx
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수 정의
// 중복될 가능성이 없는 심벌 값으로 상수 값 생성
const Direction = {
	UP: Symbol("up"),
	DOWN: Symbol("down"),
	LEFT: Symbol("left"),
	RIGHT: Symbol("right"),
};

// 변수에 상수 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
	console.log("You are going UP");
}
```

- 상수값의 변경 및 중복을 막기 위해 유일무이한 심벌 값 사용 가능
  <br>

```jsx
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값
const Direction = Object.freeze({
	UP: Symbol("up"),
	DOWN: Symbol("down"),
	LEFT: Symbol("left"),
	RIGHT: Symbol("right"),
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
	console.log("You are going UP");
}
```

- `enum (열거형)`
  - 숫자 상수의 집합으로, 자바스크립트에서는 enum 지원 X
  - 객체 변경을 방지하기 위해 `enum 대신 객체를 동결하는 Object.freeze 메서드와 심벌 값 사용`
    <br>

## 33.4 심벌과 프로퍼티 키

```jsx
const obj = {
	// 심벌 값으로 프로퍼티 키 생성
	[Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // 1
```

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로도 생성 가능
- `심벌 값을 프로퍼티 키로 사용하려면 심벌 값에 대괄호 사용`
  - 이때 프로퍼티에 접근할 때도 마찬가지로 대괄호 사용
    <br>

## 33.5 심벌과 프로퍼티 은닉

```jsx
const obj = {
	// 심벌 값으로 프로퍼티 키 생성
	[Symbol("mySymbol")]: 1,
};

// for... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로는 심벌 값을 찾을 수 없음.`
for (const key in obj) {
	console.log(key); // 아무것도 출력 x
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

```jsx
const obj = {
	// 심벌 값으로 프로퍼티 키 생성
	[Symbol("mySymbol")]: 1,
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbool)]

// getOwnPropertySymbols 메서드로 심벌 값 또한 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없음.`
  - 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티 은닉 가능
    <br>
- 단, ES6 에서 도입된 `Object.getOwnPropertySymbols 메서드를 통해서는 찾을 수 있음.`
  <br>

## 33.6 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장 X
  - `개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문`
  - 이름이 중복된다면 사용자 정의 메서드가 덮어쓰면서 문제 생길 수 있음.
    <br>

```jsx
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2].[Symbol.for('sum')]();   // 3
```

- `중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 충돌 위험 없이 안전하게 확장 가능`
  <br>

## 33.7 Well-known Symbol

- `자바스크립트가 기본 제공하는 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있음.`
  - ECMAScript 사양에서는 빌트인 심벌 값을 **Well-known Symbol** 이라고 부름.
    <br>

![image](https://user-images.githubusercontent.com/77706631/153107928-66e596fc-937d-414c-b354-87e7cc84f268.png)
<br>

- `for... of 문으로 순회 가능한 빌트인 이터러블`은 Well-known Symbol 인 `Symbol.iterator` 를 키로 갖는 메서드를 가짐.
  - Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 에 규정되어 있음.
  - 빌트인 이터러블은 이 규정, 즉 이터레이션 프로토콜을 준수함.
