## 김해나
<br>

## 9.1 타입 변환이란?
  
  <h2>1) 명시적 타입 변환 (타입 캐스팅)</h2>
💡 개발자가 의도적으로 값의 타입을 변환하는 것. <br><br>  

  ```jsx
  var x = 10; 
  var str = x.toString();         // 숫자 -> 문자열로 타입 캐스팅
  console.log(typeof str, str);   // string 10
  console.log(str)                // 10 -> 값 변경되는 것 x
  ```
<br>

  <h2>2) 암묵적 타입 변환 (타입 강제 변환)</h2>
    💡   개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 변환되는 것.

  ```jsx
  var x = 10;
  var str = 10 + ''      // 문자열 연결 연산자 +
  console.log(typeof str, str);    // string 10
  console.log(typeof x, x);        // number 10 -> 값 변경되는 것 x
  ```
<br>

&nbsp;&nbsp;=> 명시적 타입 변환/암묵적 타입 변환 모두 원시 값을 변경하는 것 X  
&nbsp;&nbsp;=> ```기존 원시 값을 사용해서 다른 타입의 새로운 원시 값을 생성```하는 것 O
<br><br>

## 9.2 암묵적 타입 변환
```jsx
// 피연산자가 모두 문자열 타입이어야 하는 문맥
'10' + 2     // '102'

//피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10'    // 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0          // true
if (1) {} 
```
<br>

* <h3>문자열 타입으로 변환

``` jsx
1 + '2'   // "12"
```
&nbsp;&nbsp; -> 연산자 '+' : 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작  
&nbsp;&nbsp; -> 문자열 연결 연산자의 피연산자 중 <u>문자열 타입이 아닌 피연산자를 문자열로 암묵적 타입 변환</u>  
<br>

```jsx 
`1 + 1 = ${1 + 1}`   // "1 + 1 = 2"
```

&nbsp;&nbsp; -> ES6에서 도입된 <U>템플릿 리터럴의 표현식 삽입</U>은 표현식의 평가 결과를 문자열 타입으로 암묵적 변환  
<br>

```jsx 
// 숫자 타입
0 + ''          // "0"
-0 + ''         // "0"
1 + ''          // "1"
-1 + ''         // "-1"
NaN + ''        // "NaN"
Infinity + ''   // "Infinity"
-Infinity + ''  // "-Infinity"

// 불리언 타입
true + ''       // "true"
false + ''      // "false"

// null 타입
null + ''       // "null"

// undefined 타입
undefined + ''  // "undefined"

// 심벌 타입
(Symbol()) + '' // TypeError : Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''       // [object Object]
Math + ''       // [object Math]
[] + ''         // ""
[10, 20] + ''   // "10, 20"
(function(){})  // "function(){}"
Array + ''      // "function Array() { [native code] }"
```
<br>

* <h3>숫자 타입으로 변환

```jsx
1 - '1'     // 0
1 * '10'    // 10
1 / 'one'   // NaN
```
&nbsp;&nbsp; -> 위 예제에서 사용한 연산자는 모두 <u>산술 연산자</u>  
&nbsp;&nbsp; -> 산술 연산자의 역할은 숫자 값을 만드는 것이기 때문에 <u>모든 피연산자는 숫자 타입</u>이어야 함.  
<br>

```jsx
'1' > 0    // true
```
&nbsp;&nbsp; -> 피연산자의 크기를 비교하는 연산자의 역할은 불리언 값을 만드는 것.  
&nbsp;&nbsp; -> <u>비교 연산자의 피연산자는 모두 숫자 타입</u>이어야 함.  
<br> 

```jsx
// 문자열 타입
+''          // 0
+'0'         // 0
+'1'         // 1
+'string'    // NaN

// 불리언 타입
+true        // 1
+false       // 0

// null 타입
+null        // 0

// undefined 타입
+undefined   // NaN

// 심벌 타입
+Symbol()    // TypeError : Cannot convert a Symbol value to a number

// 객체 타입
+{}               // NaN
+ []              // NaN
+[10, 20]         // NaN
+(function(){})   // NaN
```
&nbsp;&nbsp; -> <em>+ 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환 수행 </em>  
&nbsp;&nbsp; -> 빈 문자열(''), 빈 배열([]), null, false는 0으로, true는 1로 변환됨.  
&nbsp;&nbsp; -> 빈 배열이 아닌 배열, 객체, undefined는 변환되지 않기 때문에 NaN 반환  
<br>

* <h3>불리언 타입으로 변환
```jsx
if ('') console.log(x);
```

&nbsp;&nbsp; -> if문이나 for문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값으로 평가되어야 하는 표현식  
&nbsp;&nbsp; -> 자바스크립트 엔진은 <u>조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환</u>해야 함.  
<br> 

```jsx
if('') console.log('1');
if(true) console.log('2');
if(0) console.log('3');
if('str') console.log('4');
if(null) console.log('5');

// 출력 결과 : 2 4
```
&nbsp;&nbsp; -> 자바스크립트 엔진은 <em>불리언 타입이 아닌 값을 ```Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)으로 구분함.```  
<br>  

```jsx
if(!false) console.log(false + ' is falsy value');
if(!undefined) console.log(undefined + ' is falsy value');
if(!null) console.log(null + ' is falsy value');
if(!0) console.log(0 + ' is falsy value');
if(!NaN) console.log(NaN + ' is falsy value');
if(!'') console.log('' + ' is falsy value');
```  

- Truthy 값 ⇒ true (Falsy 이외의 값들)
- Falsy 값 ⇒ false 로 암묵적 타입 변환
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - ‘’ (빈 문자열)

<br> 

```jsx
// 예제 : Truthy / False 값을 판별하는 함수

// 전달받은 인수가 False 값이라면 true, Truthy 값이라면 false 반환
function isFalsy(v) {
  return !v;
}

// 전달받은 인수가 Truthy 값이라면 true, Falsy 값이라면 false 반환
function isTruthy(v) {
  return !!v;
}

// 모두 true 반환
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy('');    // -> 빈 문자열은 Falsy 값

// 모두 true 반환
isTruthy(true);
isTruthy('0');   // => 빈 문자열이 아닌 문자열은 Truthy 값
isTruthy({});
isTruthy(([]);)
```

- Truthy 값들의 종류  
  => Falsy 이외의 값들로, 빈 문자열이 아닌 문자열은 Truthy 값에 속함.  
<br>

## 9.3 명시적 타입 변환
> 1. 표준 빌트인 생성자 함수 (String, Number, Boolean) 를 new 연산자 없이 호출하는 방법 

> 2. 빌트인 메서드를 사용하는 방법  

> 3. 암묵적 타입 변환을 이용하는 방법

<br>

*<h3>💡 표준 빌트인 생성자 함수와 빌트인 메서드</h3>*
&nbsp;&nbsp; (자바스크립트에서 기본적으로 제공하는 함수)  
&nbsp;&nbsp; -> 표준 빌트인 생성자 함수는 객체를 생성하기 위한 함수이며 new 연산자와 함께 호출  
&nbsp;&nbsp; -> 표준 빌트인 메서드는 해당 빌트인 객체의 메서드를 의미함.  
<br>

* <h3>문자열 타입으로 변환   
```jsx
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 -> 문자열 타입
String(1);          // "1"
String(NaN);        // "NaN"
String(Infinity);   // "Infinity"

// 불리언 타입 -> 문자열 타입
String(true);       // "true"
String(false);      // "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 -> 문자열 타입
(1).toString();           // "1"
(Nan).toString();         // "NaN"
(Infinity).toString();    // "Infinity"

// 불리언 타입 -> 문자열 타입
(true).toString();        // "true"
(false).toString();       // "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 -> 문자열 타입
1 + '';           // "1"
NaN + '';         // "NaN"
Infinity + '';    // "Infinity"

// 불리언 타입 -> 문자열 타입
true + '';        // "true"
false + '';       // "false"
```
&nbsp; `1. String 생성자 함수를 new 연산자 없이 호출하는 방법`  
&nbsp; `2. Object.prototype.toString 메서드를 사용하는 방법`  
&nbsp; `3. 문자열 연결 연산자를 이용하는 방법`  
<br>
  
* <h3>숫자 타입으로 변환
```jsx
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 -> 숫자 타입
Number('0');          // 0
Number('-1');         // -1
Number('10.53');      // 10.53

// 불리언 타입 -> 숫자 타입
Number(true);         // 1
Number(false);        // 0

// 2. parseInt.parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
// 문자열 타입 -> 숫자 타입
parseInt('0');        // 0
parseInt('-1');       // -1
parseInt('10.53');     // 10.53

// 3. 단항 산술 연산자 + 를 이용하는 방법 (문자열 연결 연산자 x)
// 문자열 타입 -> 숫자 타입
+'0';        // 0
+'-1';       // -1
+'10.53';    // 10.53

// 불리언 타입 -> 숫자 타입
+true;      // 1
+false;     // 0

// 4. 산술 연산자 * 를 이용하는 방법
// 문자열 타입 -> 숫자 타입
'0' * 1;       // 0
'-1' * 1;      // -1
'10.53' * 1;   // 10.53

// 불리언 타입 -> 숫자 타입
true * 1;      // 1
false * 1;     // 0
```

&nbsp; `1. Number 생성자 함수를 new 연산자 없이 호출하는 방법`  
&nbsp; `2. parseInt.parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)`  
&nbsp; `3. 단항 산술 연산자 + 를 이용하는 방법`  
&nbsp; `4. 산술 연산자 * 를 이용하는 방법`  
<br>

* <h3>불리언 타입으로 변환
```jsx
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 -> 불리언 타입
Boolean('');        // false -> 빈 문자열은 false
Boolean('x');       // true  -> 빈 문자열이 아닌 문자열은 true
Boolean('false');   // true

// 숫자 타입 -> 불리언 타입
Boolean(0);         // false
Boolean(1);         // true
Boolean(NaN);       // false
Boolean(Infinity);  // true

// null 타입 -> 불리언 타입
Boolean(null);        // false

// undefined 타입 -> 불리언 타입
Boolean(undefined);   // false

// 객체 타입 -> 불리언 타입
Boolean({});          // true
Boolean([]);          // true


// 2.  부정 논리 연산자 ! 를 2번 사용하는 방법
// 문자열 타입 -> 불리언 타입
!!'x';          // true
!!'';           // false
!!'false';      // true

// 숫자 타입 -> 불리언 타입
!!0;            // false
!!1;            // true
!!NaN;          // false
!!Infinity;     // true

// null 타입 -> 불리언 타입
!!null;         // false

// undefined 타입 -> 불리언 타입
!!undefined;    // false

// 객체 타입 -> 불리언 타입
!!{};           // true
!![];           // true
```
&nbsp; `1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법`  
&nbsp; `2. 부정 논리 연산자 ! 를 2번 사용하는 방법`  
<br>

## 9.4 단축 평가
<h3>1. 논리 연산자를 사용한 단축 평가</h3>

&nbsp; -> <em> 논리합 (||) 또는 논리곱 (&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다.  
&nbsp; -> 논리합 (||) 또는 논리곱 (&&) 연산자 표현식은 언제나 <u>2개의 피연산자 중 어느 한쪽으로 평가</u>된다. </em>  
<br>

```jsx
// 1번 표현식 (논리곱 연산자 &&)
'Cat' && 'Dog'    // 'Dog'

// 2번 표현식 (논리합 연산자 ||)
'Cat' || 'Dog'    // 'Cat'
```  

&nbsp; <h3>💡 논리곱(&&) 연산자 : ```두 개의 피연산자 모두 true 로 평가되면 true 반환```</h3>
-> 'Cat' 은 Truthy 값이므로 true 로 평가되지만 두 개의 피연산자 모두 true 여야만 결과값이 반환되기 때문에 이 시점까지는 1번 표현식을 평가할 수 없음.   
-> 즉, 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정함.   
-> <u>논리 연산의 결과를 결정한 2번째 피연산자 문자열인 'Dog' 그대로 출력</u>

&nbsp; <h3>💡 논리합(||) 연산자 : ```두 개의 피연산자 중 하나라도 ture 로 평가되면 true 반환 ```</h3>
-> 마찬가지로 'Cat' 은 Truthy 값이므로 true 로 평가되는데, 논리합 연산자는 2개의 피연산자 중 하나만 true 여도 결과값이 반환됨.  
-> 두 번째 피연산자까지 평가해 보지 않아도 이 시점에서 바로 2번 표현식을 평가할 수 있음.  
-> <u>논리 연산의 결과를 결정한 1번째 피연산자 문자열인 'Cat' 그대로 출력 </u>

`* 단축 평가 *`  
&nbsp; : <em> 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것.</em>  
&nbsp;&nbsp; -> 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 변환  
<br>

|단축 평가 표현식 | 평가 결과 |
| --- | ---|
| `true \|\| anything` | true |
| `false \|\| anything` | anything |
| `true && anything` | anything |
| `false && anything` | false |  
<br>

``` jsx
var done = true;
var message = '';

// 주어진 조건이 true 일 때
if (done) message = '완료';

// 단축 평가로 위 if 문 대체 가능
// -> done 이 true 라면 message 에 '완료' 할당
message = done && '완료';
console.log(message);    // 출력 결과 : 완료
``` 
&nbsp; -> 조건이 Truthy 값일 때 무언가를 해야 한다면 논리곱 (&&) 연산자 표현식으로 if 문 대체 가능  
<br>

``` jsx
var done = false;
var message = '';

// 주어진 조건이 false 일 때
if (!done) message = '미완료';

// 단축 평가로 위 if 문 대체 가능
// -> done 이 false 라면 message 에 '미완료' 할당
message = done || '미완료';
console.log(message)      // 출력 결과 : 미완료
```
&nbsp; -> 조건이 Falsy 값일 때 무언가를 해야 한다면 논리합 (||) 연산자 표현식으로 if 문 대체 가능  
<br>

``` jsx
var done = true;
var message = '';

// if-else 문
if (done) message = "완료";
else      message = "미완료";
console.log(message);

// 삼항 조건 연산자로 위 if-else 문 대체 가능
message = done ? '완료' : '미완료';
console.log(message);     // 출력 결과 : 완료
```
&nbsp; -> 삼항 조건 연산자는 if-else 문으로 대체 가능
<br><br>  


<h3>2. 옵셔널 체이닝 연산자</h3>

> ES11 에서 도입된 옵셔널 체이닝 연산자 '?.' 는 좌항의 피연산자가 null 또는 undefined 인 경우 undefined 를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어감. <br>  
  
<br>

``` jsx
var elem = null;
var value = elem?.value;
console.log(value);   // undefined
```

&nbsp; => <em>객체를 가리키기를 기대하는 변수가 null 또는 undefined 가 아닌지 확인하고 프로퍼티를 참조할 때 유용 </em>


```jsx
// 옵셔널 체이닝 연산자 도입되기 전 -> 논리 연산자 && 를 이용하여 변수가 null 또는 undefined 인지 확인 (단축 평가)
// 단점 : 좌항 피연산자가 Falsy 값이면 좌항 피연산자 그대로 반환하게 되는데, Falsy 값인 0이나 '' 는 객체로 평가될 때도 있음.

var str = '';
var length = str && str.length;
console.log(length);    // '' (length 를 참조하지 못함)

// 옵셔널 체이닝 연산자 도입 후 
// 좌항의 피연산자가 Falsy 값이더라도 null 또는 undefined 가 아니면 우항의 프로퍼티 참조를 이어감.
var str = '';
var length = str?.length;
console.log(length);    // 0 (length 참조해서 '' 길이인 0 반환)
```
<br>  

<h3>3. null 병합 연산자</h3>

> ES11 에서 도입된 null 병합 연산자 '??' 는 좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를, 그렇지 않으면 좌항의 피연산자를 반환함. <br>

<br>

```jsx
var foo = null ?? 'default string';
console.log(foo);     // 'default string'
```

&nbsp; => <em>변수에 기본값 설정할 때 유용 </em>  
<br>

```jsx
// null 병합 연산자 도입되기 전 -> 논리 연산자 || 를 이용하여 변수에 기본값 설정 (단축 평가)
// 단점 : Falsy 값인 0이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있음.

var foo = '' || 'default string';
console.log(foo);     // 'default string'


// null 병합 연산자 도입 후 
// 좌항의 피연산자가 Falsy 값('') 이더라도 null 또는 undefined 가 아니기 때문에 좌황의 피연산자 반환

var foo = '' ?? 'default string';
console.log(foo);     // ""
```