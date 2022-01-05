## 김해나 
<br>

## 8.1 블록문
> 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고도 부름. 

&nbsp; => 블록문의 끝에는 세미콜론 X  
&nbsp; => 자바스크립트는 블록문을 하나의 실행 단위로 취급  
<br>

```jsx
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```
<br>

## 8.2 조건문
> 주어진 조건식의 평가 결과에 따라 블록문의 실행 여부 결정

&nbsp; => 자바스크립트는 if-else 문과 switch 문으로 2가지 조건문 제공  
<br>

- <h3>if-else 문</h3>

```jsx
if (조건식1) {
  // 조건식1이 참이면 이 코드 블록 실행
} else if (조건식2) {
  // 조건식2가 참이면 이 코드 블록 실행
} else {
  // 조건식1과 조건식 2 모두 거짓이면 이 코드 블록 실행
}
```

```jsx
var num = 2;
var kind;

if (num > 0) kind = '양수';
else if (num < 0) kind = "음수";
else kind = "영";

console.log(kind);      // 양수
```
&nbsp; => 코드 블록 내의 문이 하나뿐이라면 중괄호 생략 가능

```jsx 
// if-else 문
var x = 2;
var result;

if (x % 2) {            // 짝수는 결과값 0이니까 false, 홀수는 결과값 1이니까 true
  result = "홀수";
} else {
  result = "짝수";
}

console.log(result);     // 짝수
```

```jsx
// 위 if-else 문을 삼항 조건 연산자로 바꿔 쓴 경우
var x = 2;
var result = (x % 2) ? "홀수" : "짝수";
console.log(result);    // 짝수
```

```jsx
// 양수, 음수, 0 -> 3가지 조건을 삼항 조건 연산자로 나타낼 때
var num = 2;
var kind = num ? (num > 0 ? "양수" : "음수") : "영";      // 0은 false로 취급

console.log(kind);       // 양수
```

&nbsp; => 삼항 조건 연산자 표현식은 값처럼 사용할 수 있기 때문에 변수에 할당 가능  
<br>

- <h3>switch 문</h3>
``` jsx
switch(표현식) {
  case 표현식1 :
  switch문의 표현식과 표현식1이 일치하면 실행될 문
  break;

  case 표현식2 :
  switch문의 표현식과 표현식2가 일치하면 실행될 문
  break;
  
  default :     // break 생략
  swtich문의 표현식과 일치하는 case문이 없을 때 실행될 문 
}
```

&nbsp; => switch문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름 이동  
&nbsp; => if-else 문의 조건식은 불리언 값으로 평가되어야 하지만 switch 문의 표현식은 불리언 값보다는 문자열이나 숫자값인 경우가 많음.

```jsx
var number = 2;
var name;

switch(number) {
  case 1 : name = "김해나";
  case 2 : name = "김용진";
  default : name = "없음";

console.log(name);        // "없음"   
}

```
&nbsp; => 해당 이름 대신 "없음" 이 출력된 이유는 각 case문마다 break을 걸어주지 않아서 모든 case문을 한 번씩 다 돌았기 때문  
<br>

 <h3> 💡 폴스루 </h3>
  
  > 문을 실행한 후 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 이후의 모든 case 문과 default 문을 실행한 경우 (default 문에서는 break 생략)

  ``` jsx
  // 폴스루를 활용하여 여러 개의 case 문을 하나의 조건으로 사용한 경우
  var year = 2000;
  var month = 2;
  var days = 0;

  switch(month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
      days = 31;
      break;

    case 4 : case 6 : case 9 : case 11 :
      days = 30;
      break;

    case 2:
    // 윤년 계산 알고리즘
    // 1. 연도가 4로 나누어떨어지는 해 (2000, 2004, 2008 ...) 는 윤년 (2/29) 이다.
    // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해 (2000, 2100, 2200 ...) 는 평년 (2/28) 이다.
    // 3. 연도가 400으로 나누어떨어지는 해 (2000, 2400, 2800 ...) 는 윤년이다.

      days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
      break;

    default :
      console.log("Invalid month");
  }
  console.log(days);    // 29
  ```
<br>

## 8.3 반복문
> 조건식의 평가 결과가 참인 경우 코드 블록 실행

&nbsp; => 자바스크립트는 for 문, while 문, do-while 문으로 3가지 제공  
<br>

- for 문 : 조건식이 거짓으로 평가될 때까지 코드 블록 반복 실행
```jsx
for (var i = 0; i < 2; i++) {
  console.log(i);
}
console.log(i);   // 0 1

// 무한루프
for (;;) {        
  ...
}

// 중첩 for 문
for (var i = 0; i <= 6; i++) {
  for (var j = 1; j <= 6; j++) {
    if (i + j === 6) console.log(`[${i}, ${j}]`)
  }
}
// -> 실행 결과 : [1, 5] [2, 4] [3, 3] [4, 2] [5, 1]
```
<br>

- while 문 : 주어진 조건식의 평가 결과가 참이면 코드 블록 반복 실행
```jsx
var count = 0;

// count 가 3보다 작을 때까지 코드 블록 반복 실행
while (count < 3) {
  console.log(count);     // 0 1 2
  count++;
}

// 무한루프
while(true) {
  ...
}

// 무한루프에서 탈출하기 위해서는 if 문으로 탈출 조건 만들고 break 문으로 탈출
var count = 0;

while(true) {
  console.log(count);
  count++;
  if (count === 3) break;
}   // 출력 결과 : 0 1 2
```
<br>

- do-while 문 : 코드 블록을 일단 먼저 실행하고나서 조건식 평가  
-> 코드 블록은 무조건 한 번이상 실행됨.

```jsx
var count = 0;

// count 가 3보다 작을 때까지 코드 블록 반복 실행
do {
  console.log(count);     // 0 1 2
  count++;
} while (count < 3);
```


## 8.4 break 문
> 레이블 문, 반복문 (for, for...in, for...of, while, do-while) 또는 switch 문의 코드 블록 탈출

&nbsp; => 레이블 문 (식별자가 붙은 문), 반복문, switch 문의 코드 블록 외에 break 문 사용하면 문법 에러 발생  
<br>

```jsx
// if 문 -> 오류 발생
if (true) {
  break;      // -> Uncaught SyntaxError
}

// foo 라는 레이블 식별자가 붙은 레이블 문
foo : console.log('foo');

// 레이블 문 -> 정상 동작
foo : {
  console.log(1);
  break foo;    // 레이블 문을 탈출하려면 break 문에 식별자 지정
  console.log(2);
}
```
<br>

## 8.5 continue 문
> 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름 이동

&nbsp; => break 문처럼 반복문 탈출 X  
<br>

```jsx
// 문자열에서 특정 문자의 개수 세는 예제
var string = "Hello World!";
var search = "l";
var count = 0;

for (var i = 0; i < string.length; i++) {
  // if 문 조건이 맞으면 count++; 실행하지 않고 바로 다음 숫자로 이동
  if (string[i] !== search) continue;   
  count++;
}
console.log(count);     // 3
```