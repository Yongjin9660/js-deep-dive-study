# 41. 타이머

## 41.1 호출 스케줄링

> 💡 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하는 것

<br>

- #### **타이머 생성 함수**

  - 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머 생성
  - 즉, 생성한 타이머가 만료되면 콜백 함수가 호출됨.
  - setTimeout 과 setInterval 은 **비동기 처리 방식**으로 동작

    - 자바스크립트 엔진은 싱글 스레드로 동작하기 때문 (2가지 이상의 태스크 동시 실행 X)
      <br>

  - `setTimeout`
    - setTimeout 함수의 콜백 함수는 **`타이머가 만료되면 단 한 번 호출 `**
  - `setInterval`
    - setInterval 함수의 콜백 함수는 **`타이머가 만료될 때마다 반복 동작`**
      <br>

- #### **타이머 제거 함수**
  - `clearTimeout`, `clearInterval`

<br>

## 41.2 타이머 함수

#### 1) setTimeout / clearTimeout

> 💡 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성

<br>

```jsx
const timeoutId = setTimeout(func | code[, delay, param1, param2, ...]);
```

<br>

| 매개변수           | 설명                                                                                         |
| ------------------ | -------------------------------------------------------------------------------------------- |
| func               | 타이머가 만료된 뒤 호출될 콜백 함수                                                          |
| delay              | 타이머 만료 시간 (인수 전달을 생략한 경우 기본값 0 지정)                                     |
| param1, param2,... | 호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후의 인수로 전달 가능 |

<br>

- setTimeout 함수는 두 번째 인수로 전달받은 시간 (ms, 1/000초) 으로 단 한 번 동작하는 타이머를 생성
- 이후 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출됨.
- `즉, setTimeout 함수의 콜백 함수는 두 번째 인수로 전달받은 시간 이후 단 한 번 실행되도록 호출 스케일링`
  <br>

```js
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
setTimeout(() => console.log("Hi!"), 1000);

// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// 이때 콜백 함수에 'Lee' 가 인수로 전달된다.
setTimeout((name) => console.log(`Hi! ${name}`), 1000, "Lee");

// 두 번째 인수 (delay) 를 생략하면 기본값 0 이 지정된다.
setTimeout(() => console.log(`Hello!`));
```

- setTimeout 함수는 생성된 타이머를 식별할 수 있는 **`고유한 타이머 id 반환`**
  - 브라우저 환경인 경우 타이머 id 는 숫자
  - Node.js 환경인 경우 타이머 id 는 객체
    <br>

```js
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출된다.
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id 를 반환한다.
const timerId = setTimeout(() => console.log("Hi"), 1000);

// setTimeout 함수가 반환한 타이머 id 를 clearTimeout 함수의 인수로 전달하여 타이머를 취소한다.
// 타이머가 취소되면 setTimeout 함수의 콜백 함수가 실행되지 않는다.
clearTimeout(timerId);
```

- setTimeout 함수가 반환한 **`타이머 id 를 clearTimeout 함수의 인수로 전달하여 타이머 취소 가능`**
  - 즉, clearTimeout 함수는 호출 스케일링을 취소

<br>

#### 2) setInterval / clearInterval

> 💡 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성

<br>

```jsx
const timerId = setInterval(func | code[, delay, param1, param2, ...])
```

- setInterval 함수는 두 번째 인수로 전달받은 시간 (ms, 1/000초) 으로 반복 동작하는 타이머를 생성
- 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출됨.
- `즉, setInterval 함수의 콜백 함수는 두 번째 인수로 전달받은 시간이 경과할 때마다 반복 실행되도록 호출 스케일링`
  <br>

```js
let count = 1;

// 1초 (1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유 타이머 id 를 반환한다.
const timeoutId = setInterval(() => {
	console.log(count); // 1 2 3 4 5

	// count 가 5 이면 setInterval 함수가 반환한 타이머 id 를 clearInterval 함수의 인수로 전달하여
	// 타이머를 취소한다. 타이머가 취소되면 setInterval 함수의 콜백 함수가 실행되지 않는다.
	if (count++ === 5) {
		clearTimeout(timeoutId);
	}
}, 1000);
```

- setInterval 함수는 생성된 타이머를 식별할 수 있는 **`고유한 타이머 id 반환`**

  - 브라우저 환경인 경우 타이머 id 는 숫자
  - Node.js 환경인 경우 타이머 id 는 객체
    <br>

- setInterval 함수가 반환한 **`타이머 id 를 clearInterval 함수의 인수로 전달하여 타이머 취소 가능`**
  - 즉, clearInterval 함수는 호출 스케일링을 취소

<br>

## 41.3 디바운스와 스로틀

- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 `과도한 이벤트 핸들러의 호출을 방지`하는 프로그래밍 기법

- 디바운스와 스로틀을 구현할 때는 **`타이머 함수를 사용`** 하며 이벤트를 처리할 때 매우 유용함.

#### 1) 디바운스

- 디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 함.
- `즉, 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러를 호출시킴.`

<br>

##### \*\* **`텍스트 입력 필드에서 input 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우`**

```html
<!DOCTYPE html>
<html>
	<body>
		<input type="text" />
		<div class="msg"></div>
		<script>
			const $input = document.querySelector("input");
			const $msg = document.querySelector(".msg");

			const debounce = (callback, delay) => {
				// debounce 함수는 timerId 를 기억하는 클로저 반환
				let timerId;

				return (event) => {
					// delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머 재설정
					// 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출 X

					if (timerId) {
						clearTimeout(timerId);
					}
					timerId = setTimeout(callback, delay, event);
				};
			};

			// debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
			// 300ms 보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
			// 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출된다.
			$input.oninput = debounce((e) => {
				$msg.textContent = e.target.value;
			}, 300);
		</script>
	</body>
</html>
```

<br>

- 사용자가 일정 시간동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주
  <br>

- **`debounce 함수가 반환한 함수는 debounce 함수의 delay 인수보다 짧은 간격으로 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정`**
  - 즉, delay 보다 짧은 간격으로 이벤트가 연속해서 발생하면 debounce 함수의 첫 번째 인수로 전달한 콜백 함수는 호출되지 않다가 delay 동안 input 이벤트가 더 이상 발생하지 않으면 한 번만 호출됨.
    <br>

![image](https://user-images.githubusercontent.com/55246584/149948748-f9cc8b33-9503-45f4-975f-9edb6f409e27.png)

- 디바운스는 resize 이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 유용하게 사용됨.
  <br>

- 실무에서는 Underscore의 debounce 함수나 Lodash의 debounce 함수를 사용하는 것을 권장

  <br>

#### 2) 스로틀

- 스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 함.
- `즉, 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만듦.`

<br>

##### \*\* **`scroll 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우`**

```js
const $container = document.querySelector(".container");
const $normalCount = document.querySelector(".normal-count");
const $throttleCount = document.querySelector(".throttle-count");

const $container = (callback, delay) => {
	// throttle 함수는 timerId를 기억하는 클로저 반환
	let timerId;

	return (event) => {
		// delay 가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
		// delay 가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정
		// 따라서 delay 간격으로 callback 호출 O

		if (timerId) {
			return;
		}
		timerId = setTimeout(
			() => {
				callback(event);
				timerId = null;
			},
			delay,
			event
		);
	};
};

let normalCount = 0;
$container.addEventListener("scroll", () => {
	$normalCount.textContent = ++normalCount;
});

let throttleCount = 0;

// throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
$container.addEventListener(
	"scroll",
	throttle(() => {
		$throttleCount.textContent = ++throttleCount;
	}, 100)
);
```

<br>

- **`throttle 함수가 반환한 함수는 throttle 함수의 delay 시간이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 delay 시간이 경과했을 때 이벤트가 발생하면 콜백 함수를 호출하고 새로운 타이머를 재설정`**
  - 즉, delay 시간 간격으로 콜백 함수가 호출됨.
    <br>
- 스로틀은 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용됨.
  <br>
- 실무에서는 Underscore의 throttle 함수나 Lodash의 throttle 함수를 사용하는 것을 권장

<br><br>

\*\* 이미지 출처
https://webclub.tistory.com/607
