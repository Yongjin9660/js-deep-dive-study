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

-> 함수 객체는 일반 객체에는 없는 함수 고유의 프로퍼티 소유  
<br>

## 18.2 함수 객체의 프로퍼티

** 함수도 객체이기 때문에 프로퍼티를 가질 수 있음.

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

// __proto__ 는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptors(square, '__proto__'));      // undefined

// __proto__ 는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get : f, set : f, enumerable : false, configurable : true}
```

-> ``` arguments, caller, length, name, prototype 프로퍼티``` 는 일반 객체에는 없는 ```함수 객체 고유의 데이터 프로퍼티```

### 1) arguments 프로퍼티

### 2) caller 프로퍼티

### 3) length 프로퍼티

### 4) name 프로퍼티

### 5) __proto__ 접근자 프로퍼티

### 6) prototype 프로퍼티