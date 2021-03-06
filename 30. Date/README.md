# 30. Date

> π‘ λ μ§μ μκ°μ μν λ©μλλ₯Ό μ κ³΅νλ λΉνΈμΈ κ°μ²΄μ΄λ©΄μ μμ±μ ν¨μ

**UTC**

- κ΅­μ νμ€μ
- GMT(κ·Έλ¦¬λμΉ νκ· μ)λ‘ λΆλ¦¬κΈ°λ ν¨

**KST**

- νκ΅­ νμ€μ
- UTCμ 9μκ°μ λν μκ°

## 30.1 Date μμ±μ ν¨μ

- λ΄λΆμ μΌλ‘ λ μ§μ μκ°μ λνλ΄λ μ μκ°μ κ°μ§
- 1970λ 1μ 1μΌ 00:00:00(UTC)μ κΈ°μ μΌλ‘ Date κ°μ²΄κ° λνλ΄λ λ μ§μ μκ°κ°μ§μ λ°λ¦¬μ΄λ₯Ό λνλ

**new Date()**

```js
// newμ ν¨κ» νΈμΆνλ©΄ νμ¬ λ μ§μ μκ°μ κ°μ§λ Date κ°μ²΄λ₯Ό λ°ν
new Date(); // Mon Jul 06 2020 01:03:18 GMT+0900 (λνλ―Όκ΅­ νμ€μ)

// new μ°μ°μ μμ΄ νΈμΆνλ©΄ Date κ°μ²΄λ₯Ό λ°ννμ§ μκ³  λ μ§μ μκ° μ λ³΄λ₯Ό λνλ΄λ λ¬Έμμ΄ λ°ν
Date(); // "Mon Jul 06 2020 01:03:18 GMT+0900 (λνλ―Όκ΅­ νμ€μ)"
```

**new Date(milliseconds)**

```js
// μ«μ νμμ λ°λ¦¬μ΄λ₯Ό μΈμλ‘ μ λ¬νλ©΄
// 1970λ 1μ 1μΌ 00:00:00(UTC)μ κΈ°μ μΌλ‘ μΈμλ‘ μ λ¬λ λ°λ¦¬μ΄λ§νΌ κ²½κ³Όν
// λ μ§μ μκ°μ λνλ΄λ Date κ°μ²΄λ₯Ό λ°ν

new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
```

**new Date(dateString)**

```js
// λ μ§μ μκ°μ λνλ΄λ λ¬Έμμ΄μ μΈμλ‘ μ λ¬νλ©΄
// μ§μ λ λ μ§μ μκ°μ λνλ΄λ Date κ°μ²΄λ₯Ό λ°ν

new Date('May 26, 2020 10:00:00');
// Tue May 26 2020 10:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)

new Date('2020/03/26/10:00:00');
// Thu Mar 26 2020 10:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
```

**new Date(year, month, [, day, hour, minute, second, millisecond])**

| μΈμ        | λ΄μ©                                 |
| ----------- | ------------------------------------ |
| year        | μ°μ λνλ΄λ 1900λ μ΄νμ μ μ     |
| month       | μμ λνλ΄λ 0 ~ 11κΉμ§μ μ μ      |
| day         | μΌμ λνλ΄λ 1 ~ 31κΉμ§μ μ μ      |
| hour        | μλ₯Ό λνλ΄λ 0 ~ 23κΉμ§μ μ μ      |
| minute      | λΆμ λνλ΄λ 0 ~ 59κΉμ§μ μ μ      |
| second      | μ΄λ₯Ό λνλ΄λ 0 ~ 59κΉμ§μ μ μ      |
| millisecond | λ°λ¦¬μ΄λ₯Ό λνλ΄λ 0 ~ 999κΉμ§μ μ μ |

```js
// μ°, μ, μΌ, μ, λΆ, μ΄, λ°λ¦¬μ΄λ₯Ό μλ―Ένλ μ«μλ₯Ό μΈμλ‘ μ λ¬
// μ§μ λ λ μ§μ μκ°μ λνλ΄λ Date κ°μ²΄λ₯Ό λ°ν

// μμ λνλ΄λ 2λ 3μμ μλ―Έ
// 2020/3/1/00:00:00:00
new Date(2020, 2);
// -> Sun Mar 01 2020 00:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)

new Date(2020, 2, 26, 10, 00, 00, 0);
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)

new Date('2020/3/26/10:00:00:00');
// -> Thu Mar 26 2020 10:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
```

## 30.2 Date λ©μλ

**Date.now**

```js
/**
 * 1970λ 1μ 1μΌ 00:00:00(UTC)μ κΈ°μ μΌλ‘
 * νμ¬κΉμ§ κ²½κ³Όν λ°λ¦¬μ΄λ₯Ό μ«μλ‘ λ°ν
 */

const now = Date.now(); // 1644212724396
new Date(now); // -> Mon Feb 07 2022 14:45:24 GMT+0900 (νκ΅­ νμ€μ)
```

**Date.parse**

```js
/**
 * 1970λ 1μ 1μΌ 00:00:00(UTC)μ κΈ°μ μΌλ‘
 * μΈμλ‘ μ λ¬λ μ§μ  μκ°κΉμ§μ λ°λ¦¬μ΄λ₯Ό μ«μλ‘ λ°ν
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
 * 1970λ 1μ 1μΌ 00:00:00(UTC)μ κΈ°μ μΌλ‘
 * μΈμλ‘ μ λ¬λ μ§μ  μκ°κΉμ§μ λ°λ¦¬μ΄λ₯Ό μ«μλ‘ λ°ν
 */

Date.UTC(1970, 0, 2); // 86400000
```

**Date.prototype.getFullYear**

```js
/**
 * Date κ°μ²΄μ μ°λλ₯Ό λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24').getFullYear(); // 2020
```

**Date.prototype.setFullYear**

```js
/**
 * Date κ°μ²΄μ μ°λλ₯Ό λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// λλ μ§μ 
today.setFullYear(2000);
today.getFullYear(); // 2000

// λλ/μ/μΌ μ§μ 
today.setFullYear(1900, 0, 1);
today.getFullYear(); // 1900
```

**Date.prototype.getMonth**

```js
/**
 * Date κ°μ²΄μ μμ λνλ΄λ 0 ~ 11μ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24').getMonth(); // 6
```

**Date.prototype.setMonth**

```js
/**
 * Date κ°μ²΄μ μμ λνλ΄λ 0 ~ 11μ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// μ μ§μ 
today.setMonth(0); // 1μ
today.getMonth(); // 0

// μ/μΌ μ§μ 
today.setMonth(11, 1); // 12μ 1μΌ
today.getMonth(); // 11
```

**Date.prototype.getDate**

```js
/**
 * Date κ°μ²΄μ λ μ§(1 ~ 31)λ₯Ό λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24').getDate(); // 24
```

**Date.prototype.setDate**

```js
/**
 * Date κ°μ²΄μ λ μ§(1 ~ 31)λ₯Ό λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// λ μ§ μ§μ 
today.setDate(1);
today.getDate(); // 1
```

**Date.prototype.getDay**

```js
/**
 * Date κ°μ²΄μ μμΌ(0 ~ 6)μ λνλ΄λ μ μλ₯Ό λ°ν
 * μΌμμΌ : 0
 */

new Date('2020/07/24').getDay(); // 5
```

**Date.prototype.getHours**

```js
/**
 * Date κ°μ²΄μ μκ°(0 ~ 23)μ λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24/12:00').getHours(); // 12
```

**Date.prototype.setHours**

```js
/**
 * Date κ°μ²΄μ μκ°(0 ~ 23)μ λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// μκ° μ§μ 
today.setHours(7);
today.getHours(7); // 7

// μκ°/λΆ/μ΄/λ°λ¦¬μ΄ μ§μ 
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

**Date.prototype.getMinutes**

```js
/**
 * Date κ°μ²΄μ λΆ(0 ~ 59)μ λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24/12:30').getMinutes(); // 30
```

**Date.prototype.setMinutes**

```js
/**
 * Date κ°μ²΄μ λΆ(0 ~ 59)μ λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// λΆ μ§μ 
today.setMinutes(50);
today.getMinutes(); // 50

// λΆ/μ΄/λ°λ¦¬μ΄ μ§μ 
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // 5
```

**Date.prototype.getSeconds**

```js
/**
 * Date κ°μ²΄μ μ΄(0 ~ 59)λ₯Ό λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24/12:30:10').getSeconds(); // 10
```

**Date.prototype.setSeconds**

```js
/**
 * Date κ°μ²΄μ μ΄(0 ~ 59)λ₯Ό λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// μ΄ μ§μ 
today.setSeconds(30);
today.getSeconds(); // 30
```

**Date.prototype.getMilliseconds**

```js
/**
 * Date κ°μ²΄μ λ°λ¦¬μ΄(0 ~ 999)λ₯Ό λνλ΄λ μ μλ₯Ό λ°ν
 */

new Date('2020/07/24/12:30:10:150').getMilliseconds(); // 150
```

**Date.prototype.setMilliseconds**

```js
/**
 * Date κ°μ²΄μ λ°λ¦¬μ΄(0 ~ 999)λ₯Ό λνλ΄λ μ μλ₯Ό μ€μ 
 */

const today = new Date();

// λ°λ¦¬μ΄ μ§μ 
today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

**Date.prototype.getTime**

```js
/**
 * 1970λ 1μ 1μΌ 00:00:00(UTC)λ₯Ό κΈ°μ μΌλ‘ Date κ°μ²΄μ μκ°κΉμ§ κ²½κ³Όλ λ°λ¦¬μ΄λ₯Ό λ°ν
 */

new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

**Date.prototype.setTime**

```js
/**
 * Date κ°μ²΄μ 1970λ 1μ 1μΌ 00:00:00(UTC)λ₯Ό κΈ°μ μΌλ‘ κ²½κ³Όλ λ°λ¦¬μ΄λ₯Ό μ€μ 
 */

const today = new Date();

// 1970λ 1μ 1μΌ 00:00:00(UTC)λ₯Ό κΈ°μ μΌλ‘ κ²½κ³Όλ λ°λ¦¬μ΄ μ€μ 
today.setTime(86400000); // 86400000μ 1dayλ₯Ό λνλ
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
```

**Date.prototype.getTimezoneOffset**

```js
/**
 * UTCμ Date κ°μ²΄μ μ§μ λ locale μκ°κ³Όμ μ°¨μ΄λ₯Ό λΆ λ¨μλ‘ λ°ν
 * UTC = KST - 9h
 */

const today = new Date(); // todayμ μ§μ  localeμ KST

// UTCμ todayμ μ§μ  locale KSTμμ μ°¨μ΄λ -9μκ°
today.getTimezoneOffset() / 60; // -9
```

**Date.prototype.toDateString**

```js
/**
 * μ¬λμ΄ μ½μ μ μλ νμμ λ¬Έμμ΄λ‘ Date κ°μ²΄μ λ μ§λ₯Ό λ°ν
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
today.toDateString(); // Fri Jul 24 2020
```

**Date.prototype.toTimeString**

```js
/**
 * μ¬λμ΄ μ½μ μ μλ νμμΌλ‘ Date κ°μ²΄μ μκ°μ ννν λ¬Έμμ΄μ λ°ν
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
today.toTimeString(); // 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
```

**Date.prototype.toISOString**

```js
/**
 * ISO 8601 νμμΌλ‘ Date κ°μ²΄μ λ μ§μ μκ°μ ννν λ¬Έμμ΄μ λ°ν
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
today.toISOString(); // 2020-07-24T03:30:00.000Z

today.toISOString().slice(0, 10); // 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ''); // 20200724
```

**Date.prototype.toLocaleString**

```js
/**
 * μΈμλ‘ μ λ¬ν localeμ κΈ°μ€μΌλ‘
 * Date κ°μ²΄μ λ μ§μ μκ°μ ννν λ¬Έμμ΄μ λ°ν
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
today.toLocaleString(); // -> 2020. 7. 24. μ€ν 12:30:00
today.toLocaleString('ko-KR'); // -> 2020. 7. 24. μ€ν 12:30:00
today.toLocaleString('en-US'); // -> 7/24/2020, 12:30:00 PM
today.toLocaleString('ja-JP'); // -> 2020/7/24, 12:30:00
```

**Date.prototype.toLocaleTimeString**

```js
/**
 * μΈμλ‘ μ λ¬ν localeμ κΈ°μ€μΌλ‘
 * Date κ°μ²΄μ μκ°μ ννν λ¬Έμμ΄μ λ°ν
 */

const today = new Date('2020/7/24/12:30');

today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (λνλ―Όκ΅­ νμ€μ)
today.toLocaleTimeString(); // μ€ν 12:30:00
today.toLocaleTimeString('ko-KR'); // μ€ν 12:30:00
today.toLocaleTimeString('en-US'); // 12:30:00 PM
today.toLocaleTimeString('ja-JP'); // 12:30:00
```

## 30.3 Dateλ₯Ό νμ©ν μκ³ μμ 

```js
(function printNow() {
	const today = new Date();

	const dayNames = [
		'(μΌμμΌ)',
		'(μμμΌ)',
		'(νμμΌ)',
		'(μμμΌ)',
		'(λͺ©μμΌ)',
		'(κΈμμΌ)',
		'(ν μμΌ)',
	];

	const day = dayNames[today.getDay()];

	const year = today.getFullYear();
	const month = today.getFullMonth() + 1;
	const date = today.getDate();
	let hour = today.getHours();
	let minute = today.getMinutes();
	let second = today.getSeconds();
	const ampm = hour >= 12 ? 'PM' : 'AM';

	// 12μκ°μ λ‘ λ³κ²½
	hour %= 12;
	hour = hour || 12; // hourκ° 0μ΄λ©΄ 12 ν λΉ

	// 10 λ―Έλ§μΈ λΆκ³Ό μ΄λ₯Ό 2μλ¦¬λ‘ λ³κ²½
	minute = minute < 10 ? '0' + minute : minute;
	second = second < 10 ? '0' + second : second;

	const now = `${year}λ ${month}μ ${date}μΌ ${day} ${hour}:${minute}:${second}${ampm}`;

	console.log(now);

	// 1μ΄λ§λ€ printNow ν¨μλ₯Ό μ¬κ· νΈμΆν¨
	setTimeout(printNow, 1000);
})();
```
