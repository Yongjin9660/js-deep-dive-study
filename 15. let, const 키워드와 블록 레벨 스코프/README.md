# let, const 키워드와 블록 레벨 스코프
<br>

## 15.1 var 키워드로 선언한 변수의 문제점
<br>

### 1) 변수 중복 선언 허용

``` jsx 
var x = 1;
var y = 1;

var x = 100;      // -> 초기화문이 있는 변수 선언문은 var 키워드가 없는 것처럼 동작
var y;            // -> 초기화문이 없는 변수 선언문은 무시됨

console.log(x);   // 100
console.log(y);   // 1
```

- 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작  

- 초기화문이 없는 변수 선언문은 무시됨  

&ensp; ``` -> var 키워드로 선언한 변수는 중복 선언이 가능하기 때문에 의도치 않게 값이 변경되는 부작용 발생 가능 ```  
<br>

### 2) 함수 레벨 스코프

``` jsx
var i = 10;

// for 문에서 선언한 i는 전역 변수 -> 이미 선언된 전역 변수 i가 있으므로 중복 선언
for (var i = 0; i < 5; i++) {
  console.log(i);     // 5
}
```
- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정  

- 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수  

&ensp; ``` -> var 키워드로 선언한 변수는 함수 레벨 스코프이기 때문에 의도치 않게 전역 변수가 중복 선언되는 경우 발생 가능  ```    
<br>

### 3) 변수 호이스팅

``` jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언됨. (1. 선언 단계)
// 변수 foo 는 undefined 로 초기화됨. (2. 초기화 단계)
console.log(foo);     // undefined

// 변수에 값 할당 (3. 할당 단계)
foo = 123;
console.log(foo);     // 123

// 변수 선언은 런타임 이전에 암묵적으로 실행됨.
var foo;
```

``` -> 변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의해 에러를 발생시키지는 않지만 가독성을 떨어뜨리고 오류 발생 가능```  

-> var 키워드로 선언한 변수는 변수 호이스팅에 의해 할당 이전에 참조 시 undefined를 반환  
<br>


## 15.2 let 키워드  
<br>

> var 키워드의 단점을 보완하기 위해 ES6 에서 새로운 변수 선언 키워드 (let, const) 도입  

<br>

### 1) 변수 중복 선언 금지  


``` jsx
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456;    // SyntaxError
```

``` -> let 이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언 허용 X  ```  
<br>

### 2) 블록 레벨 스코프

``` jsx
let foo = 1;
{
  let foo = 2;      // 지역 변수
  let bar = 3;      // 지역 변수
}

console.log(foo);   // 1
console.log(bar);   // ReferenceError
```

-> ``` let 키워드로 선언된 변수는 블록 레벨 스코프``` 를 따르기 때문에 모든 코드 블록 (함수, if문, for문, while문, try/catch문 등) 을 지역 스코프로 인정함.   
<br>

### 3) 변수 호이스팅 

``` jsx
// 1. var 키워드로 선언한 변수

console.log(foo);     // undefined

var foo;
console.log(foo);     // undefined

foo = 1;
console.log(foo);     // 1
```
-> var 키워드로 선언한 변수는 런타임 이전에 암묵적으로 <u> '선언 단계' 와 '초기화 단계' 가 한번에 진행됨. </u>  

-> 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 undefined 를 반환하며 에러 발생 X  
<br>

``` ** var 키워드로 선언한 변수의 생명 주기 ** ```

![img](https://poiemaweb.com/img/var-lifecycle.png)  

<br>

``` jsx
// 2. let 키워드로 선언한 변수

console.log(foo);     // ReferenceError

let foo;
console.log(foo);     // undefined

foo = 1;
console.log(foo);     // 1
```

-> let 키워드로 선언한 변수는 <u>'선언 단계' 와 '초기화 단계' 가 분리되어 진행됨. </u>  

-> 런타임 이전에 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행됨.  

-> 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생되는데, 이렇게 ``` 스코프의 시작 지점 ~ 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 '일시적 사각지대 (TDZ :Temporal Dead Zone)' ``` 라고 부름.  
<br>

``` ** let 키워드로 선언한 변수의 생명 주기 ** ```

![img](https://poiemaweb.com/img/let-lifecycle.png)

<br>

``` jsx
let foo = 1;    // 전역 변수

{
  console.log(foo);     // ReferenceError
  let foo = 2;          // 지역 변수
}
```  

-> ES6 에서 도입된 모든 선언은 호이스팅이 이루어지지만 let, const, class 를 사용한 선언문은 <em> <u>호이스팅이 발생하지 않는 것처럼</u> 동작함. </em>  
<br>


### 4) 전역 객체와 let

``` jsx
// 브라우저 환경에서 실행

// 전역 변수
var x = 1;

// 암묵적 전역 
y = 2;

// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window 의 프로퍼티
console.log(window.x);    // 1

// 전역 객체 window 의 프로퍼티는 전역 변수처럼 사용 가능
console.log(x);     // 1

// 암묵적 전역은 전역 객체 window 의 프로퍼티
console.log(window.y);    // 2
console.log(y);     // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window 의 프로퍼티
console.log(window.foo;   // f foo() {}

// 전역 객체 window 의 프로퍼티는 전역 변수처럼 사용 가능
console.log(foo);   // f foo() {}
```

-> var 키워드로 선언한 전역 변수/함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window 의 프로퍼티가 됨.  

-> 전역 객체의 프로퍼티를 참조할 때 window 생략 가능  
<br>

``` jsx
// 브라우저 환경에서 실행
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window 의 프로퍼티가 아님.
console.log(window.x);    // undefined
console.log(x);       // 1
```

-> let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님.  
<br>

## 15.3 const 키워드
> const 키워드는 주로 상수 선언할 때 사용  
> 상수는 재할당이 금지된 변수

<br>

``` jsx
const foo = 1;

const foo;    // -> SyntaxError
```

``` -> const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화 필요```    
<br>

``` jsx
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작
  console.log(foo);     // ReferenceError
  const foo = 1;
  console.log(foo);     // 1
}

// 블록 레벨 스코프를 가짐
console.log(foo);       // ReferenceError
```

-> const 키워드로 선언한 변수는 ``` 블록 레벨 스코프 ```를 가짐.   
<br>

``` jsx
const foo = 1;
foo = 2;      // TypeError
```

-> const 키워드로 선언한 변수는 ``` 재할당 금지```  

-> 원시 값은 변경 불가능한 값이고 const 키워드에 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법 X   

<br>

``` jsx 
const person = {
  name : 'Lee'
};

// 객체는 변경 가능한 값 -> 재할당 없이 변경 가능
person.name = 'Kim';
console.log(person.name);   // {name : 'Kim'}
```

``` -> const 키워드로 선언한 변수에 객체를 할당한 경우 값 변경 가능 ```  

-> const 키워드는 재할당을 금지할 뿐 '불변' 을 의미하지는 않음.  
<br>

## 15.4 var vs. let vs. const
<br>

- ES6 를 사용한다면 var 키워드는 사용하지 않는다.  

- 재할당이 필요한 경우에 한정하여 let 키워드를 사용하고, 이때 변수의 스코프는 최대한 좁게 만든다.  

- 변경이 발생하지 않고 읽기 전용으로 사용하는 (재할당이 필요없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.