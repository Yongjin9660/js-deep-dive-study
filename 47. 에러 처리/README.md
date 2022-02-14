# 47. 에러 처리

## 47.1 에러 처리의 필요성

> 💡 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행 가능

에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료됨

```js
console.log('start');

foo(); // ReferenceError
// 프로그램 강제 종료

// 아래 코드는 실행되지 않음
console.log('end');
```

try .. catch 문을 사용해 발생한 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행 가능

```js
console.log('start');
try {
	foo();
} catch (error) {
	console.error('[에러 발생]', error);
	// [에러 발생] ReferenceError
}

// 발생한 에러에 적절한 대응을 하면 프로그램이 강제 종료되지 않음
console.log('End');
```

```js
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환
const $button = document.querySelector('button'); // null

$button.classList.add('disabled');
// TypeError: Cannot read property 'classList' of null
```

```js
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환
const $button = document.querySelector('button'); // null
$button?.classList.add('disabled');
```

## 47.2 try...catch... finally 문

**에러 처리를 구현하는 방법**

```
  1) 예외적인 상황이 발생하면 반환하는 값 (null 또는 -1) 을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법

  2) 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법
```

#### try...catch... finally 문

```js
try {
	// 실행할 코드(에러가 발생할 가능성이 있는 코드)
} catch (err) {
	// try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행
	// err에는 try 코드 블록에서 발생한 Error 객체가 전달
} finally {
	// 에러 발생과 상관없이 반드시 한번 실행
}
```

- **3개의 코드 블록으로 구성**

  - `try` 문 : 가장 먼저 실행되는 코드 블록으로, try 코드 블록에 포함된 문 중에서 에러가 발생하면 catch 문의 err 변수에 전달됨.
    <br>

  - `catch` 문 : try 코드 블록에서 에러가 발생하면 catch 문의 err 변수가 생성되고, catch 코드 블록에서만 유효함.
    <br>
  - `finally` 문 : 에러 발생과 상관없이 반드시 1번 실행됨. (생략 가능)

## 47.3 Error 객체

```jsx
const error = new Error('invalid');
```

- `Error 생성자 함수` 는 에러 객체를 생성하며, 에러를 상세히 설명하는 에러 메세지를 인수로 전달함
  <br>
- Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티 와 stack 프로퍼티를 가짐.
  - `message 프로퍼티` : Error 생성자 함수에 인수로 전달한 에러 메시지
  - `stack 프로퍼티` : 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열이며, 디버깅 목적으로 사용

#### 7가지의 에러 객체를 생성할 수 있는 `Error 생성자 함수`

| 생성자 함수    | 인스턴스                                                                       |
| -------------- | ------------------------------------------------------------------------------ |
| Error          | 일반적 에러 객체                                                               |
| SyntaxError    | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체                |
| ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                         |
| TypedError     | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체         |
| RangeError     | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체                            |
| URIError       | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
| EvalError      | eval 함수에서 발생하는 에러 객체                                               |

## 47.4 throw 문

> 💡 에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 함

```js
try {
	// 에러 객체를 던지면 catch 코드 블록이 실행되기 시작
	throw new Error('something wrong');
} catch (err) {
	console.log(err);
}
```

- throw 문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 지정
  - 에러를 던지면 catch 문의 에러 변수가 생성되고 던져진 에러 객체가 할당
  - → `catch 코드 블록 실행`

## 47.5 에러의 전파

> 💡 에러는 호출자 방향으로 전파됨

- 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파

```js
const foo = () => {
	throw Error('foo에서 발생한 에러');
};

const bar = () => {
	foo();
};

const baz = () => {
	bar();
};

try {
	baz();
} catch (err) {
	console.err(err);
}
```

- throw된 에러를 캐치하지 않으면 호출자 방향으로 전파됨
- 이때 throw된 에러를 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름을 복구할 수 있음
- throw된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료됨

![image](https://user-images.githubusercontent.com/55246584/153345578-60119574-406a-4f94-bf24-de35c2573946.png)

- 비동기 함수인 setTimetour이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없음
- 에러를 전파할 호출자가 존재하지 않음

### 참고

- https://velog.io/@chltjdrhd777/%EC%97%90%EB%9F%AC-%EC%A0%84%ED%8C%8C
- https://velog.io/@niyu/47%EC%9E%A5-%EC%97%90%EB%9F%AC-%EC%B2%98%EB%A6%AC
