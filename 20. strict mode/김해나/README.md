# 20. strict mode
<br>

## 20.1 strict mode 란?
> 자바스크립트 언어의 문법을 좀 더 엄격히 적용해서 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 ```문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시키는 것.```

<br>

### ```** 린트 도구 (ESLint)```

-> strict mode 와 유사한 효과를 얻을 수 있는 도구  
-> 정적 분석 기능을 통해 소스 코드를 실행하기 전에 소스코드를 스캔하여 <em> 문법적 오류만이 아닌 잠재적 오류까지 찾아내어 오류의 원인을 리포팅함. </em>  
<br>

## 20.2 strict mode 의 적용
> 전역의 선두 또는 함수 몸체의 선두에 ```'use strict';``` 추가하여 사용   
> -> 코드의 선두에 위치시키지 않으면 제대로 동작 X 

<br>

``` jsx
// 전역 선두에 추가
'use strict';

function foo() {
  x = 10;     // ReferenceError
}
foo();

// 함수 몸체 선두에 추가
function foo() {
  'use strict';

  x = 10;     // ReferenceError
}
foo();

// 코드 선두에 위치시키지 않은 경우
function foo() {
  x = 10;     // 에러 X
  'use strict';
}
foo();
```

- 전역의 선두에 추가  
-> 스크립트 전체에 strict mode 적용  

- 함수 몸체의 선두에 추가  
-> 해당 함수와 중첩 함수에 strict mode 적용  

<br>

## 20.3 전역에 strict mode 를 적용하는 것은 피하자
> ```전역에 적용한 strict mode 는 스크립트 단위로 적용됨.```

<br>


``` html
<!DOCTYPE html>
<html>
<body>

  <script>
    'use strict';
  </script>

  <script>
    x = 1;      <!-- 에러 발생 X -->
    console.log(x)
  </script>

</body>
</html>
```

-> 스크립트 단위로 적용된 strict mode 는 다른 스크립트에 영향 주지 않고 <u> 해당 스크립트에 한정되어 적용됨. </u>  

-> strict mode 스크립트와 non-scrict mode 스크립트를 혼용하는 것은 오류 발생 위험이 있기 때문에 전역에 strict mode 를 적용하는 것은 바람직하지 않음.  
<br>

``` jsx
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
``` 

-> 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수 선두에 strict mode 적용하는 것이 오류 발생 ↓  
<br>

## 20.4 함수 단위로 strict mode 를 적용하는 것도 피하자
<br>

``` jsx
(function () {
  // non-strict mode
  var let = 10;       // 에러 X

  function foo() {
    'use strict';

    let = 20;         // SyntaxError
  }
  foo();
}()));
```

-> 함수 단위로 strict mode 를 적용할 수는 있지만 특정 함수에만 적용하는 것은 바람직하지 않으며, 모든 함수에 일일이 strict mode 적용하는 것도 번거로운 일.  

### ```-> strict mode 는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직함.```  
<br>

## 20.5 strict mode 가 발생시키는 에러
<br>

### - ```암묵적 전역 ``` 
&ensp; -> 선언하지 않은 변수를 참조하면 ReferenceError 발생

<br>

### - ```변수, 함수, 매개변수의 삭제```  
&ensp; -> delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생  

<br>

### - ```매개변수 이름의 중복```  
&ensp; -> 중복된 매개변수 이름 사용 시 SyntaxError 발생  

<br>

### -``` with 문의 사용```  
&ensp; -> with 문 사용 시 SyntaxError 발생

<br>

** with 문 : 전달된 객체를 스코프 체인에 추가  
-> 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지지만 성능/가독성 ↓   
<br>

## 20.6 strict mode 적용에 의한 변화
<br>

### 1) 일반 함수의 this

``` jsx
(function () {
  'use strict';

  function foo() {
    console.log(this);    // undefined
  }
  foo();
  
  function Foo() {
    console.log(this);    // Foo
  }
  new Foo();  
}());
```

```-> strict mode 에서 함수를 일반 함수로서 호출하면 this 에 undefined 가 바인딩됨.```  

-> 생성자 함수가 아닌 일반 함수 내부에서는 this 를 사용할 필요가 없기 때문이고, 이때 에러는 발생하지 않음.     
<br>

### 2) arguments 객체

``` jsx
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않음.
  console.log(arguments);     // { 0 : 1, length : 1 }
}(1));
```

```-> strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영 X```