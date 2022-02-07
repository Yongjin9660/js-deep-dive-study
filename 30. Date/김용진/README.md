# 30. Date

> 💡 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수

**UTC**

- 국제표준시
- GMT(그리니치 평균시)로 불리기도 함

**KST**

- 한국 표준시
- UTC에 9시간을 더한 시간

## 30.1 Date 생성자 함수

- 내부적으로 날짜와 시간을 나타내는 정수값을 가짐
- 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간가지의 밀리초를 나타냄

**new Date()**

```js
// new와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환
new Date(); // Mon Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)

// new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열 반환
Date(); // "Mon Jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)"
```

**new Date(milliseconds)**

```js
// 숫자 타입의 밀리초를 인수로 전달하면
// 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한
// 날짜와 시간을 나타내는 Date 객체를 반환

new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

**new Date(dateString)**

```js
// 날짜와 시간을 나타내는 문자열을 인수로 전달하면
// 지정된 날짜와 시간을 나타내는 Date 객체를 반환

new Date('May 26, 2020 10:00:00');
// Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date('2020/03/26/10:00:00');
// Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

**new Date(year, month, [, day, hour, minute, second, millisecond])**

| 인수        | 내용                                 |
| ----------- | ------------------------------------ |
| year        | 연을 나타내는 1900년 이후의 정수     |
| month       | 월을 나타내는 0 ~ 11까지의 정수      |
| day         | 일을 나타내는 1 ~ 31까지의 정수      |
| hour        | 시를 나타내는 0 ~ 23까지의 정수      |
| minute      | 분을 나타내는 0 ~ 59까지의 정수      |
| second      | 초를 나타내는 0 ~ 59까지의 정수      |
| millisecond | 밀리초를 나타내는 0 ~ 999까지의 정수 |

```js
// 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달
// 지정된 날짜와 시간을 나타내는 Date 객체를 반환

// 월을 나타내는 2는 3월을 의미
// 2020/3/1/00:00:00:00
new Date(2020, 2);
// -> Sun Mar 01 2020 00:00:00 GMT+0900 (대한민국 표준시)

new Date(2020, 2, 26, 10, 00, 00, 0);
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date('2020/3/26/10:00:00:00');
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

## 30.2 Date 메서드

**Date.now**

```js
/**
 * 1970년 1월 1일 00:00:00(UTC)을 기점으로
 * 현재까지 경과한 밀리초를 숫자로 반환
 */

const now = Date.now(); // 1644212724396
new Date(now); // -> Mon Feb 07 2022 14:45:24 GMT+0900 (한국 표준시)
```

**Date.parse**

```js
/**
 * 1970년 1월 1일 00:00:00(UTC)을 기점으로
 * 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
 */

// UTC
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000

// KST
Date.parse('Jan 2, 1970 09:00:00'); // 86400000

// KST
Date.parse('1970/01/02/09:00:00'); // 86400000
```

**Date.UTC**

```js
/**
 * 1970년 1월 1일 00:00:00(UTC)을 기점으로
 * 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
 */

Date.UTC(1970, 0, 2); // 86400000
```

**Date.prototype.getFullYear**

```js
/**
 * Date 객체의 연도를 나타내는 정수를 반환
 */

new Date('2020/07/24').getFullYear(); // 2020
```

**Date.prototype.setFullYear**

```js
/**
 * Date 객체에 연도를 나타내는 정수를 설정
 */

const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear(); // 2000

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear(); // 1900
```

**Date.prototype.getMonth**

```js
/**
 * Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환
 */

new Date('2020/07/24').getMonth(); // 6
```

**Date.prototype.setMonth**

```js
/**
 * Date 객체에 월을 나타내는 0 ~ 11의 정수를 설정
 */

const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // 0

// 월/일 지정
today.setMonth(11, 1); // 12월 1일
today.getMonth(); // 11
```

**Date.prototype.getDate**

```js
/**
 * Date 객체의 날짜(1 ~ 31)를 나타내는 정수를 반환
 */

new Date('2020/07/24').getDate(); // 24
```

**Date.prototype.setDate**

```js
/**
 * Date 객체에 날짜(1 ~ 31)를 나타내는 정수를 설정
 */

const today = new Date();

// 날짜 지정
today.setDate(1);
today.getDate(); // 1
```

**Date.prototype.getDay**

```js
/**
 * Date 객체의 요일(0 ~ 6)을 나타내는 정수를 반환
 * 일요일 : 0
 */

new Date('2020/07/24').getDay(); // 5
```

**Date.prototype.getHours**

```js
/**
 * Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환
 */

new Date('2020/07/24/12:00').getHours(); // 12
```

**Date.prototype.setHours**

```js
/**
 * Date 객체의 시간(0 ~ 23)을 나타내는 정수를 설정
 */

const today = new Date();

// 시간 지정
today.setHours(7);
today.getHours(7); // 7

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

**Date.prototype.getMinutes**

```js
/**
 * Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환
 */

new Date('2020/07/24/12:30').getMinutes(); // 30
```

**Date.prototype.setMinutes**

```js
/**
 * Date 객체에 분(0 ~ 59)을 나타내는 정수를 설정
 */

const today = new Date();

// 분 지정
today.setMinutes(50);
today.getMinutes(); // 50

// 분/초/밀리초 지정
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // 5
```

**Date.prototype.getSeconds**

```js
/**
 * Date 객체의 초(0 ~ 59)를 나타내는 정수를 반환
 */

new Date('2020/07/24/12:30:10').getSeconds(); // 10
```

**Date.prototype.setSeconds**

```js
/**
 * Date 객체에 초(0 ~ 59)를 나타내는 정수를 설정
 */

const today = new Date();

// 초 지정
today.setSeconds(30);
today.getSeconds(); // 30
```

**Date.prototype.getMilliseconds**

```js
/**
 * Date 객체의 밀리초(0 ~ 999)를 나타내는 정수를 반환
 */

new Date('2020/07/24/12:30:10:150').getMilliseconds(); // 150
```

**Date.prototype.setMilliseconds**

```js
/**
 * Date 객체에 밀리초(0 ~ 999)를 나타내는 정수를 설정
 */

const today = new Date();

// 밀리초 지정
today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

**Date.prototype.getTime**

```js
/**
 * 1970년 1월 1일 00:00:00(UTC)를 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환
 */

new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

**Date.prototype.setTime**

```js
/**
 * Date 객체에 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초를 설정
 */

const today = new Date();

// 1970년 1월 1일 00:00:00(UTC)를 기점으로 경과된 밀리초 설정
today.setTime(86400000); // 86400000은 1day를 나타냄
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

**Date.prototype.getTimezoneOffset**

```js
/**
 * UTC와 Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환
 * UTC = KST - 9h
 */

const today = new Date(); // today의 지정 locale은 KST

// UTC와 today의 지정 locale KST와의 차이는 -9시간
today.getTimezoneOffset() / 60; // -9
```

**Date.prototype.toDateString**

```js
/**
 * 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

**Date.prototype.toTimeString**

```js
/**
 * 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toTimeString(); // 12:30:00 GMT+0900 (대한민국 표준시)
```

**Date.prototype.toISOString**

```js
/**
 * ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toISOString(); // 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ''); // 20200724
```

**Date.prototype.toLocaleString**

```js
/**
 * 인수로 전달한 locale을 기준으로
 * Date 객체의 날짜와 시간을 표현한 문자열을 반환
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('ko-KR'); // -> 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString('ja-JP'); // -> 2020/7/24, 12:30:00
```

**Date.prototype.toLocaleTimeString**

```js
/**
 * 인수로 전달한 locale을 기준으로
 * Date 객체의 시간을 표현한 문자열을 반환
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleTimeString(); // 오후 12:30:00
today.toLocaleTimeString('ko-KR'); // 오후 12:30:00
today.toLocaleTimeString('en-US'); // 12:30:00 PM
today.toLocaleTimeString('ja-JP'); // 12:30:00
```

## 30.3 Date를 활용한 시계 예제

```js
(function printNow() {
	const today = new Date();

	const dayNames = [
		'(일요일)',
		'(월요일)',
		'(화요일)',
		'(수요일)',
		'(목요일)',
		'(금요일)',
		'(토요일)',
	];

	const day = dayNames[today.getDay()];

	const year = today.getFullYear();
	const month = today.getFullMonth() + 1;
	const date = today.getDate();
	let hour = today.getHours();
	let minute = today.getMinutes();
	let second = today.getSeconds();
	const ampm = hour >= 12 ? 'PM' : 'AM';

	// 12시간제로 변경
	hour %= 12;
	hour = hour || 12; // hour가 0이면 12 할당

	// 10 미만인 분과 초를 2자리로 변경
	minute = minute < 10 ? '0' + minute : minute;
	second = second < 10 ? '0' + second : second;

	const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second}${ampm}`;

	console.log(now);

	// 1초마다 printNow 함수를 재귀 호출함
	setTimeout(printNow, 1000);
})();
```
