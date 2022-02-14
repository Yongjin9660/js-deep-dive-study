# 31. RegExp

## 31.1 정규 표현식이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- 자바스크립트는 펄의 정규 표현식 문법을 ES3 부터 도입 (JS 고유 문법 X)
- 문자열을 대상으로 `패턴 매칭 기능` 제공
  <br>

##### \*\* **`패턴 매칭 기능이란?`**

→ 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능
<br>

```jsx
// 휴대폰 전화번호 패턴 (xxx-xxxx-xxxx) 을 아래와 같은 정규 표현식으로 정의

// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-5678";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel 이 휴대폰 전화번호 패턴에 매칭되는지 테스트
regExp.test(tel); // false
```

- 정규표현식을 사용하지 않는다면 반복문/조건문을 통해 한 문자씩 연속해서 체크해야 함.
- `정규표현식을 사용`하면 반복문/조건문 없이 `패턴을 정의하고 테스트하는 것으로 간단히 체크 가능`
  - 정규 표현식은 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성 ↓
    <br>

## 31.2 정규 표현식의 생성

- 정규 표현식 리터럴을 사용하여 정규 표현식 생성
- Regexp 생성자 함수를 사용하여 정규 표현식 생성
  <br>

#### 1) 정규 표현식 리터럴 사용

```jsx
const target = "Is this all there is?";

// 패턴 : is
// 플래그 : i -> 대소문자 구별하지 않고 검색
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp 의 패턴을 검색하여
// 매칭 결과를 불리언 값으로 반환
regexp.test(target); // true
```

- 정규 표현식 리터럴은 `패턴`과 `플래그`로 구성됨.
  <br>

#### 2) Regexp 생성자 함수

```jsx
new RegExp(pattern[, flags])

// * pattern : 정규 표현식의 패턴
// * flags : 정규 표현식의 플래그 (g, i, m, u, y)
```

```jsx
const target = "Is this all there is?";

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // true
```

- RegExp 생성자 함수를 사용하여 RegExp 객체 생성
  <br>

## 31.3 RegExp 메서드

#### 1) RegExp.prototype.exec

> 💡 exec 메서드는 **인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환**
> → 매칭 결과가 없는 경우 null 반환

<br>

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// ["is", index : 5, input : "Is this all there is?", groups : undefined]
```

- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 `첫 번째 매칭 결과만 반환`

<br>

#### 2) RegExp.prototype.test

> 💡 test 메서드는 **인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환**

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // true
```

<br>

#### 3) RegExp.prototype.match

> 💡 match 메서드는 **대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환**

<br>

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.match(target);
// ["is", index : 5, input : "Is this all there is?", groups : undefined]
```

```jsx
const target = "Is this all there is?";
const regExp = /is/g;

regExp.match(target); // ["is", "is"]
```

- match 메서드는 `g 플래그가 지정되면 exec 메서드와 달리 모든 매칭 결과를 배열로 반환`

<br>

## 31.4 플래그

| 플래그 | 의미        | 설명                                                       |
| ------ | ----------- | ---------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴 검색                         |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색 유지                    |

<br>

- 플래그는 옵션이므로 `선택적으로 사용 가능`
- 순서와 상관없이 `하나 이상의 플래그를 동시에 설정 가능`
- 어떠한 플래그도 사용하지 않은 경우
  - 대소문자를 구별해서 패턴 검색
  - 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료

<br>

## 31.5 패턴

#### 1) 문자열 검색

```jsx
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴
const regExp = /is/gi;

// 플래그 i -> 대소문자 구별 X
// 플래그 g -> 패턴과 일치하는 모든 문자열을 전역 검색

target.match(regExp); // ["Is", "is", "is"]
```

<br>

#### 2) 임의의 문자열 검색

```jsx
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색
const regExp = /.../g;

target.match(regExp); // ["Is ", "thi", "s  a", "ll ", "the", "re ", "is?"]
```

- `.` 은 `임의의 문자 한 개를 의미`하며, 문자의 내용은 무엇이든 상관없음.
- '...' 은 문자의 내용과 상관없이 3자리 문자열과 매치한다는 의미

<br>

#### 3) 반복 검색

```jsx
const target = "A AA B BB Aa Bb AAA";

// 'A' 가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
const regExp1 = /A{1,2}/g;
target.match(regExp1); // ["A", "AA", "A", "AA", "A"]

// 'A' 가 2번 반복되는 문자열을 전역 검색
const regExp2 = /A{2}/g;
target.match(regExp2); // ["AA", "AA"]

// 'A' 가 최소 2번 이상 반복되는 문자열을 전역 검색
const regExp3 = /A{2,}/g;
target.match(regExp3); // ["AA", "AAA"]
```

- `{m,n}` 은 앞선 패턴이 `최소 m번, 최대 n번 반복`되는 문자열 의미
- `{n}` 은 앞선 패턴이 `n 번 반복`되는 문자열 의미
- `{n, }` 은 앞선 패턴이 `최소 n 번 이상 반복`되는 문자열 의미
- 콤마 뒤에 공백 있으면 정상적으로 동작 X

<br>

```jsx
const target = "A AA B BB Aa Bb AAA";

// 'A' 가 최소 한 번 이상 반복되는 문자열을 전역 검색
const regExp1 = /A+/g;
target.match(regExp1); // ["A", "AA", "A", "AAA"]
```

- `+` 는 앞선 `패턴이 최소 1번 이상 반복`되는 문자열 의미
- 즉, +는 {1,} 과 같음.
  <br>

```jsx
const target = 'color colour';

// 'colo' 다음 'u' 가 최대 한 번 (0번 포함) 이상 반복되고 'r' 이 이어지는
// 문자열 'color', 'colour' 를 전역 검색
const regExp2 = /colou?r/g;
target.match(regExp2; // ["color", "colour"]
```

- `?` 는 앞선 `패턴이 최대 1번 (0번 포함) 이상 반복`되는 문자열 의미
- 즉, ?는 {0,1} 과 같음.

<br>

#### 4) OR 검색

```jsx
// 1번
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B' 가 1번 이상 반복되는 문자열을 전역 검색
const regExp = /A+|B+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

```jsx
// 2번
const target = "A AA B BB Aa Bb";

const regExp = /[AB]+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

- `|` 는 `or 의 의미`를 가지는데, `[ ] 내의 문자는 or 로 동작`하기 때문에 1번을 2번처럼 간단하게 쓰는 것도 가능함.
  <br>

```jsx
const target = "A AA BB ZZ Aa Bb";

// 'A' ~ 'Z' 가 한 번 이상 반복되는 문자열을 전역 검색
const regExp1 = /[A-Z]+/g;
target.match(regExp1); // "A", "AA", "BB", "ZZ", "A", "B"]
```

```jsx
const target = "AA BB 12,345";

// '0' ~ '9' 또는 ',' 가 한 번 이상 반복되는 문자열을 전역 검색
const regExp2 = /[0-9, ]+/g;
target.match(regExp2); // ["12,345"]
```

- `[] 내에 -를 사용하여 범위를 지정할 수 있음.`
- `/d` : 숫자 의미 -> `[0-9]` 와 같은 의미
- `/D` : 숫자가 아닌 문자 의미
- `/w` : 알파벳, 숫자, 언더스코어 의미 -> `[A-Za-z0-9_]` 와 같은 의미
- `/W` : 알파벳, 숫자, 언더스코어가 아닌 문자 의미
  <br>

#### 5) NOT 검색

```jsx
const target = 'AA BB 12 Aa Bb';

// 숫자를 제외한 문자열을 전역 검색
const regExp = /[^0-9]+'g;
target.match(regExp);   // ["AA", "BB", "Aa", "Bb"]
```

- `[...] 내의 ^` 은 `not` 의미
  <br>

#### 6) 시작 위치로 검색

```jsx
const target = "https://poiemaweb.com";

// 'https' 로 시작하는지 검사
const regExp = /^https/;
regExp.test(tarte); // true
```

- `[...] 밖의 ^` 은 `문자열의 시작` 의미
  <br>

#### 7) 마지막 위치로 검색

```jsx
const target = "https://poiemawe.com";

// 'com' 으로 끝나는지 검사
const regExp = /com$/;
regExp.test(target); // true
```

- `$ 은 문자열의 마지막 의미`
  <br>

## 31.6 자주 사용하는 정규표현식

#### 1) 특정 단어로 시작하는지 검사

```jsx
// 'http://' 또는 'https://' 로 시작하는지 검사
const url = "https://example.com";

// 1번 방법
/^https?:\/\//.test(url); // true

// 2번 방법
/^(http|https):\/\//.test(url); // true
```

- `[...] 바깥의 ^` 은 문자열의 시작 의미
- `?` 은 앞선 패턴이 최대 한 번 (0번 포함) 이상 반복되는지 의미
  => 검색 대상 문자열에 앞선 패턴('s') 이 있거나 없거나 매치 O
  <br>

#### 2) 특정 단어로 끝나는지 검사

```jsx
// 'html' 로 끝나는지 검사
const fileName = "index.html";

/html$/.test(fileName); // true
```

- `$` 는 문자열의 마지막 의미
  <br>

#### 3) 숫자로만 이루어진 문자열인지 검사

```jsx
// 숫자로만 이루어진 문자열인지 검사
const target = "12345";

// 처음과 끝이 숫자이고 최소 1번 이상 반복되는 문자열과 매치
/^\d+$/.test(target); // true
```

- `[...] 바깥의 ^` 은 문자열의 시작 의미
- `$` 는 문자열의 마지막 의미
- `\d` 는 숫자를 의미하고, `+` 는 앞선 패턴이 최소 1번 이상 반복되는 문자열 의미
  <br>

#### 4) 하나 이상의 공백으로 시작하는지 검사

```jsx
// 하나 이상의 공백으로 시작하는지 검사
const target = " Hi!";

/^[\s]+/.test(target); // true
```

- `\s` 는 여러 가지 공백 문자 (스페이스, 탭 등) 를 의미
  - 즉, `[\t\r\n\v\f]` 와 같은 의미
    <br>

#### 5) 아이디로 사용 가능한지 검사

```jsx
// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10 자리인지 검사
const id = "abc123";

/^[A-Za-z0-9]{4,10}$/.test(target); // true
```

<br>

#### 6) 메일 주소 형식에 맞는지 검사

```jsx
// 검색 대상 문자열이 메일 주소 형식에 맞는지 검사
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z](-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // true
```

<br>

#### 7) 핸드폰 번호 형식에 맞는지 검사

```jsx
// 검색 대상 문자열이 핸드폰 번호 형식에 맞는지 검사
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

<br>

#### 8) 특수 문자 포함 여부 검사

```jsx
// 검색 대상 문자열에 특수 문자가 포함되어 있는지 검사
// 특수 문자는 A-Za-z0-9 이외의 문자 의미

const target = "abc#123";
/[^A-Za-z0-9]/gi.test(target); // true

// 특수 문자를 제거할 때는 replace 메서드 사용
target.replace(/[^A-Za-z0-9]/gi, ""); // abc123
```
