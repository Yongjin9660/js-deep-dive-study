# 19. 프로토타입
<br>

### **자바스크립트란?**
- ```프로토타입 기반의 객체 지향 프로그래밍 언어```  
- 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어    

<br>

## 19.1 객체지향 프로그래밍
> 절차지향적 관점에서 벗어나 ```여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임``` 

<br>

** 추상화 
: 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것.  
<br>

``` jsx
const circle = {
  radius : 5,          // 반지름 -> 원의 속성을 나타내는 데이터 (프로퍼티)

  // 원의 지름 : 2r -> 동작 (메서드)
  getDiameter() {
    return 2 * this.radius;
  },

  // 원의 둘레 : 2πr -> 동작 (메서드)
  getPerimeter() {
    return 2 * Math.PI * this.radius;
  }
};

console.log(circle);     // 10
// { radius : 5, getDiameter : f, getPerimeter : f }

console.log(circle.getDiameter());      //  31.41592
console.log(circle.getPerimeter());     // 78.539816
```

### ```-> 객체는 상태 데이터를 나타내는 프로퍼티와 동작을 나타내는 메서드로 이루어진 복합적인 자료구조```  
<br>

## 19.2 상속과 프로토타입
<br>

### **상속이란?**
- ```어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것.```   

- 자바스크립트는 <u>프로토타입을 기반</u>으로 상속을 구현하여 <u> 불필요한 중복을 제거함.</u>   
<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);

// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유함.
console.log(circle1.getArea === circle2.getArea);     // false
```

-> radius 프로퍼티 값은 일반적으로 인스턴스마다 다름.  

-> Circle 생성자 함수는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성했기 때문에 모든 인스턴스가 중복 소유하고 있음.

```-> 모든 인스턴스가 동일한 내용의 메서드를 사용할 때는 단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직함.```   
<br>

``` jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가

// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있음.
Circle.prototype.getArea = function () {
  return Math.PI  * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea);     // true
```

```-> getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype 의 메서드로 할당되어 있음.```

-> Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는 프로토타입 Circle.prototype 으로부터 getArea 메서드를 상속받아 사용할 수 있게 됨.   

-> 즉, 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고, 내용이 동일한 메서드는 상속을 통해 공유하여 사용하는 것.  
<br>

## 19.3 프로토타입 객체
- 프로토타입은 ```객체 간 상속을 구현```하기 위해 사용됨.
- 어떤 객체의 ```부모 객체의 역할을 하는 객체```로서 다른 객체에 공유 프로퍼티를 제공함.

- 모든 객체는 하나의 프로토타입을 가지며, ```객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]] 에 저장됨.```

<br>

![img](https://www.howdy-mj.me/static/a88859306abea59f9f7b0277f510fe8c/891d5/connect.png)

<br>

-> [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만 **__proto__ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근 가능**  

-> **프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근 가능**    

<br>

### 1) __proto__ 접근자 프로퍼티
> ```모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근 가능```

<br>

![img](https://user-images.githubusercontent.com/77706631/149862067-a066d6c7-c751-4ae9-99af-36fe4b8938ad.png)

- 빨간 부분 : <span style="color:red">person 객체의 프로퍼티</span>

- 파란 부분 : <span style="color:blue">person 객체의 프로토타입 </span>  

  -> Object.prototype 의미  
<br>

``` jsx
const obj = {};
const parent = { x : 1 };

// getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입 취득
obj.__proto__;

// setter 함수인 set __proto__ 가 호출되어 obj 객체의 프로토타입 교체
obj.__proto__ = parent;

console.log(obj.x);     // 1
```

-> 접근자 프로퍼티 __proto__ 를 통해 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근 가능  

- __proto__ 접근자 프로퍼티를 통해 ```프로토타입에 접근```하면 __proto__ 접근자 프로퍼티의 ```getter 함수```가 호출됨. 

- __proto__ 접근자 프로퍼티를 통해 ```새로운 프로토타입을 할당```하면 __proto__ 접근자 프로퍼티의 ```setter 함수```가 호출됨.   
<br>

### ** __접근자 프로퍼티란?__
> 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 ```접근자 함수 [[Get]], [[Set]]``` 프로퍼티 O  

<br>

``` jsx
const person = { name : 'Lee' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다. 
console.log(person.hasOwnProperty('__proto__'));      // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype 의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));

// { get : f, set : f, enumerable : false, configurable : true }

// 모든 객체는 Object.prototype 의 접근자 프로퍼티 __proto__ 를 상속받아 사용 가능
console.log({}.__proto__ === Object.prototype);       // true
```
- ``` __proto__ ```접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아닌 ```Object.prototype 의 프로퍼티 ```   

- 모든 객체는 ```상속```을 통해 ```Object.prototype.__proto__ 접근자 프로퍼티 사용 가능```   
<br>

### ** **Object.prototype 이란?**
-> 프로토타입 체인의 종점, 즉 <em>프로토타입 체인의 최상위 객체</em>  
-> 이 객체의 프로퍼티와 메서드는 모든 객체에 상속됨.  
<br>

``` jsx
const parent = {};
const child = {};

// child 의 프로토타입을 parent 로 설정
child.__proto__ = parent;

// parent 의 프로토타입을 child 로 설정
parent.__proto__ = child;       // TypeError
```
-> 위 예제는 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인   

-> 이렇듯 순환 참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 무한 루프에 빠지게 됨.  

```-> 프로토타입은 단방향 링크드 리스트로 구현, 즉 프로퍼티 검색 방향이 한쪽 방향으로만 흘러가야 한다는 것.```  
<br>  

### 2) prototype 프로퍼티
> 함수 객체만이 소유하는 prototype 프로퍼티는 ```생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴.```   
> -> 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor 는 이 프로퍼티를 소유/생성하지 않음.

<br>
** 화살표 함수와 ES6 의 메서드 축약 표현으로 정의한 메서드는 non-constructor 에 속함.   

<br>

``` jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// Person.prototype 과 me.__proto__ 는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__);       // true
```

### ```=> 모든 객체가 가지고 있는 (Object.prototype 으로부터 상속받은) __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 동일한 프로토타입을 가짐.```  

<br>

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 | 
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 | 
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 인스턴스의 프로토타입을 할당하기 위해 사용 | 
<br>

### 3) 프로토타입의 constructor 프로퍼티와 생성자 함수
> 모든 프로토타입은 constructor 프로퍼티를 가지고, constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴.   
> 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄짐.  

<br>

``` jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person 이다.
console.log(me.constructor === Person);       // true
```

- Person 생성자 함수가 me 객체를 생성하고, me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결됨.  

- me 객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype 에는 constructor 프로퍼티가 존재함.  

- me 객체는 프로토타입인 Person.prototype 의 constructor 프로퍼티를 상속받아 사용할 수 있게 됨.   
<br>

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
<br>

``` jsx
// obj 객체를 생성한 생성자 함수는 Object 이다.
const obj = new Object();
console.log(obj.constructor === Object);         // true

// add 함수 객체를 생성한 생성자 함수는 Function 이다.
const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function);       // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person 이다.
const me = new Person('Lee');
console.log(me.constructor === Person);          // true
```

- 생성자 함수에 의해 ```생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결됨. ```  

- 이때 constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수 의미  
<br>

``` jsx
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object);            // true

// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 선언했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면
// 함수 foo 의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function);          // true
```

- ```객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체와 미묘한 차이가 있지만 결국 객체로서 동일한 특성을 가짐.```

- 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하기 때문에 <em>가상적인 생성자 함수를 가짐.</em> 

- **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재함.**  

<br>

| 리터럴 표기법 | 생성자 함수 | 프로토타입 | 
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype | 
| 배열 리터럴 | Array | Array.prototype | 
| 정규 표현식 리터럴 | RegExp | RegExp.prototype | 
<br>

## 19.5 프로토타입의 생성 시점
> 객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 ```모든 객체는 생성자 함수와 연결되어 있음.```  
> -> ```프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성됨.```

<br>

### 1) 사용자 정의 생성자 함수와 프로토타입 생성 시점
> 생성자 함수로서 호출할 수 있는 함수, 즉 constructor 는 함수 정의가 평가되어 ```함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨.```

<br>

``` jsx
// 함수 정의 (constructor) 가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨.
console.log(Person.prototype);        // { constructor : f }

// 생성자 함수
function Person(name) {
  this.name = name;
}

// 화살표 함수는 non-constructor 
const Person = name => {
  this.name = name;
};

// non-constructor 는 프로토타입 생성 X 
console.log(Person.prototype);        // undefined
```

- 함수 선언문으로 정의된 Person 생성자 함수는 함수 호이스팅에 의해 어떤 코드보다도 먼저 평가되어 함수 객체가 됨.  

- ```사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성됨.```  

- ```이때 생성된 프로토타입의 프로토타입은 언제나 Object.prototype```     
<br>

### 2) 빌트인 생성자 함수와 프로토타입 생성 시점
> Object, String, Number, Array, Function, RegExp, Date, Promise 등과 같은 ```모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성됨.```  
> -> 이때 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩됨.  

<br>

### ** **전역 객체란?** 
&ensp; : 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 생성되는 특수한 객채
- 브라우저에서는 __window__ 객체 의미
- Node.js 에서는 __global__ 객체 의미  

``` jsx
// 전역 객체 window 는 브라우저에 종속적이므로 아래 코드는 브라우저 환경에서 실행해야 한다.
// 빌트인 객체인 Object 는 전역 객체 window 의 프로퍼티다.
window.Object === Object;         // true
```

```-> 표준 빌트인 객체인 Object 도 전역 객체의 프로퍼티이며, 전역 객체가 생성되는 시점에 생성됨.```   
 
1) 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재함.  

2) 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다. 

3) 이로써 생성된 객체는 프로토타입을 상속받는다.  
<br>


## 19.6 객체 생성 방식과 프로토타입의 결정
<br>

### ** **객체 생성 방법**
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

=> 각 방식마다 세부적인 차이는 있으나 ```추상연산 OrdinaryObjectCreate 에 의해 생성```된다는 공통점 O  

-> 즉, 프로토타입은 추상 연산 OrdinaryObjectCreate 에 전달되는 인수에 의해 결정되며, 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정됨.   
<br>

### **추상 연산이란?**
> 추상 연산 OrdinaryObjectCreate 는 ```빈 객체를 생성한 후, 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가```

<br>

### 1) 객체 리터럴에 의해 생성된 객체의 프로토타입
> **객체 리터럴에 의해 생성되는 객체의 프로토타입은 ```Object.prototype```**

<br>

``` jsx
const obj = { x : 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype 을 상속받는다.
console.log(obj.constructor === Object);        // true
console.log(obj.hasOwnProperty('x'));           // true
```

-> 객체 리터럴에 의해 생성된 obj 객체는 <em>Object.prototype 을 프로토타입으로 갖게 되면서 Object.prototype 을 상속받음.</em>    

-> obj 객체는 constructor 프로퍼티와 hasOwnProperty 메서드 등을 소유하지 않지만 자신의 프로토타입인 Object.prototype 의 프로퍼티와 메서드를 자유롭게 사용할 수 있게 됨.    
<br>

### 2) Object 생성자 함수에 의해 생성된 객체의 프로토타입
> **Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 ```Object.prototype```**

<br>

``` jsx
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype 을 상속받는다.
console.log(obj.constructor === Object);        // true
console.log(obj.hasOwnProperty('x'));           // true
```

- 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티 추가
- Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티 추가  
<br>

### 3) 생성자 함수에 의해 생성된 객체의 프로토타입
> **생성자 함수에 의해 생성되는 객체의 프로토타입은 ```생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체```**


<br>

``` jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();        // Hi! My name is Lee
you.sayHello();       // Hi! My name is Kim
```

- 사용자 정의 생성자 함수 Person 과 더불어 생성된 프로토타입 Person.prototype 의 프로퍼티는 constructor 하나뿐  

- ```프로토타입도 객체이기 때문에 일반 객체처럼 프로퍼티를 추가하여 하위 객체가 상속받을 수 있도록 구현```  

- Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용 가능  
<br>

## 19.7 프로토타입 체인
> 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 ```[[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는 것```

<br>

``` jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty 는 Object.prototype 의 메서드다.
console.log(me.hasOwnProperty('name'));       // true
```

- Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype 의 메서드인 hasOwnProperty 를 호출할 수 있음.  

- me 객체의 프로토타입은 Person.prototype 이지만 Person.prototype 뿐만 아니라 Object.prototype 도 상속받았다는 것을 의미함.

### &ensp; ``` ** object.prototype 은 프로토타입 체인의 종점으로, 모든 객체는 Object.prtotype 을 상속받음.```  
<br>

``` jsx
console.log(me.foo);        // undefined
```

-> 프로토타입 체인의 종점인 Object.prototype 에서도 프로퍼티를 검색할 수 없는 경우 undefined 반환 (에러 X)  
<br>

``` jsx
// 프로퍼티가 아닌 식별자는 스코프 체인에서 검색
me.hasOwnProperty('name');
```
- 먼저 스코프 체인에서 me 식별자 검색 (전역 스코프 확인)
- me 객체의 프로토타입 체인에서 hasOwnProperty 메서드 검색  

  -> ```프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘```  

  -> ```스코프 체인은 식별자 검색을 위한 메커니즘```  

  -> 스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용됨.  
<br>

## 19.8 오버라이딩과 프로퍼티 섀도잉

``` jsx
const Person = (funtion () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function() {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수 반환
  return Person;
}());

const me = new Person('Lee');

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello();    // Hey! My name is Lee

// -> sayHello 는 프로토타입 메서드 sayHello 를 오버라이딩했고, 프로토타입 메서드 sayHello 에서는 프로퍼티 섀도잉이 발생한다.
```

- 프로토타입 프로퍼티 : 프로토타입이 소유한 프로퍼티 (메서드 포함)

- 인스턴스 프로퍼티 : 인스턴스가 소유한 프로퍼티 (메서드 포함)  
<br>


![img](https://media.vlpt.us/images/rlatp1409/post/9494d940-71c9-4d89-b9d1-918071ab9e16/IMG_82F41D4FD6A2-1.jpeg)

<br>

### ※ 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면
  - 프로토타입 프로퍼티를 덮어씀 (X)
  - 인스턴스 프로퍼티로 추가 (O)  
<br>

### ** **오버라이딩이란?**
> ```상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식```

<br>

### ** **프로퍼티 섀도잉이란?**
> ```상속관계에 의해 프로퍼티가 가려지는 현상```

<br>

### ** **오버로딩이란?**
> ```함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식```  

<br>

``` jsx
// 인스턴스 메서드 삭제
delete me.sayHello();

// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello();        // Hi! My name is Lee

// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;

// 프로토타입 메서드가 호출된다.
me.sayHello();        // Hi! My name is Lee

// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

me.sayHello();        // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello();        // TypeError
```

### ```※ 하위 객체를 통해 프로토타입의 프로퍼티 변경/삭제하려면``` 
  - 프로토타입 체인으로 접근 (X)
  - ```프로토타입에 직접 접근 (O)```  
<br>

## 19.9 프로토타입의 교체
> ```프로토타입은 임의의 다른 객체로 변경 가능```  
> -> 부모 객체인 프로토타입을 동적으로 변경 가능하다는 의미 

<br>

### 1) 생성자 함수에 의한 프로토타입의 교체



### 2) 인스턴스에 의한 프로토타입의 교체

<br>

## 19.10 instanceof 연산자  
<br>

``` jsx
객체 instanceof 생성자 함수 
```
- 좌변 : 객체를 가리키는 식별자
- 우변 : 생성자 함수를 가리키는 식별자 (함수 아닌 경우 에러 발생)  
<br>

&ensp; -> 우변의 생성자 함수의 prototype 에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true 로 평가되고, 그렇지 않은 경우에는 false 로 평가됨. 

<br>

``` jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// Person.prototype 이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Person);      // true

// Object.prototype 이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Object);      // true
``` 

``` jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {};

// 프로토타입 교체 (Person -> parent)
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent);         // false
console.log(parent.constructor === Person);       // false

// Person.prototype 이 me 객체의 프로토타입 체인 상에 존재하지 않으므로 false
console.log(me instanceof Person);         // false

// Object.prototype 이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Object);         // true

// -------------------------------------------------------------

// ★★ parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩한다.
Person.prototype = parent;

// Person.prototype 이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Person);         // true

// Object.prototype 이 me 객체의 프로토타입 체인 상에 존재하므로 true
console.log(me instanceof Object);         // true
```
<br>

### ```※ instanceof 연산자는```
  - 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것 (X)

  - ```생성자 함수의 prototype 에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인 (O)```   
    - <em> me instanceof Person </em>     
    : me 객체의 프로토타입 체인 상에 Person.prototype 에 바인딩된 객체가 존재하는지 확인

    - <em> me instanceof Object </em>     
    : me 객체의 프로토타입 체인 상에 Object.prototype 에 바인딩된 객체가 존재하는지 확인  
<br>

## 19.11 직접 상속

### 1) Object.create 에 의한 직접 상속
> ```명시적으로 프로토타입을 지정하여 새로운 객체를 생성하면서 직접 상속 구현```

<br>

``` jsx
Object.create(매개변수1, 매개변수2)
```
- 매개변수 1 : 생성할 객체의 프로토타입으로 지정할 객체 

- 매개변수 2 : 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체 (생략 가능)

<br>

``` jsx
// 프로토타입이 null 인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj -> null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null);                // true

// Object.prototype 을 상속받지 못한다.
console.log(obj.toString());            // TypeError

// obj -> Object.prototype -> null
// obj = {}; 와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeof(obj) === Object.prototype);     // true

// obj -> Object.prototype -> null
// obj = { x : 1 }; 과 동일하다.
obj = Object.create(Object.prototype, {
  x : { value : 1, writable : true, enumerable : true, configurable : true } 
});

const myProto = { x : 10 };

// 임의의 객체를 상속받는다.
// obj -> myProto  -> Object.prototype -> null
obj = Object.create(myProto);
console.log(obj.x);         // 10
console.log(Object.getPrototypeOf(obj) === myProto);              // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj -> Person.prototype -> Object.prototype -> null
// obj = new Person('Lee') 와 동일하다.
obj = Object.create(Person.prototype);
obj.name = 'Lee';

console.log(obj.name);      // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype);     // true
```
<br>

### ```** Object.create 메서드의 장점```

- new 연산자 없이도 객체를 생성할 수 있다.

- 프로토타입을 지정하면서 객체를 생성할 수 있다.

- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

<br>

``` jsx
// 프로토타입이 null 인 객체 생성
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null);                 // true

// obj 는 Object.prototype 의 빌트인 메서드 사용 불가능
console.log(obj.hasOwnProperty('a'));                             // TypeError

// Object.prototype 의 빌트인 메서드는 객체로 직접 호출 X
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));      // true
```

<em> -> 프로토타입 체인의 종점에 위치하는 객체는 Object.prototype 의 빌트인 메서드를 간접적으로 호출해야 함. </em>   
<br>

### 2) 객체 리터럴 내부에서 __proto__ 에 의한 직접 상속
> ES6 에서는 객체 리터럴 내부에서 ```__proto__ 접근자 프로퍼티를 사용하여 직접 상속 구현```

<br>

``` jsx
const myProto = { x : 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다. 
const obj = {
  y : 20,
  // 객체를 직접 상속받는다. 
  // obj -> myProto -> Object.prototype -> null
  __proto__ : myProto;
};

console.log(obj.x, obj.y);         // 10 20
console.log(Object.getPrototypeOf(obj) === myProto);      // true
```
<br>

## 19.12 정적 프로퍼티/메서드
> ```생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드```

<br>

``` jsx

```


<br><br>
*이미지 출처 : https://www.howdy-mj.me/javascript/prototype-and-proto/  
https://velog.io/@rlatp1409/JS-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85-%EC%B2%B4%EC%9D%B8-%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%94%A9-%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-%EC%84%80%EB%8F%84%EC%9E%89