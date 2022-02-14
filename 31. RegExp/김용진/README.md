# 31. RegExp

## 31.1 정규 표현식이란?

> 💡 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어

- `패턴 매칭 기능`을 제공
  - 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-567팔';

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트
regExp.test(tel); // false
```

## 31.2 정규 표현식의 생성

> 💡 정규 표현식 리터럴과 RegExp 생성자 함수를 사용

![image](https://user-images.githubusercontent.com/55246584/153702024-a35fd291-2824-4bd1-b9f1-e2ce23f35c19.png)

**정규 표현식 리터럴**

```js
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대문자를 구별하지 않고 검색
const regExp = /is/i;

// test메서드는 target 문자열에 대해 정규 표현식 regExp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환
regExp.test(target); // true
```

**RegExp 생성자 함수**

```js
/**
 * pattern: 정규 표현식의 패턴
 * flags: 정규 표현식의 플래그(g, i, m, u, y)
 */

new RegExp(pattern[, flags])
```

```js
const target = 'Is this all there is?';

const regExp = new RegExp(/is/i); // ES6

regExp.test(target); // true
```

## 31.3 RegExp 메서드

**RegExp.prototype.exec**

```js
/**
 * 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
 * 매칭 결과가 없는 경우 null을 반환
 */

const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

👉 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환

**RegExp.prototype.test**

```js
/**
 * 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
 */

const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // true
```

**RegExp.prototype.match**

```js
/**
 * 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭결과를 배열로 반환
 */

const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

👉 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환

```js
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp); // ["is" ,"is"]
```

## 31.4 플래그

> 💡 정규 표현식의 검색 방식을 설정하기 위해 사용

| 플래그 | 의미        | 설명                                                       |
| :----: | ----------- | ---------------------------------------------------------- |
|   i    | ignore case | 대소문자를 구별하지 않고 패턴을 검색                       |
|   g    | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
|   m    | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속함                |

- 옵션이므로 선택적으로 사용 가능
- 순서와 상관없이 하나 이상의 플래그를 동시에 설정 가능

```js
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색
target.match(/is/);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색
target.match(/is/i);
// ["is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색
target.match(/is/g);
// ["is", "is"];

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색
target.match(/is/gi);
// ["Is", "is", "is"]
```

## 31.5 패턴

### 문자열 검색

> 💡 정규 표현식 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색함

```js
const target = 'Is this all there is?';

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트
regExp.text(target); // true

// target과 정규 표현식의 매칭 결과를 구함
target.match(regExp);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]
//
```

### 임의의 문자열 검색

👉 . 은 임의의 문자 한 개를 의미

```js
const target = 'Is there all tehre is?';

// 임의의 3가지 문자열을 대소문자를 구별하여 전역 검색
const regExp = /.../g;
target.match(regExp); // ["Is", "this", "s a", "ll ", "the", "re ", "is?"]
```

### 반복 검색

👉 {m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
let regExp = /A{1,2}/g;

target.match(regExp); // ["A", "AA", "A", "AA", "A"]

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색
regExp = /A{2,}/g;

target.match(regExp); // ["AA", "AAA"]
```

👉 +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미 = {1,}

```js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 한번 이상 반복되는 문자열('A', 'AA', 'AAA', ...)을 전역 검색
const regExp = /A{1,2}/g;
target.match(regExp); // ["A", "AA", "A", "AAA"]
```

👉 ? 는 앞선 패턴이 최대 한 번(0번) 이상 반복되는 문자열을 의미 = {0,1}

```js
const target = 'color colur';

// 'colo 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
// 문자열 'color', 'colour'를 전역 검색
const regExp = /colou?r/g;

target.match(regExp); // ["color", "colour"]
```

### OR 검색

👉 | 은 or의 의미를 가짐

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색
const regExp = /A|B/g;

target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
```

👉 + 는 분해되지 않는 단어 레벨로 검색

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;

target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

👉 [] 내의 문자는 or로 동작

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

👉 - 는 범위를 지정

```js
const target = 'A AA BB ZZ Aa Bb';

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Z]+/g;
target.match(regExp); // ["A", "AA", "BB", "ZZ", "A", "B"]
```

👉 대소문자 구별하지 않고 알파벳 검색

```js
const target = 'AA BB Aa Bb 12';

// 'A' ~ 'Z' 또는 'a' ~ 'z' 가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Za-z]+/g;
target.match(regExp); // ["AA", "BB", "Aa", "Bb"]
```

👉 숫자를 검색

```js
const target = 'AA BB 12,345';

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[0-9]+/g;
target.match(regExp); // ["12", "345"]
```

👉 \d 는 숫자를 의미, \D는 숫자가 아닌 문자를 의미

```js
const target = 'AA BB 12,345';

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색
let regExp = /[\d,]+/g;

target.match(regExp); // ["12,345"]

// '0' ~ '9'가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp = /[\D,]+/g;

target.match(regExp); // ["AA BB ", ","]
```

👉 \w는 알파벳, 숫자, 언더스코어를 의미 = [A-Za-z0-9_], \W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미

```js
const target = 'Aa Bb 12,345 _$%&';

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색
let regExp = /[\w,]+/g;
target.match(regExp); // ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp = /[\W,]+/g;

target.match(regExp); // [" ", " ", ",", "$%&"]
```

### NOT 검색

👉 [...] 내의 ^ 은 not의 의미를 가짐

```js
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색
const regExp = /[^0-9]+/g;

target.match(regExp); // ["AA BB ", " Aa Bb"]
```

### 시작 위치로 검색

👉 [...] 밖의 ^은 문자열이ㅡ 시작을 의미

```js
const target = 'https://poiemaweb.com';

// 'https'로 시작하는지 검색
const regExp = /^https/;

regExp.test(target); // true
```

### 마지막 위치로 검색

👉 $ 은 문자열의 마지막을 의미

```js
const target = 'https://poiemaweb.com';

// 'com'으로 끝나는지 검사
const regExp = /com$/;

regExp.test(target); // true
```

## 31.6 자주 사용하는 정규표현식

### 특정 단어로 시작하는지 검삭

```js
// 'http://' 또는 'https://'로 시작하는 검사

const url = 'https://example.com';
/^https?:\/\//.test(url); // true
```

### 특정 단어로 끝나는지 검사

```js
// 'html'로 끝나는지 검사

const fileName = 'index.html';

/html$/.test(fileName);
```

### 숫자로만 이루어진 문자열인지 검사

```js
const target = '12345';

/^\d+$/.test(target); // true
```

### 하나 이상의 공백으로 시작하는 검사

```js
const target = ' Hi!';

/^[\s]+/.text(target); // true
```

### 아이디로 사용 가능한지 검사

```js
// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10 자리인지 검사

const id = 'abc123';

/^[A-Za-z0-9]{4,10}$/.text(id); // true
```

### 메일 주소 형식에 맞는지 검사

```js
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}/.test(email); // true
```

### 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

### 특수 문자 포함 여부 검사

```js
const target = 'abc#123';

/[^A-Za-z0-9]/gi.test(target); // true
```

### 참고

- https://velog.io/@krungy/%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D
