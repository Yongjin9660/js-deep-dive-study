# 22. this

## 22.1 this 키워드 
> `자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수`  
> -> this 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티/메서드 참조 가능

<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  // this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function() {
  // this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
}

const circle = new Circle(5);
console.log(circle.getDiameter());      //  10
```

- this 는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조 가능

- 함수 호출 시 this 가 암묵적으로 함수 내부에 전달되며, 함수 내부에서 지역 변수처럼 사용 가능  

<br>

****바인딩**   
&ensp; : **식별자와 값을 연결**하는 과정  
&ensp; ex) 변수 선언은 변수 이름과 확보된 메모리 공간의 주소를 바인딩하는 것.  
<br>

``` jsx
// this 는 어디서든지 참조 가능
// 전역에서 this 는 전역 객체 window 의미
console.log(this);         // window

function square(number) {
  // 일반 함수 내부에서 this 는 전역 객체 window 의미
  console.log(this);       // window
  return number * number;
}
square(2);      // 4

const person = {
  name : 'Lee',
  getName() {
    // 메서드 내부에서 this 는 메서드를 호출한 객체 의미
    console.log(this);     // { name : 'Lee', getName : f }
    return this.name;
  }
};

console.log(person.getName);       // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this 는 생성자 함수가 생성한 인스턴스 의미
  console.log(this);      // Person { name : "Lee" }
}

const me = new Person('Lee');      // Lee
```


- `자바스크립트의 this 는 함수가 호출되는 방식에 따라 this 에 바인딩될 값, 즉 this 바인딩이 동적으로 결정됨.`

- strict mode 가 적용된 일반 함수 내부의 this 에는 undefined 가 바인딩됨.  
 (일반 함수 내부에서는 this 를 사용할 필요가 없기 때문)  
<br>  

## 22.2 함수 호출 방식과 this 바인딩
> `this 바인딩은 함수 호출 방식에 따라 동적으로 결정됨.`

<br>

### ** 함수 호출 방식
- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출  
<br>

### 1) 일반 함수 호출
> ``일반 함수로 호출하면 함수 내부의 this 에는 전역 객체가 바인딩됨.``

<br>

``` jsx 
// 일반 함수
function foo() {
  console.log("foo's this : ", this);           // window
  function bar() {
    console.log("foo's this : ", this);         // window
  }
  bar();
}
foo();

// strict mode 적용
function foo() {
  'use strict';

  console.log("foo's this : ", this);           // undefined
  function bar() {
    console.log("foo's this : ", this);         // undefined
  }
  bar();
}
foo();
```
- 중첩 함수도 일반 함수로 호출하면 함수 내부의 this 에는 전역 객체가 바인딩됨.

- strict mode 가 적용된 일반 함수 내부의 this 에는 undefined 바인딩됨.  
<br>

``` jsx
// var 키워드로 선언한 전역 변수 value 는 전역 객체의 프로퍼티
// const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티 X
var value = 1;

const obj = {
  value : 100,
  foo() {
    console.log("foo's this : ", this);             // { value : 100, foo : f }

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this : ", this);      // window
      console.log("bar's this.value : ", this.value);      // 1
    }

    // 콜백 함수 내부의 this 에는 전역 객체가 바인딩됨
    setTimeout(function() {
      console.log("callback's this : ", this);      // window
      console.log("callback's this.value : ", this.value);      // 1
    }, 100);
    bar();
  }
};

obj.foo();
```
- `일반 함수로 호출된 모든 함수 (중첩함수, 콜백 함수 포함) 내부의 this 에는 전역 객체가 바인딩됨.`  

- 중첩 함수/콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 하므로 외부 함수의 일부 로직을 대신하는 경우가 많음.

- `But, 외부 함수인 메서드와 중첩 함수/콜백 함수의 this 가 일치하지 않는다는 것은 헬퍼 함수로 동작하기 어렵게 만듦.`

<br>

### ** **메서드 내부의 중첩 함수/콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키는 방법**
``` jsx
// 방법 1
var value = 1;

const obj = {
  value : 100, 
  foo() {
    // this 바인딩 (obj) 을 변수 that 에 할당
    const that = this;

    // 콜백 함수 내부에서 this 대신 that 참조
    setTimeout(function () {
      console.log(that.value);      // 100
    }, 100);
  }
};

obj.foo();

// 방법 2
var value = 1;

const obj = {
  value : 100, 
  foo() {
    // 콜백 함수에 명시적으로 this 바인딩
    setTimeout(function() {
      console.log(this.value);      // 100
    }.bind(this), 100);
  }
};

obj.foo();

// 방법 3
var value = 1;

const obj = {
  value : 100, 
  foo() {
    // 화살표 함수 내부의 this 는 상위 스코프의 this 를 가리킴
    setTimeout(() => console.log(this.value), 100);     // 100
  }
};

obj.foo();
```
<br>

### 2) 메서드 호출
>`` 메서드 내부의 this 에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩됨.``

<br>

``` jsx
var obj1 = {
  name : 'Lee',
  sayName : function() {
    console.log(this.name);
  }
}

var obj2 = {
  name : 'Kim'
}

obj2.sayName = obj1.sayName;

obj1.sayName();      // Lee
obj2.sayName();      // Kim
```
- `메서드 내부의 this 는 메서드를 호출한 객체에 바인딩됨.`  
(프로퍼티로 메서드를 가리키고 있는 객체와는 상관 X)
 
<br>

![img](https://poiemaweb.com/img/Method_Invocation_Pattern.png)

<br>

``` jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me
console.log(me.getName());                    // Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype
console.log(Person.prototype.getName());      // Kim
```

- Person.prototype 도 객체이므로 직접 메서드 호출 가능  

- ```프로토타입 메서드 내부에서 사용된 this 도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩됨. ```   
<br>

![img](https://poiemaweb.com/img/prototype_metthod_invocation_pattern.png)

<br>

### <em> `=> this 에 바인딩될 객체는 호출 시점에 결정됨.` </em>
<br>

### 3) 생성자 함수 호출
> ``생성자 함수 내부의 this 는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩됨.``

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

// 반지름이 5인 Circle 객체 생성
const circle1 = new Circle(5);

// 반지름이 10인 Circle 객체 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter());     // 10
console.log(circle2.getDiameter());     // 20

// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작 X 
// 즉, 일반적인 함수의 호출
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle 에는 반환문이 없으므로 암묵적으로 undefined 반환
console.log(circle3);       // undefined

// 일반 함수로 호출된 Circle 내부의 this 는 전역 객체를 가리킴
console.log(radius);        // 15
```
- new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아닌 일반 함수로 동작  
<br>

### 4) Function.prototype.apply/call/bind 메서드에 의한 간접 호출  
> `apply, call, bind 메서드는 Function.prototype 의 메서드로, 모든 함수가 상속받아 사용 가능`

<br>

``` jsx
// apply 메서드 : 호출할 함수의 인수를 배열로 묶어 전달
Function.prototype.apply(thisArg[, argsArray])  

// call 메서드 : 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

// bind 메서드 : 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성하여 반환
Function.prototype.bind(thisArg)
```

- thisArg : this 로 사용할 객체

- argsArray : 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체

- arg1, arg2, ... : 함수에게 전달할 인수 리스트

- return : 호출된 함수의 반환값  
<br>

``` jsx
function getThisBinding() {
  console.log(arguments);
  return this;
}

// this 로 사용할 객체
const thisArg = { a : 1 };

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this 에 바인딩

// apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArg, [1, 2, 3]));       
// Arguments(3) [1, 2, 3, callee : f, Symbol(Symbol.iterator): f]
// {a : 1}

// call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
console.log(getThisBinding.call(thisArg, 1, 2, 3));        
// Arguments(3) [1, 2, 3, callee : f, Symbol(Symbol.iterator): f]
// {a : 1}
```

- `apply 와 call 메서드의 본질적인 기능은 함수를 호출하는 것` 

- apply 와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this 에 바인딩  
(호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작)  
<br>

```jsx
function getThisBinding() {
  return this;
}

// this 로 사용할 객체
const thisArg = { a : 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg 로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성하여 반환
console.log(getThisBinding.bind(thisArg));         // getThisBinding

// bind 메서드는 함수를 호출하진 않으므로 명시적으로 호출해야 함.
console.log(getThisBinding.bind(thisArg)());       // { a: 1 }
```

- `bind 메서드는 apply/call 메서드와 달리 함수 호출 X`  
<br>

``` jsx
const person = {
  name : 'Lee',
  foo (callback) {
    // -> 1번 
    setTimeout(callback, 100);
  }
};

person.foo(function() {
  console.log(`Hi! My name is ${this.name}.`);      // 2번 -> Hi! My name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name 은 브라우저 환경에서 window.name 과 동일
  // 브라우저 환경에서 window.name 은 브라우저 창의 이름을 나타내는 빌트임 프로퍼티이며 기본값은 ''
  // Node.js 환경에서 this.name 은 undefined
});
```

- 1번 시점에서 this 는 foo 메서드를 호출한 객체, 즉 person 객체를 가리킴

- 2번  시점에서 this 는 전역 객체 window 를 가리킴  
<br>

``` jsx
const person = {
  name : 'Lee',
  foo (callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function() {
  console.log(`Hi! My name is ${this.name}.`);      // Hi! My name is Lee
});
```

-``` 메서드의 this 와 메서드 내부의 중첩 함수/콜백 함수의 this 가 불일치하는 문제 해결 가능```  
<br>

## ** 정리

| 함수 호출 방식 | this 바인딩 |
| --- | --- | 
| 일반 함수 호출 | 전역 객체 | 
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | 메서드에 첫 번째 인수로 전달한 객체
