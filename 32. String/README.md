# 32. String

## 32.1 String 생성자 함수

> 💡 표준 빌트인 객체인 String 객체는 생성자 함수 객체

- new 연산자와 함께 호출하여 String 인스턴스 생성 가능

```js
const strObj = new String("Lee");
console.log(strObj);
// String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}
```

- String 래퍼 객체는 배열과 마찬가지로 유사 배열 객체이면서 이터러블
- length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가짐
- 단, 문자열은 원시 값이므로 변경할 수 없음

```js
console.log(strObj[0]); // L

// 문자열은 원시 값이므로 변경할 수 없음
strObj[0] = "S";
console.log(strObj); // 'Lee'
```

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환함

```js
// 숫자 => 문자열
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"

// 불리언 타입 => 문자열 타입
String(true); // "true"
String(false); // "false"
```

## 32.2 length 프로퍼티

```js
/**
 * 문자열의 문자 개수를 반환
 */

"Hello".length; // 5
"안녕하세요!".length; // 6
```

## 32.3 String 메서드

> 💡 String 객체의 메서드는 언제나 새로운 문자열을 반환

**String.prototype.indexOf**

```js
/**
 * 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번 째 인덱스를 반환
 * 검색에 실패하면 -1을 반환
 */

const str = "Hello World";

str.indexOf("l"); // 2
str.indexOf("or"); // 7
str.indexOf("x"); // -1

// 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환
str.indexOf("l", 3); // 3

// 특정 문자열에 특정 문자열이 존재하는지 확인
if (str.indexOf("Hello") !== -1) {
	// str에 'Hello'가 포함되어 있늦 경우에 처리할 내용
}

// ES6에서 도입된 String.prototype.includes 메서드를 사용하면 가독성이 더 좋음
if (str.includes("Hello")) {
	// 문자열 str에 'Hello'가 포함되어 있는 경우에 처리할 내용
}
```

**String.prototype.search**

```js
/**
 * 대상 문자열에서 인수로 전달받은 정규표현식과 매치하는 문자열을 검색하여 일치하는 문자열을 반환
 * 검색에 실패하면 -1을 반환
 */

const str = "Hello World";

str.serach(/o/); // 4
str.serach(/x/); // -1
```

**String.prototype.includes**

```js
/**
 * 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인
 * 결과를 true 또는 false로 반환
 */

const str = "Hello World";

str.includes("Hello"); // true
str.includes(""); // true
str.includes("x"); // false
str.includes(); // false

// 문자열 str의 인덱스 3부터 확인
str.includes("l", 3); // true
str.includes("H", 3); // false
```

**String.prototype.startWith**

```js
/**
 * 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인
 * 결과를 true 또는 false로 반환
 */

const str = "Hello world";

str.startsWith("He"); // true
str.startsWith("x"); // false

// 문자열 str의 인덱스 5부터 시작하는 문자열이 ' '로 시작하는지 확인
str.startsWith(" ", 5); // true
```

**String.prototype.endsWith**

```js
/**
 * 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인
 * 결과를 true 또는 false로 반환
 */
const str = "Hello world";

str.endsWith("ld"); // true
str.endsWith("x"); // false

// 문자열 str의 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
str.endsWith("lo", 5); // true
```

**String.prototype.charAt**

```js
/**
 * 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환
 */

const str = "Hello";

for (let i = 0; i < str.length; i++) {
	console.log(str.charAt(i)); // H e l l o
}

// 인덱스가 문자열 범위를 벗어난 경우 빈 문자열 반환
str.charAt(5); // ''
```

**String.prototype.substring**

```js
/**
 * 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터
 * 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환
 */

const str = "Hello world";

str.substring(1, 4); // ell

// 인덱스 1부터 마지막 문자까지
str.substring(1); // 'ello world'

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수가 교환됨
str.substring(4, 1); // 'ell'

// 인수 < 0 또는 NaN인 경우 0으로 취급
str.substring(-2); // 'Hello world'

// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급
str.substring(1, 100); // 'ello world'
str.substring(20); // ''
```

**String.prototype.slice**

```js
/**
 * substring 메서드와 동일하게 동작
 * slice 메서드에는 음수인 인수 전달 가능
 * 음수인 인수 전달 시 대상 문자열을 뒤에서부터 시작하여 문자열을 잘라내어 반환
 */

const str = "Hello world";

str.substring(0, 5); // 'Hello'
str.slice(0, 5); // Hello

str.substring(2); // 'llo world'
str.slice(2); // 'llo world'

// 인수 < 0 또는 NaN인 경우 0으로 취급
str.substring(-5); // 'hello world'
// 뒤에서 5자리를 잘라내어 반환
str.slice(-5); // 'world'
```

**String.prototype.toUpperCase**

```js
/**
 * 대상 문자열을 모두 대문자로 변경한 문자열을 반환
 */

const str = "Hello World!";

str.toUpperCase(); // 'HELLO WORLD!'
```

**String.prototype.toLowerCase**

```js
/**
 * 대상 문자열을 모두 소문자로 변경한 문자열을 반환
 */

const str = "Hello World!";

str.toLowerCase(); // 'hello world!'
```

**String.prototype.trim**

```js
/**
 * 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환
 */

const str = "   foo  ";
str.trim(); // 'foo'

str.trimStart(); // 'foo  '
str.trimEnd(); // '   foo'
```

**String.prototype.repeat**

```js
/**
 * 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환
 * 인수로 전달받은 정수가 0이면 빈 문자열을 반환, 음수이면 RangeError를 발생
 * 인수의 기본값은 0
 */

const str = "abc";

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2); // 'abcabc'
str.repeat(2.5); // 'abcabc' (2.5 -> 2)
str.repeat(-1); // RangeError
```

**String.prototype.replace**

```js
/**
 * 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여
 * 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환
 */

const str = "Hello world";

str.replace("world", "Lee"); // 'Hello Lee'

// 특수한 교체 패턴. ($& => 검색된 문자열)
str.replace("world", "<strong>$&</strong>");
```

- replace 메서드의 두 번째 인수로 치환 함수를 전달할 수 있음

```js
// Camel Case => Snake Case
const camelToSnake = (camelCase) =>
	cameCase.replace(/.[A-Z]/g, (match) => match[0] + "_" + match[1].toLowerCase());

const camelCase = "helloWorld";
camelToSnake(camelCase); // 'hello_world'

// Snake => Camel
const SnakeToCamel = (snakeCase) => snakeCase.replace(/_[a-z]/g, (match) => match[1].toUpperCase());

const snakeCase = "hello_world";
snakeToCamel(snakeCase); // 'helloWorld'
```

**String.prototype.split**

```js
/**
 * 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여
 * 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
 */

const str = "How are you doing?";

str.split(" "); // ["How", "are", "you", "doing?"]

// \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미
str.split(/\s/); // ["How", "are", "you", "doing?"]

str.split(" ", 3); // ["How", "are", "you"]
```
