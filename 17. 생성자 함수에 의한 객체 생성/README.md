# 17. 생성자 함수에 의한 객체 생성
<br>

** 생성자 함수 
> ```new 연산자와 함께 호출하여 객체 (인스턴스) 를 생성하는 함수```  

<br>

** 인스턴스 
> ```생성자 함수에 의해 생성된 객체```

<br>

## 17.1 Object 생성자 함수

<br>

``` jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Lee";
person.sayHello = function () {
  console.log('Hi! My name is ' + person.name);
};

console.log(person);      // { name : "Lee", sayHello : f }
person.sayHello();        // Hi! My name is Lee
```

### ``` -> new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환```    
-> 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체 완성

``` jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj);            // object
console.log(strObj;);                  // String {"Lee"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj);            // object
console.log(numObj;);                  // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj);           // object
console.log(boolObj;);                 // Boolean {true}

// Function 생성자 함수에 의한 Function 객체 (함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func);              // object
console.log(func);                     // f anonymous(x)

// Array 생성자 함수에 의한 Array 객체 (배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr);               // object
console.log(arr);                      // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체 (정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp);            // object
console.log(regExp);                   // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date);              // object
console.log(date);                     // Mon May 04 2020 08:35:12 GMT+0900 (대한민국 표준시)
```

-> 자바스크립트는 Object 생성자 함수 이외에도 다양한 빌트인 생성자 함수 제공
- String / Number / Boolean
- Function / Array
- Date / RegExp / Promise  
<br>

## 17.2 생성자 함수

### - 객체 리터럴에 의한 객체 생성 방식의 문제점
> ```객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성 가능```  
> -> 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하는 단점 O

<br>

```js
const circle1 = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	},
};

const circle2 = {
	radius: 10,
	getDiameter() {
		return 2 * this.radius;
	},
};
```

- 객체의 프로퍼티는 객체 고유의 상태를 표현

- 메서드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현  

- 프로퍼티 값은 객체마자 다를 수 있지만 메서드는 동일한 경우가 대부분  
<br>

### - 생성자 함수에 의한 객체 생성 방식의 장점
> ```프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능```  
-> 객체 (인스턴스) 를 생성하기 위한 템플릿 (클래스) 처럼 사용

<br>

- `생성자 함수`는 이름 그대로 객체(인스턴스)를 생성하는 함수

  - 일반 함수와 동일한 방법으로 생성자 함수를 정의

  - **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작**  
<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성
const circle1 = new Circle(5);      // 반지름이 5인 Circle 객체 생성
const circle2 = new Circle(10);     // 반지름이 10인 Circle 객체 생성

console.log(circle1.getDiameter());   // 10
console.log(circle2.getDiameter());   // 20
```  
<br>

### ``` ** this 바인딩```
| 함수 호출 방식 | this 가 가리키는 값 (this 바인딩) |
| --- | --- | 
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체 (마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 | 

```-> this 바인딩은 함수 호출 방식에 따라 동적으로 결정됨.```  
<br>

### - 생성자 함수의 인스턴스 생성 과정
> 💡 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로서 동작  
> ⇒ 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화 (인스턴스 프로퍼티 추가 및 초기값 할당)

<br>

``` jsx
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩된다.
  console.log(this);      // Circle {}

// 2. this 에 바인딩되어 있는 인스턴스를 초기화한다.
this.radius = radius;
this.getDiameter = function () {
  return 2 * this.radius;
  };

  // 3. 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this 를 반환한다.
const circle = new Circle(1);
console.log(circle);        // Circle { radius : 1, getDiameter : f }
```

### ``` 1) 인스턴스 생성과 this 바인딩```  

> 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this 에 바인딩된다. 

<br>

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}
}
```

- **암묵적으로 빈 객체가 생성**  

  ⇒ (아직 완성 X) 생성자 함수가 생성한 인스턴스

- **빈 인스턴스는 this에 바인딩됨**  

  ⇒ 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가리키는 이유  
<br>

### ```2) 인스턴스 초기화```  
> 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this 에 바인딩되어 있는 인스턴스를 초기화함.  

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}

	// 2. this에 바인딩되어 있는 인스턴스를 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}
```

- this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가  

- 생성자 함수의 인수로 전달받은 값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당

<br>

### ```3) 인스턴스 반환```  
> 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환됨.

<br>

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this 에 바인딩
	console.log(this); // Circle {}

	// 2. this에 바인딩되어 있는 인스턴스를 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환
	// 명시적으로 원시 값을 반환하면 무시됨
	return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환
const circle = new Circle(1);
console.log(circle); // Circle { radius: 1, getDiameter: f }
```

- this 가 아닌 다른 객체를 명시적으로 반환하면 return 문에 명시한 객체가 반환됨.

- 명시적으로 원시 값을 반환하면 암묵적으로 this 가 반환됨.  

&ensp; => 생성자 함수 내부에서 명시적으로 this 가 아닌 다른 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손하는 것  

&ensp; ``` => 생성자 함수 내부에서는 return 문을 반드시 생략해야 함.```   
<br>

### - 내부 메서드 [[Call]] 과 [[Construct]]
> 함수는 객체에 속하지만 일반 객체와 다르게 호출 가능함.

<br>

`callable`

- 내부 메서드 \[[Call]]을 갖는 함수 객체
- 호출할 수 있는 객체 ⇒ 함수

`constructor`

- 내부 메서드 \[[Constructor]]을 갖는 함수 객체
- 생성자 함수로서 호출할 수 있는 함수

`non-constructor`

- 내부 메서드 \[[Constructor]]을 갖지 않는 함수 객체
- 생성자 함수로서 호출할 수 없는 함수

&ensp;-> 함수 객체는 **callable이면서 constructor** 이거나 **callable 이면서 non-constructor**  

<br>

``` jsx
function foo() {}

// 일반적인 함수로서 호출 => [[Call]] 이 호출됨.
foo();

// 생성자 함수로서 호출 => [[Construct]] 이 호출됨.
new foo();
```
<br>

![img](https://media.vlpt.us/images/ssu00/post/206fb440-2ba7-4d36-8443-bb823fb4fa07/callable.png)  
<br>

### constructor와 non-constructor의 구분

`** constructor`

- 함수 선언문, 함수 표현식, 클래스(클래스도 함수)

`** non-constructor`

- 메서드 (ES6 메서드 축약 표현), 화살표 함수

- 함수가 ```일반 함수```로서 호출되면 함수 객체의 내부 메서드 ```[[Call]]``` 이 호출됨.
  - callable  : 호출할 수 있는 객체, 즉 함수 의미    
<br>

- ```new 연산자와 함께 생성자 함수```로서 호출되면 내부 메서드 ```[[Construct]]``` 가 호출됨.  
  - constructor : 생성자 함수로서 호출할 수 있는 함수
  - non-constructor :  생성자 함수로서 호출할 수 없는 함수

### ``` => 모든 함수는 callable 이면서 constructor 이거나 callable 이면서 non-constructor```   
<br>

### - constructor 과 non-constructor 의 구분
> 함수 객체를 생성할 때 <u> 함수 정의 방식</u>에 따라 함수를 constructor 와 non-constructor 로 구분

<br>

``` jsx
// 일반 함수 정의 : 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};

// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x : function () {}
};

// 일반 함수로 정의된 함수만이 constructor 이다.
new foo();         // foo {}
new bar();         // bar {}
new baz.x();       // x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow();       // TypeError

// 메서드 정의 : ES6 의 메서드 축약 표현만 메서드로 인정
const obj = {
  x() {}
};

new obj.x();       // TypeError
```

- constructor : 함수 선언문, 함수 표현식, 클래스 (클래스도 함수)
- non-constructor : 메서드 (ES6 메서드 축약 표현), 화살표 함수  
<br>


### - new 연산자
> new 연산자와 함께 호출하는 함수는 constructor

<br>

``` jsx
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst);      // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');

// 함수가 생성한 객체를 반환
console.log(isnt);      // { name : "Lee", role : "admin" }
```

- ```new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작함.```    
  -> 함수 객체의 내부 메서드 [[Call]] 이 아닌 ```[[Construct]]``` 가 호출되는 것.    
<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출 시 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle);          // undefined

// 일반 함수 내부의 this 는 전역 객체 window 를 가리킨다.
console.log(radius);          // 5
console.log(getDiameter());   // 10

circle.getDiameter();         // TypeError
```

- ```new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출됨.```    
  -> 함수 객체의 내부 메서드 [[Construct]] 가 아닌 ```[[Call]]``` 이 호출되는 것.  
<br>

### - new.target
> 함수 내부에서 new.target 을 사용하면 ```new 연산자와 함께 생성자 함수로서 호출되었는지 확인 가능```   
> -> this와 유사하게 constructor 인 모든 함수 내부에서 암묵적인 지역 변수로 사용되며, '메타 프로퍼티' 라고도 불림.  

<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target 은 undefined

  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스 반환
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출해도 new.target 을 통해 생성자 함수로서 호출됨
const circle = Circle(5);
console.log(circle.getDiameter());
```

- new 연산자와 함께 생성자 함수로서 호출된 경우  
  -> 함수 내부의 new.target 은 함수 자신을 가리킴.

- new 연산자 없이 일반 함수로서 호출된 경우  
  -> 함수 내부의 new.target 은 undefined  
<br>

``` jsx
// Object 와 Function 생성자 함수
let obj = new Object();
console.log(obj);       // {}

obj = Object();
console.log(obj);       // {}

let f = new Function('x', 'return x ** x');
console.log(f);         // f anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f);         // f anonymous(x) { return x ** x }


// String, Number, Boolean 생성자 함수 
const str = String(123);
console.log(str, typeof str);     // 123 string

const num = Number('123');
console.log(num, typeof num);     // 123 number

const bool = Boolean('true');
console.log(bool, typeof bool);     // true boolean
```

- Object 와 Function 생성자 함수  
  -> new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작  

- String, Number, Boolean 생성자 함수  
  -> new 연산자 없이 호출하면 문자열, 숫자, 불리언 값 반환  

<br><br>
** 이미지 출처 : https://velog.io/@ssu00/17%EC%9E%A5-%EC%83%9D%EC%84%B1%EC%9E%90-%ED%95%A8%EC%88%98%EC%97%90-%EC%9D%98%ED%95%9C-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1