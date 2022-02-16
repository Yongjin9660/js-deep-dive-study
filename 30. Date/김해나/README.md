# 30. Date

## 30.1 Date 생성자 함수

#### 1) new Date()

#### 2) new Date(milliseconds)

#### 3) new Date(dateString)

#### 4) new Date(year, month[, dat, hour, minute, second, millisecond])

## 30.2 Date 메서드

#### 1) Date.now

#### 2) Date.parse

#### 3) Date.UTC

#### 4) Date.prototype.getFullYear

#### 5) Date.prototype.setFullYear

#### 6) Date.prototype.getMonth

#### 7) Date.prototype.setMonth

#### 8) Date.prototype.getDate

#### 9) Date.prototype.setDate

#### 10) Date.prototype.getDay

#### 11) Date.prototype.getHours

#### 12) Date.prototype.setHours

#### 13) Date.prototype.getMinutes

#### 14) Date.prototype.setMinutes

#### 15) Date.prototype.getSeconds

#### 16) Date.prototype.setSeconds

#### 17) Date.prototype.getMilliseconds

#### 18) Date.prototype.setMilliseconds

#### 19) Date.prototype.getTime

#### 20) Date.prototype.setTime

#### 21) Date.prototype.getTimezoneOffset

#### 22) Date.prototype.toDateString

#### 23) Date.prototype.toTimeString

#### 24) Date.prototype.toISOString

#### 25) Date.prototype.toLocalString

#### 26) Date.prototype.toLocaleTimeString

## 30.3 Date 를 활용한 시계 예제

```jsx
(function printNow() {
	const today = new Date();

	const dayNames = [
		"(일요일)",
		"(월요일)",
		"(화요일)",
		"(수요일)",
		"(목요일)",
		"(금요일)",
		"(토요일)",
	];

	// getDay 메서드는 해당 요일 (0~6) 을 나타내는 정수를 반환한다.
	const day = dayNames[today.getDay()];

	const year = today.getFullYear();
	const month = today.getMonth() + 1;
	const date = today.getDate();
	let hour = today.getHours();
	let minute = today.getMinutes();
	let second = today.getSeconds();
	let ampm = hour >= 12 ? "PM" : "AM";

	// 12시간제로 변경
	hour %= 12;
	hour = hour || 12; // hour 가 0 이면 12 재할당

	// 10 미만인 분과 초를 2자리로 변경
	minute = minute < 10 ? "0" + minute : minute;
	second = second < 10 ? "0" + second : second;

	const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

	console.log(now);

	// 1초마다 printNow 함수를 재귀 호출한다.
	setTimeout(printNow, 1000);
})();
```
