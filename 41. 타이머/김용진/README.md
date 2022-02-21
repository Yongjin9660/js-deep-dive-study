# 41. 타이머

## 41.1 호출 스케줄링

> 💡 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하는 것

- 자바스크립트는 Timer를 생성할 수 있는 **setTimeout, setInterval** 함수를 제공
  - 일정 시간이 경과된 이후 콜백 함수가 호출됨
- Timer를 제어할 수 있는 **clearTimeout과 clearInterval**을 제공

`setTimeout`

- 단 한 번 동작

`setInterval`

- 타이머가 만료될 때마다 반복 호출

## 41.2 타이머 함수

### setTimeout / clearTimeout

> 💡 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성

```js
const timeoutId = setTimeout(func | code[, delay, param1, param2, ...]);
```

`func`

- 타이머가 만료된 뒤 호출될 콜백 함수

`delay`

- setTimeout 함수는 delay 시간으로 단 한 번 동작하는 타이머를 생성
- 타이머 만료 시간(ms 단위)
- 인수를 생략하면 0으로 지정

`param`

- 호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 인수로 전달 가능

```js
// 1초(1000ms) 후 타이머가 만료되면 콜백 함수가 호출
setTimeout(() => console.log('Hi!'), 1000);

// 인수 전달
setTimeout((name) => console.log(`Hi! ${name}`), 1000, 'Kim');
```

- setTimeout 함수는 생성된 타이머를 식별할 수 있는 **고유한 타이머 id를 반환**
- **clearTimeout** 함수의 인수로 전달하여 타이머를 취소할 수 있음

```js
const timerId = setTimeout(() => console.log('Hi'), 1000);

// setTimeout 함수가 반환한 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머 취소
clearTimeout(timerId);
```

### setInterval / clearInterval

> 💡 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성

```js
const timeoutId = setInterval(func | code[, delay, param1, param2, ...]);
```

```js
let count = 1;

// 1초 (1000ms) 후 타이머가 만료될 때마다 콜백 함수 실행
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유 타이머 Id 반환
const timeoutId = setInterval(() => {
	console.log(count);
	if (count++ === 5) {
		clearTimeout(timeoutId);
	}
}, 1000);
```

## 41.3 디바운스와 스로틀

### 디바운스

> 💡 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 후에 이벤트 핸들러가 한 번만 호출되도록 함

```html
<!DOCTYPE html>
<html>
	<body>
		<input type="text" />
		<div class="msg"></div>
		<script>
			const $input = document.querySelector('input');
			const $msg = document.querySelector('.msg');

			const debounce = (callback, delay) => {
				let timerId;
				// delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머 재설정
				// 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않음
				return (event) => {
					if (timerId) {
						clearTimeout(timerId);
					}
					timerId = setTimeout(callback, delay, event);
				};
			};

			// debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록
			$input.oninput = debounce((e) => {
				$msg.textContent = e.target.value;
			}, 300);
		</script>
	</body>
</html>
```

- 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간 동안 이벤트가 더 이상 발생하지 않으면 이벤트 핸들러가 한 번만 호출되도록 함
- 실무에서는 Underscore의 debounce 함수나 Lodash의 debounce 함수를 사용하는 것을 권장

  ![image](https://user-images.githubusercontent.com/55246584/149948748-f9cc8b33-9503-45f4-975f-9edb6f409e27.png)

### 스로틀

> 💡 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 함

```js
const $container = document.querySelector('.container');
const $normalCount = document.querySelector('.normal-count');
const $throttleCount = document.querySelector('.throttle-count');

const $container = (callback, delay) => {
	let timerId;
	// throttle 함수는 timerId를 기억하는 클로저를 반환
	return (event) => {
		// delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
		// delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정
		// 따라서 delay 간격으로 callback이 호출
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
$container.addEventListener('scroll', () => {
	$normalCount.textContent = ++normalCount;
});

let throttleCount = 0;
// throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록
$container.addEventListener(
	'scroll',
	throttle(() => {
		$throttleCount.textContent = ++throttleCount;
	}, 100)
);
```

- scroll 이벤트는 사용자가 스크롤할 때 짧은 시간 간격으로 연속해서 발생
- 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만듦
- 실무에서는 Underscore의 throttle 함수나 Lodash의 throttle 함수를 사용하는 것을 권장

### 참고

- https://webclub.tistory.com/607
