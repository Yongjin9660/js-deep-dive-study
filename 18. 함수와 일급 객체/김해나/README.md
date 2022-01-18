# 18. 함수와 일급 객체
<br>

## 18.1 일급 객체
> 자바스크립트 함수는 아래의 조건을 모두 만족하므로 '일급 객체' 에 속함.  
> ```함수를 객체와 동일하게 사용할 수 있다는 의미```  

<br>

- 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.  
- 변수나 자료구조 (객체, 배열 등) 에 저장할 수 있다.  
- 함수의 매개변수에 전달할 수 있다.  
- 함수의 반환값으로 사용할 수 있다.  
<br> 

``` jsx
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다. 
// 런타임 (할당 단계) 에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할 수 있다. 
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다. 
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser());     // 1
console.log(increaser());     // 2

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser());     // -1
console.log(decreaser());     // -2
```

-> 함수는 값을 사용할 수 있는 곳 (변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문 등) 이라면 어디서든지 리터럴로 정의 가능  

-> 일반 객체는 호출할 수 없지만 함수 객체는 호출 가능  

```-> 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티 소유  ```  
<br>

## 18.2 함수 객체의 프로퍼티
<br>

** console.dir 메서드를 통해 함수 객체 내부 들여다보기  
<br>
![img](https://user-images.githubusercontent.com/77706631/149686249-1ad66043-5671-4efe-8559-b7d4ad4478ae.png)  
<br>

``` jsx
function square(number) {
  return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));

/*
{
  length : {value : 1, writable : false, enumerable : false, configurable : true},
  name : {value : "squre", writable : false, enumerable : false, configurable : true}, 
  arguments : {value : null, writable : false, enumerable : false, configurable : false},
  caller : {value : null, writable : false, enumerable : false, configurable : false},
  prototype : {value : {...}, writable : true, enumerable : false, configurable : false}
}
*/
```

### ```-> arguments, caller, length, name, prototype 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 데이터 프로퍼티```  
<br>

``` jsx
// __proto__ 는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptors(square, '__proto__'));      // undefined

// __proto__ 는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));

// {get : f, set : f, enumerable : false, configurable : true}
```

### ```-> __proto__ 는 함수 객체 고유의 프로퍼티가 아닌 접근자 프로퍼티로, Object.prototype 객체의 프로퍼티를 상속받은 것.```  
<br>

### 1) arguments 프로퍼티
> ``` 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체 ```  
> -> 함수 내부에서 지역 변수처럼 사용되기 때문에 함수 외부에서는 참조 X   

<br>

![image](https://user-images.githubusercontent.com/77706631/149686994-a7bd026c-2688-41ea-9c8b-2dd82cd411c6.png)
 
<br>

### ``` ** arguments 객체는```

  - 프로퍼티 키 : 인수의 순서 (인덱스)
  - 프로퍼티 값 : 인수  
  - callee 프로퍼티 : arguments 객체를 생성한 함수, 즉 함수 자신을 가리킴
  - length 프로퍼티 : 인수의 개수  
<br>

``` jsx
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

console.log(multiply());           // NaN 
console.log(multiply(1));          // NaN
console.log(multiply(1, 2));       // 2
console.log(multiply(1, 2, 3));    // 2
```

-> 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined 로 초기화된 이후 인수가 할당됨.  

-> 매개변수의 개수보다 인수를 적게 전달한 경우 undefined 로 초기화된 상태를 유지함.  

-> 매개변수의 개수보다 인수를 더 많이 전달한 경우 초과된 인수는 무시됨.   
<br>

``` jsx
function sum() {
  let res = 0;

  // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로 for 문으로 순회 가능
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum(1,2));      // 3
```

-> arguments 객체는 매개변수 개수를 확정할 수 없는 ```가변 인자 함수를 구현```할 때 유용함.  
<br>

``` jsx
// ES6 Rest 파라미터 도입
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2));     // 3
```

-> 유사 배열 객체는 배열이 아니므로 ```배열 메서드를 사용할 경우 에러 발생 ```  
-> ES6 에서는 ```Rest 파라미터를 도입```하여 arguments 객체가 배열 메서드를 사용할 수 있도록 함.  
<br>

### 2) caller 프로퍼티
> ```caller 프로퍼티는 함수 자신을 호출한 함수를 가리킴.```    
> -> ECMAScript 사양에 포함되지 않은 비표준 프로퍼티

<br>

``` jsx
function foo(func) {
  return func();
}

function bar() {
  return 'caller : ' + bar.caller;
}

// 브라우저에서의 실행한 결과
console.log(foo(bar));      // caller : function foo(func) { ... }
console.log(bar());         // caller : null
```
<br>

### 3) length 프로퍼티
> ``` length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킴. ```  

<br>

``` jsx
function foo() {)
console.log(foo.length);      // 0

function bar(x) {
  return x;
}

console.log(bar.length);      // 1

function baz(x, y) {
  return x * y;
}

console.log(baz.length);      // 2
```


-> arguments 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티의 값은 다를 수 있음.
- arguments 객체의 length 프로퍼티 => 함수 호출 시 인자 개수 
- 함수 객체의 length 프로퍼티 => 함수 정의 시 매개변수 개수  
<br>

### 4) name 프로퍼티
>``` name 프로퍼티는 함수 이름을 나타냄.```   

``` jsx
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);          // foo

// 익명 함수 표현식
// ES6 에서는 함수 객체를 가리키는 식별자를 값으로 가짐.
var anonymousFunc = function() {};
console.log(anonymousFunc.name);      // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name);                // bar
```

### -> 익명 함수 표현식의 경우, 
- ES5 에서 name 프로퍼티는 빈 문자열을 값으로 가짐.  
- ES6 에서는 ```함수 객체를 가리키는 식별자를 값으로 가짐.```  
<br>

### 5) __proto__ 접근자 프로퍼티
>```[[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티```  
> -> 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한해 접근 가능

<br>

``` jsx
const obj = { a : 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype 이다.
console.log(obj.__proto__ === Object.prototype);      // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype 의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype 의 메서드다.
console.log(obj.hasOwnProperty('a'));                // true
console.log(obj.hasOwnProperty('__proto__'));        // false
```

** hasOwnProperty 메서드  
&ensp; : 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true 반환  
&ensp; &ensp; (상속받은 프로토타입의 프로퍼티 키인 경우 false 반환)  
<br>

### 6) prototype 프로퍼티
> ```생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티```   
> -> 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴.

<br>

``` jsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype');     // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');                 // false
```

-> 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor 에는 prototype 프로퍼티가 없음.
