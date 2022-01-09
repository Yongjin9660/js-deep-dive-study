# 12. 함수


## 12.1 함수란?
> 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것  
> 함수는 값이며, 여러 개 존재할 수 있으므로 식별자인 함수 이름 사용 가능  

<br>  


``` jsx
function add(x, y) {          // 함수 정의
  return x + y;
}

var result = add(2, 5);       // 함수 호출
console.log(result);          // 7
```

- 매개 변수 : 함수 내부로 입력을 전달받는 변수  
- 인수 : 입력  
- 반환값 : 출력  

<br>

## 12.2 함수를 사용하는 이유
- 코드의 재사용
- 유지보수의 편의성 향상
- 코드의 신뢰성 향상
- 코드의 가독성 향상  
<br>

## 12.3 함수 리터럴

``` jsx
// 변수에 함수 리터럴 할당
var f = function add(x, y) {
  return x + y;
}
```
<br>

<h3>💡 함수 리터럴의 구성요소 </h3>

| 구성 요소 | 설명 |
| --- | --- |
|함수 이름 | 1) 함수 이름은 식별자다. 따라서 식별자 네이밍 규칙을 준수해야 한다. |
|          | 2) 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다. |
|          | 3) 함수 이름은 생략할 수 있다. 이름이 있는 기명 함수, 이름이 없는 함수를 무명/익명 함수라고 한다. |
| 매개변수 목록 | 1) 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다. |
|               | 2) 각 매개변수에는 함수를 호출할 때 지정한 인자가 순서대로 할당된다. 즉, 매개변수 목록은 순서에 의미가 있다. |
|               | 3) 매개변수는 함수 몸체 내에서 변수와 동일하게 취급된다. 따라서 매개변수도 변수와 마찬가지로 식별자 네이밍 규칙을 준수해야 한다. |
| 함수 몸체 | 1) 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록이다. |
|           | 2) 함수 몸체는 함수 호출에 의해 실행된다. |

<br>

<h3> ** 일반 객체와 함수 (객체) 와의 차이점 ** </h3>  

- 일반 객체는 호출할 수 없지만 함수는 호출 가능
- 일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 가짐.  
<br>

## 12.4 함수 정의
<br>

| 함수 정의 방식 | 예시 |
| --- | --- |
|  함수 선언문 | function add (x, y) {    return x + y;                              } |
| 함수 표현식 | var add = function (x, y)  { return x + y;                                }  |
| Function 생성자 함수 | var add = new Function (’x’, ‘y’, ‘return x + y’); |
| 화살표 함수 (ES6) | var add = (x, y) ⇒ x + y; |
<br>

<h2> 1. 함수 선언문 </h2>

``` jsx
// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 참조
// console.dir 은 console.log 와는 달리 함수 객체의 프로퍼티까지 출력
// 단, Node.js 환경에서는 console.log 와 같은 결과가 출력됨.
console.dir(add);           // fadd(x, y)

// 함수 호출
console.log(add(2, 5));     // 7
```

-> 함수 선언문은 함수 리터럴과 형태가 동일함.   

-> 함수 리터럴은 함수 이름 생략이 가능하지만 <u> 함수 선언문은 함수 이름을 생략할 수 없음. </u>   
<br>

```jsx
// 함수 선언문은 함수 이름 생략 X
function (x, y) {
  return x + y;     // SyntaxError
}     
```

-> 함수 선언문은 표현식이 아닌 문이기 때문에 함수 선언문을 실행하면 완료 값 undefined 출력  

-> 표현식이 아닌 문, 즉 <u> 함수 선언문은 변수에 할당할 수 없음.  </u>  
<br>

```jsx
// 함수 선언문은 표현식이 아닌 문이므로 변수에 할당 X
// 하지만 함수 선언문이 변수에 할당되는 것처럼 보인다.
var add = function add(x, y) {
  return x + y;
}

// 함수 호출
console.log(add(2, 5));     // 7
```

** 함수 선언문이 변수에 할당되는 것처럼 보이는 이유   

&ensp;자바스크립트 엔진이 코드의 문맥에 따라 동일한 <u>함수 리터럴</u>이라도
<br> <em> &ensp;&ensp; 1) 표현식이 아닌 문인 함수 선언문으로 해석하는 경우 </em> 
<br> <em> &ensp;&ensp; 2) 표현식인 문인 함수 리터럴 표현식으로 해석하는 경우 </em> 가 존재하기 때문  
<br>

-  함수 선언문은 함수 이름을 생략할 수 없다는 점을 제외하면 함수 리터럴과 형태가 동일  
- 이름이 있는 <em> 기명 함수 리터럴은 함수 선언문 or 함수 리터럴 표현식으로 해석될 가능성 O </em> 
<br>   

- ### 기명 함수 리터럴을 ```단독으로 사용``` (함수 리터럴을 피연산자로 사용하지 않는 경우) 하면 ```함수 선언문``` 으로 해석  
- ### 기명 함수 리터럴이 ```값으로 평가되어야 하는``` 문맥 (함수 리터럴을 변수에 할당하거나 피연산자로 사용하는 경우) 에는 ```함수 리터럴 표현식```으로 해석
<br>

```jsx
// 함수 리터럴을 피연산자로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석
// 함수 리터럴에서는 함수 이름 생략 O
(function bar() {0
  console.log('bar');
});

bar();      // ReferenceError
```
-> 함수 리터럴에서 '함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자' 라고 했기 때문에 함수 몸체 외부에서는 함수 이름으로 함수를 호출할 수 없음.

-> 즉, 함수를 가리키는 식별자가 없어서 ReferenceError 가 발생한 것.
<br><br>

<img src="https://media.vlpt.us/images/minj9_6/post/90f4a91f-a584-4790-b277-6aade14b6c59/image.png" width="500px" height="480px">  
<br><br>

``` jsx
// 기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석
// 함수 선언문에서는 함수 이름 생략 X
function foo() {
  console.log('foo');
}

foo();      // foo
```
-> 함수 선언문에서 함수 객체를 가리키는 식별자 foo 를 선언한 적은 없지만 자바스크립트 엔진이 암묵적으로 식별자를 생성함.

-> 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당함.
<br><br>

<img src="https://media.vlpt.us/images/minj9_6/post/e97dc97d-651f-4ede-a529-8a1677fa0072/image.png"  width="500px" height="480px">  
<br><br>

 <h3>💡 함수는 함수 이름으로 호출하는 것이 아니라 <u> 함수 객체를 가리키는 식별자로 호출</u>함.  </h3><br>

<h2> 2. 함수 표현식 </h2>

> ```변수에 할당되는 값이 함수 리터럴인 문  ```  
> 자바스크립트의 함수는 객체 타입으로, 값처럼 변수에 할당할 수도 있고 프로퍼티 값이 될 수도 있으며 배열의 요소가 될 수 있음 -> 자바스크립트 함수는 ```일급 객체``` (값의 성질을 갖는 객체)   

<br>

```jsx
// 기명 함수 표현식
var add = function foo(x, y) {
  return x + y;
};

// 함수 객체를 가리키는 식별자로 호출
console.log(add(2, 5));

// 함수 이름으로 호출하면 ReferenceError 발생
// 함수 이름은 함수 몸체 내부에서만 유효한 식별자이기 때문
console.log(foo(2, 5));     // ReferenceError 
```

-> 함수를 호출할 때는 함수 객체를 가리키는 식별자 사용  

-> 리터럴의 함수 이름은 함수 몸체 내부에서만 유효한 식별자이기 때문에 함수 이름이 아닌 식별자로 호출  
<br>  

<h2> 3. 함수 생성 시점과 함수 호이스팅 </h2>

```jsx
// 함수 참조
console.dir(add);   // f add(x, y)
console.dir(sub);   // undefined

// 함수 호출
console.log(add(2, 5));   // 7
console.log(sub(2, 5));   // TypeError

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x - y;
};
```

-> 함수 선언문으로 정의한 함수는 선언문 이전에 호출 O  

-> 함수 표현식으로 정의한 함수는 함수 표현식 이전에 호출 X

-> 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 ```함수의 생성 시점이 다르기 때문```  

<br>

모든 선언문이 그렇듯 함수 선언문도 런타임 이전에 자바스크립트 엔진에 의해 함수 객체가 먼저 생성되고, 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하여 함수 객체를 할당함.   
<br>

``` ** 함수 호이스팅 ** ```  

> 함수 선언이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징   
> <em> 함수 선언문에서 코드가 한 줄씩 순차적으로 실행되기 시작하는 런타임에는 이미 함수 객체가 생성되어 있고, 함수 이름과 동일한 식별자에 할당까지 완료된 상태 </em>  

<br>

``` ** 변수 호이스팅과 함수 호이스팅의 차이점 ** ```

> 변수 호이스팅 : var 키워드로 선언된 변수는 <u> undefined 로 초기화 </u> 

> 함수 호이스팅 : 함수 선언문을 통해 암묵적으로 생성된 식별자는 <u> 함수 객체로 초기화 </u>  

<br>

``` ** 함수 선언문과 함수 표현식의 생성 시점 ** ```  
> ★ <em> 함수 선언문</em> 은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행됨.   
> -> 함수 선언문으로 함수를 정의하면 <u> 함수 호이스팅</u>이 발생함.  

<br>

> ★ <em> 함수 표현식</em> 의 함수 리터럴은 할당문이 실행되는 시점, 즉 런타임에 평가되어 함수 객체가 됨.  
> -> 함수 표현식으로 함수를 정의하면 함수 호이스팅이 아닌 <u>변수 호이스팅</u> 이 발생함.  
> -> 함수 표현식 이전에 함수를 참조하면 undefined 로 평가되어 이때 함수를 호출하면 undefined 를 호출하는 것과 마찬가지이므로 TypeError 가 발생하는 것.  

<br>

<h2> 4. Function 생성자 함수 </h2>  

> Function 생성자 함수 : 객체 생성하는 함수  

<br>

```jsx
var add = new Function('x' ,'y', 'return x + y');
console.log(add(2, 5))    // 7
```

-> 자바스크립트가 기본 제공하는 빌트인 함수인 Function 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달  

-> new 연산자와 함께 호출하면 함수 객체를 생성해서 반환  

-> But, 함수 선언문/표현식으로 생성한 함수와 동일하게 동작 X  
(클로저를 생성하지 않는 등 => 일반적이지도 않고 바람직하지 않음)

<br>

<h2> 5. 화살표 함수 </h2>
<br>

``` jsx
const add = (x, y) => x + y;
console.log(add(2, 5));     // 7
```

-> ES6 에서 도입된 화살표 함수는 function 키워드 대신 화살표 => 를 사용하여 좀 더 간략하게 함수 선언 가능  

-> 내부 동작이 간략화되어 있음
  - 생성자 함수로 사용할 수 없음
  - this 바인딩 방식이 다름
  - prototype 프로퍼티가 없음
  - arguments 객체를 생성하지 않음

<br>

## 12.5 함수 호출 ##
<br>

```jsx
function add(x, y) {
  console.log(x, y);    // 2 5
  return x + y;
}
 
add(2, 5);

// add 함수의 매개변수 x, y는 함수 몸체 내부에서만 참조 가능
console.log(x, y);    // -> ReferenceError

// 매개변수 y 는 undefined 로 초기화된 상태이기 때문에 NaN 반환
console.log(add(2));      // NaN

// 초과된 인수는 암묵적으로 ARGUMENTS 객체의 프로퍼티로 보관됨.
console.log(add(2, 5, 7));    // 7
```

-> 함수 외부에서 함수 내부로 값을 전달해야 하는 경우 매개변수 (인자) 를 통해 인수 전달 

-> 매개변수는 함수 몸체 내부에서만 참조 가능 (매개변수의 스코프는 함수 내부이기 때문)  

-> 인수가 부족해서 인수가 할당되지 않은 매개변수의 값은 undefined  

-> 매개변수보다 인수가 더 많은 경우 초과된 인수는 무시됨.  
<br>

```jsx
// 함수를 정의할 때 적절한 인수가 전달되었는지 확인하는 방법

function add(x, y) {
  if (typeof x !== 'number' || typeof y !== 'number') {
    throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
  }
  return x + y;
}

console.log(add(2));         // TypeError
console.log(add('a', 'b'))   // TypeError
```

-> 자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인하지 않음  
-> 자바스크립트는 동적 타입 언어 => 매개변수의 타입을 사전에 지정할 수 없음  
<br>

```jsx
// 인수가 전달되지 않은 경우 단축 평가를 사용하여 매개변수에 기본값을 할당하는 방법

function add(a, b, c) {
  a = a || 0;
  b = b || 0;
  c = c|| 0;
  return a + b + c;
}

console.log(add(1, 2, 3));   // 6
console.log(add(1, 2));      // 3
console.log(add(1));         // 1
console.log(add())           // 0
```

``` jsx
// ES6 에서 도입된 매개변수 기본값을 사용하여 함수 내에서 수행하던 인수 체크 및 초기화 간소화하는 방법

function add(a = 0, b = 0, c = 0) {
  return a + b + c;
}

console.log(add(1, 2, 3));    // 6
console.log(add(1, 2));      // 3
console.log(add(1));         // 1
console.log(add())           // 0
```

-> ECMAScript 사양에서는 매개변수의 최대 개수에 대해 명시적으로 제한하고 있지 않음.  
-> 이상적인 매개변수의 개수는 0개이며, 최대 3개 이상을 넘지 않는 것을 권장    
-> 이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 함.

<br>

- <h3> 반환문 </h3>

``` jsx
// 반환문 이후에 다른 문이 존재하면 그 문은 실행되지 않고 무시됨.

function multiply(x, y) {
  return x * y;     // 반환문
  console.log('실행되지 않는다');
}

console.log(multiply(3, 5));    // 15

// 반환문 뒤에 사용할 표현식을 지정하지 않거나 반환문을 생략하면 undefined 반환

function foo() {
  return ;
}

consoel.log(foo());   // undefined
```

-> 반환문은 함수의 실행을 중단하고 함수 몸체를 빠져나가는 역할  

-> 반환문은 return 키워드 뒤에 오는 표현식을 평가하여 반환하는 역할  

(return  키워드 뒤에 반환값으로 사용할 표현식을 명시적으로 지정하지 않으면 undefined 반환)  
<br>

## 12.6 참조에 의한 전달과 외부 상태의 변경
<br>

```jsx
// 매개변수 primitive 는 원시 값을 전달받고, 매개변수 obj 는 객체를 전달받는다.
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = "Kim";
}

// 외부 상태
var num = 100;
var person = {name : 'Lee'};

console.log(num);       // 100
console.log(person);    // {name : 'Lee'}

// 원시 값은 값 자체가 복사되어 전달되고, 객체는 참조 값이 복사되어 전달된다.
changeVal(num, person);

// 원시 값은 원본이 훼손되지 않고, 객체는 원본이 훼손된다.
console.log(num);       // 100
console.log(person);    // {name : 'Kim'}
```

- **원시값**
  - `immutable value`
  - 직접 변경할 수 없기 때문에 재할당을 통해 할당된 원시 값을 새로운 원시 값으로 교체
- **객체**
  - `mutable value`
  - 직접 변경할 수 있기 때문에 재할당 없이 직접 할당된 객체를 변경

<br>

## 12.7 다양한 함수의 형태
<br>

<h3> 1. 즉시 실행 함수</h3>

> 함수 정의와 동시에 즉시 호출되는 함수   
> -> 즉시 실행 함수는 단 한 번만 호출되며 다시 호출 X  

<br>

```jsx
// 익명 즉시 실행 함수
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}()); 

// 기명 즉시 실행 함수
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}());

foo();    // -> ReferenceError
```

-> 즉시 실행 함수는 익명 함수를 사용하는 것이 일반적  

-> 그룹 연산자 (...) 내의 기명 함수는 함수 리터럴로 평가되어 함수 이름은 함수 몸체에서만 참조할 수 있는 식별자이므로 즉시 실행 함수를 다시 호출할 수 없음.  
<br>

```jsx
// 즉시 실행 함수는 반드시 그룹 연산자 (...) 로 감싸야 함.

function () {
  // ...
}();    // -> SyntaxError
```

-> 먼저 함수 리터럴을 평가해서 함수 객체를 생성하기 위해서 그룹 연산자로 함수를 묶는 것.  
<br>

``` jsx
// 즉시 실행 함수도 일반 함수처럼 값 반환 가능
var res = (function() {
  var a = 3;
  var b = 5;
  return a * b;
}());

consoel.log(res);   // 15

// 즉시 실행 함수에도 일반 함수처럼 인수 전달 가능
res = (functoin(a, b) {
  return a * b;
}(3, 5))

console.log(res);   // 15
```  
<br>


<h3> 2. 재귀 함수</h3>

> 함수가 자기 자신을 호출하는 행위, 즉 재귀 호출을 수행하는 함수  
> 주로 반복되는 처리를 위해 사용

<br>

``` jsx
// 일반 함수로 구현한 CountDown 함수
function countdown(n) {
  for (var i = n; i >= 0; i--) console.log(i);
}

countdown(10);

// 재귀 함수로 구현한 CountDown 함수
function countdown(n) {
  if (n < 0) return ;     // n이 0보다 작아지면 바로 함수 종료
  console.log(n);
  countdown(n - 1);   // 재귀 호출
}

countdown(10);
```

-> 반복되는 처리를 반복문 없이 구현 가능    

-> 재귀 함수는 자신을 무한 재귀 호출하기 때문에 탈출 조건을 반드시 만들어야 함.

-> 탈출 조건이 없으면 함수가 무한 호출되어 스택 오버플로 에러 발생  
<br>


``` jsx
// 팩토리얼 함수
function factorial(n) {
  // 탈출 조건 : n 이 1 이하일 때 재귀 호출을 멈춘다.
  if (n <= 1>) return 1;

  // 재귀 호출
  return n * factorial(n - 1);
}

console.log(factorial(4));    // 24
```

-> 함수 몸체 내부에서는 함수 이름이 유효하기 때문에 자기 자신을 호출할 때 함수 이름을 식별자로 사용  
<br>

``` jsx
// 함수 표현식
var factorial = function foo(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);

  // 함수 내부에서는 함수 이름으로 자기 자신 재귀 호출 가능
  // console.log(factorial === foo);   // True
  // return n * foo(n - 1);   -> 이 방법도 가능
};


// 함수 외부에서 함수를 호출할 때는 함수를 가리키는 식별자 사용
console.log(factorial(5));    
```
-> 함수 내부에서는 함수 이름은 물론 함수를 가리키는 식별자로도 자기 자신을 재귀 호출 가능  

-> But, <em> 함수 외부에서 함수를 호출할 때는 반드시 함수를 가리키는 식별자로 해야 함. </em>   
<br>  

<h3> 3. 중첩 함수 (내부 함수) </h3>
 
> 함수 내부에 정의된 함수로, 외부 함수 내부에서만 호출 가능

<br>

``` jsx
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;

    // 외부 함수의 변수 참조 가능
    console.log(x + y);   // 3
  }

  inner();    // 중첩 함수는 외부 함수 내에서만 호출 가능
}

outer();
```

- 외부 함수 : 중첩 함수를 포함하는 함수
- 내부 함수 (중첩 함수) : 함수 내부에 정의된 함수  
(주로 자신을 포함하는 외부 함수를 돕는 헬퍼 함수의 역할 수행)  
<br>

<h3> 4. 콜백 함수</h3>

> 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수  

<br>

``` jsx
// n 만큼 어떤 일을 반복 수행
function repeat1(n) { 
  // i 출력
  for (var i = 0;  i< n; i++) console.log(i);
}

repeat1(5);     // 0 1 2 3 4

// n 만큼 어떤 일을 반복 수행
function repeat2(n) {
  for (var i = 0; i < n; i++) {

    // i가 홀수일 때만 출력
    if (i % 2) console.log(i);
  }
}

repeat2(5);     // 1 3 
```

-> 위 예제의 함수들은 함수의 일부분만이 다르기 때문에 매번 함수를 새롭게 정의했음.  

-> ```함수의 변하지 않는 공통 로직은 미리 정의해 두고, 경우에 따라 변경되는 로직은 추상화해서 함수 외부에서 함수 내부로 전달 ```하는 아래 방법 고려  
<br>

``` jsx
// 외부에서 전달받은 f를 n만큼 반복 호출
function repeat(n, f) {         // -> 고차 함수
  for (var i = 0; i < n; i++) {
    f(i);
  }
}

var logALL = function (i) {     // -> 콜백 함수
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logALL);    // 0 1 2 3 4

var logOdds = function (i) {    // -> 콜백 함수
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달
repeat(5, logOdds);   // 1 3
```

-> repeat 함수는 경우에 따라 변경되는 일을 함수 f로 추상화하여 외부에서 전달받음.  
<br>

- <em>콜백 함수</em> : 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수  
-> 콜백 함수는 고차 함수에 전달되어 헬퍼 함수의 역할 수행

- <em>고차 함수</em> : 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수    
-> 고차 함수는 콜백 함수를 자신의 일부분으로 합성  
-> 고차 함수에 콜백 함수를 전달할 때 호출하지 않고 <u>함수 자체를 전달</u>

<br>

> 고차 함수는 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출  
> 콜백 함수는 고차 함수에 의해 호출되며, 이때 고차 함수는 필요에 따라 콜백 함수에 인수 전달 가능  

<br>

``` jsx
// 콜백 함수를 사용한 이벤트 처리
// myButton 버튼을 클릭하면 콜백 함수 실행
document.getElementById('mybutton').addEventListener('click', function() {
  console.log('button clicked!');
});

// 콜백 함수를 사용한 비동기 처리
// 1초 후에 메세지 출력
setTimeout(function() {
  console.log('1초 경과');
}, 1000);
```

``` jsx
// 콜백 함수를 사용하는 고차 함수 map
var res = [1, 2, 3].map(function (item) {
  return item * 2;
});

console.log(res);     // [2, 4, 6]

// 콜백 함수를 사용하는 고차 함수 filter
res = [1, 2, 3].filter(function (item) {
  return item % 2;
});

console.log(res);     // [1, 3]

// 콜백 함수를 사용하는 고차 함수 reduce
res = [1, 2, 3].reduce(function (acc, cur) {
  return acc + cur;
}, 0);

console.log(res);     // 6
```

<br>

<h3> 5. 순수 함수와 비순수 함수</h3>
<br>

- <em>순수 함수</em> : 외부 상태에 의존하지도 않고 외부 상태를 변경하지도 않는, 즉 부수 효과가 없는 함수  
-> 동일한 인수가 전달되면 언제나 동일한 값 반환 (<u>최소 하나 이상의 인수를 전달받음) </u>   
-> 오직 매개변수를 통해 함수 내부로 전달된 인수에게만 의존해 값을 생성하여 반환

- <em>비순수 함수</em> : 외부 상태에 의존하거나 외부 상태를 변경하는, 즉 부수 효과가 있는 함수  
-> 외부 상태에 따라 반환값이 달라짐.

** 외부 상태 : 전역 변수, 서버 데이터, 파일, Console, DOM 등  
<br>

``` jsx
// 현재 카운트를 나타내는 상태
var count = 0;

// 순수 함수 increase 는 동일한 인수가 전달되면 언제나 동일한 값 반환
function increase(n) {
  return ++n;
}

// 순수 함수가 반환한 결과값을 변수에 재할당해서 상태 변경
count = increase(count);
console.log(count);       // 1

count = increase(count);
console.log(count);       // 2
```

``` jsx
// 현재 카운트를 나타내는 상태 : increase 함수에 의해 변화함
var count = 0;

// 비순수 함수
function increase() {
  return ++count;   // 외부 상태에 의존하며 외부 상태를 변경
}

// 비순수 함수는 외부 상태 (count) 를 변경하므로 상태 변화를 추적하기 어려워짐.
increase();
console.log(count);       // 1

increase();
console.log(count);       // 2
```

<br><br>
** 이미지 출처 : https://velog.io/@minj9_6/JavaScript-%ED%95%A8%EC%88%98%EC%9D%98-%EC%A0%95%EC%9D%98%EC%9D%98-4%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95-%EC%A4%91-%ED%95%A8%EC%88%98-%EC%84%A0%EC%96%B8%EB%AC%B8%EA%B3%BC-%ED%91%9C%ED%98%84%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C  