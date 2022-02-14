# 32. String

## 32.1 String 생성자 함수

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체
  - new 연산자와 함께 호출 -> String 인스턴스 생성
    <br>

##### \*\* String 생성자 함수에 인수를 전달하지 않고 new 연산자와 호출한 경우

```jsx
const strObj = new String();
console.log(strObj); // String {length : 0, [[PrimitiveValue]] : ""}
```

- `[[StiringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체 생성`
- \[[PrimitiveValue]] 프로퍼티는 \[[StringData]] 내부 슬롯을 가리키는 접근 불가능한 프로퍼티
  <br>

##### \*\* String 생성자 함의 인수로 문자열을 전달하면서 new 연산자와 호출한 경우

```jsx
const strObj = new String("Lee");
console.log(strObj); // String {0 : "L", 1 : "e", 2 : "e", length : 3, [[PrimitiveValue]] : "Lee"}
```

- `[[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체 생성`
  <br>

```jsx
// 배열과 유사하게 인덱스로 각 문자에 접근 가능
console.log(strOb[0]); // L

// 문자열은 원시 값이므로 변경할 수 없다. 하지만 이때 에러 발생 X
strObj[0] = "S";
console.log(strObj); // 'Lee'
```

- String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이자 이터러블
  - `배열과 유사하게 인덱스를 통해 각 문자에 접근 가능`
    <br>

```jsx
// String 생성자 함수의 인수로 문자열이 아닌 값을 전달한 경우
let strObj = new String(123);
console.log(strObj); // String {0 : "1", 1 : "2", 2 : "3", length : 3, [[PrimitiveValue]] : "123"}

strObj = new String(null);
console.log(strObj); // String {0 : "n", 1 : "u", 2 : "l", 3 : "l", length : 4, [[PrimitiveValue]] : "null"}
```

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 `인수를 문자열로 강제 변환 후 String 래퍼 객체 생성`
  <br>

```jsx
// new 연산자를 사용하지 않고 String 생성자 함수를 호출한 경우

// 숫자 타입 -> 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"

// 불리언 타입 -> 문자열 타입
String(true); // "true"
String(false); // "false"
```

- ew 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환
  - 이를 이용하여 명시적으로 타입 변환 가능
    <br>

## 32.2 length 프로퍼티

> length 프로퍼티는 문자열의 문자 개수 반환

<br>

```jsx
"Hello".length; // 4
"안녕하세요!".length; // 6
```

- String 래퍼 객체는 `유사 배열 객체`
  → length 프로퍼티를 가짐.
  <br>

## 32.3 String 메서드

- `String 객체의 메서드는 언제나 새로운 문자열을 생성하여 반환함.`
  - String 객체의 모든 메서드는 String 래퍼 객체를 직접 변경 X
    <br>
- 문자열은 변경 불가능한 원시 값이기 때문에 `String 래퍼 객체도 읽기 전용 객체로 제공됨.`

<br>

#### 1) String.prototype.indexOf

> 💡 indexOf 메서드는 대상 문자열 (메서드를 호출한 문자열) 에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스 반환
> → 검색에 실패하면 -1 반환

<br>

```jsx
const str = "Hello World";

// 문자열 str 에서 'l' 을 검색하여 첫 번째 인덱스 반환
str.indexOf("l"); // 2

// 문자열 str 의 인덱스 3부터 'l' 을 검색하여 첫 번째 인덱스 반환
str.indexOf("l", 3); // 3

// 대상 문자열에 특정 문자열이 존재하는지 확인
if (str.indexOf("Hello") !== -1) {
	// 문자열 str 에 'Hello' 가 포함되어 있는 경우에 처리할 내용
}

// ES6 에서 도입된 String.prototype.includes 메서드를 사용하면 가독성 ↑
if (str.includes("Hello")) {
	// 문자열 str 에 'Hello' 가 포함되어 있는 경우에 처리할 내용
}
```

- indexOf 메서드의 2번째 인수에는 검색을 시작할 인덱스 전달 가능
- indexOf 메서드 대신 ES6 에서 도입된 includes 메서드를 사용하면 가독성 ↑

<br>

#### 2) String.prototype.search

> 💡 search 메서드는 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스 반환
> → 검색에 실패하면 -1 반환

<br>

```jsx
const str = "Hello world";

// 문자열 str 에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스 반환
str.search(/o/); // 4
str.search(/x/); // -1
```

<br>

#### 3) String.prototype.includes

> 💡 includes 메서드는 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 불리언 값으로 반환

<br>

```jsx
const str = "Hello world";

str.includes("Hello"); // true
str.includes("x"); // false

// 문자열 str 의 인덱스 3부터 'l' 이 포함되어 있는지 확인
str.includes("l", 3); // true
```

- includes 메서드의 2번째 인수에는 검색을 시작할 인덱스 전달 가능

<br>

#### 4) String.prototype.startsWith

> 💡 startswith 메서드는 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 불리언 값으로 반환

<br>

```jsx
const str = "Hello world";

// 문자열 str 이 'He' 로 시작하는지 확인
str.startsWith("He"); // true
str.startsWith("wo"); // false

// 문자열 str 의 인덱스 5부터 시작하는 문자열이 ' ' 로 시작하는지 확인
str.startsWith(" ", 5); // true
```

- startswith 메서드의 2번째 인수에는 검색을 시작할 인덱스 전달 가능

<br>

#### 5) String.prototype.endsWith

> 💡 endswith 메서드는 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 불리언 값으로 반환

<br>

```jsx
const str = "Hello world";

// 문자열 str 이 'ld' 로 시작하는지 확인
str.endsWith("ld"); // true
str.endsWith("He"); // false

// 문자열 str 의 처음부터 5자리까지('Hello') 가 'lo' 로 끝나는지 확인
str.endsWith("lo", 5); // true
```

- endswith 메서드의 2번째 인수에는 검색할 문자열의 길이 전달 가능

<br>

#### 6) String.prototype.charAt

> 💡 charAt 메서드는 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환

<br>

```jsx
const str = "Hello";

for (let i = 0; i < str.length; i++) {
	console.log(str.charAt(i)); // H e l l o
}

// 인덱스가 문자열 범위를 벗어난 경우 빈 문자열 ('') 반환
str.charAt(5); // ''
```

- 인덱스는 문자열의 범위, 즉 0 ~ (문자열 길이-1) 사이의 정수여야 함.
  - 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열 반환

<br>

#### 7) String.prototype.substring

> 💡 substring 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환

<br>

```jsx
const str = "Hello World";

// 인덱스 1 부터 인덱스 4 이전까지의 부분 문자열 반환
str.substring(1, 4); // 'ell'

// 인덱스 1 부터 마지막 문자까지 부분 문자열 반환
str.substring(1); // 'ello World'
```

- substring 메서드의 두 번째 인수는 생략 가능
  - 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지 부분 문자열 반환

<br>

```jsx
const str = "Hello World";

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교체된다.
str.substring(4, 1); // 'ell'

// 인수 < 0 또는 NaN 인 경우 0 으로 취급된다.
str.substring(-2); // 'Hello World'

// 인수 > 문자열의 길이 (str.length) 인 경우 인수는 문자열의 길이로 취급된다.
str.substring(1, 100); // 'ello World'
str.substring(20); // ''
```

- 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교체됨.
- 인수 < 0 또는 NaN 인 경우 0 으로 취급됨.
- 인수 > 문자열의 길이 (str.length) 인 경우 인수는 문자열의 길이로 취급됨.

<br>

#### 8) String.prototype.slice

> 💡 slice 메서드는 substring 메서드와 동일하게 동작한다.
> 단, slice 메서드에는 음수인 인수를 전달할 수 있으며, 이때 대상 문자열의 가장 뒤에서부터 시작하여 부분 문자열 반환

<br>

```jsx
const str = "Hello world";

str.substring(0, 5); // 'Hello'
str.slice(0, 5); // 'Hello'

str.substring(-5); // 'Hello world'
str.slice(-5); // 'world'
```

<br>

#### 9) String.prototype.toUpperCase

> 💡 toUpperCase 메서드는 대상 문자열을 모두 대문자로 변경한 문자열을 반환

<br>

```jsx
const str = "Hello World";

str.toUpperCase(str); // 'HELLO WORLD'
```

<br>

#### 10) String.prototype.toLowerCase

> 💡 toLowerCase 메서드는 대상 문자열을 모두 소문자로 변경한 문자열을 반환

<br>

```jsx
const str = "Hello World";

str.toLowerCase(str); // 'hello world'
```

<br>

#### 11) String.prototype.trim

> 💡 trim 메서드는 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

<br>

```jsx
const str = "   foo   ";

str.trim(); // 'foo'
str.trimStart(); // 'foo   '
str.trimEnd(); // '   foo'

// 정규 표현식을 인수로 전달하여 공백 문자를 제거하는 것도 가능
str.replace(/\s/g, ""); // 'foo'
str.replace(/\s+/g, ""); // 'foo   '
str.replace(/\s+$/g, ""); // '   foo'
```

- Stage 4 에 제안되어 있는 String.prototype.trimStart, String.prototype.trimEnd 를 사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환

<br>

#### 12) String.prototype.repeat

> 💡 repeat 메서드는 대상 문자열을 인수로 전달받은 정수만큼 반복하여 연결한 새로운 문자열을 반환

<br>

```jsx
const str = "abc";

str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2.5); // 'abcabc' (2.5 -> 2)
str.repeat(-2); // RangeError
```

- 인수로 전달받은 정수가 0 이면 빈 문자열을 반환하고, 음수이면 RangeError 발생
  - 인수 생략 시 기본값 0 이 설정됨.
    <br>

#### 13) String.prototype.replace

> 💡 replace 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규 표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환

<br>

```jsx
const str = "Hello world world';

// str 에서 첫 번째 인수 'world' 를 검색하여 두 번째 인수 'Lee' 로 치환한다.
str.replace('world', 'Lee');    // 'Hello Lee world'
```

```jsx
const str = "Hello world";

// 특수한 교체 패턴 사용 가능
str.replace("world", "<strong>$&</strong>");
```

- replace 메서드의 첫 번째 인수로 정규 표현식 전달 가능
- 검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환
- 특수한 교체 패턴 사용 가능
  - `$&` : 검색된 문자열 의미
    <br>

#### 14) String.prototype.split

> 💡 split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환

<br>

```jsx
const str = "How are you doing?";

// 공백으로 구분 (단어로 구분) 하여 배열로 반환
str.split(" "); // ["How", "are", "you", "doing?"]

// 인수로 빈 문자열 전달 -> 각 문자 모두 분리해서 반환
str.split(""); // ["H", "o", "w", "a", ... , "g", "?"]

// 인수 생략 -> 대상 문자열 전체 반환
str.split(); // ["How are you doing?"]

// 공백으로 구분하여 배열로 반환한다. 단, 배열의 길이는 3 이다.
str.split(" ", 3); // ["How", "are", "you"]
```

- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리
- 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환
- 두 번째 인수로 배열의 길이 지정 가능
