# 11. 원시값과 객체의 비교
<br>

<h3>💡 원시 타입의 값과 객체 타입의 값의 차이점</h3>

- <h3> 원시 타입의 값</h3>  
1) 변경 불가능한 값   
2) 원시 값을 변수에 할당하면 변수 (확보된 메모리 공간) 에는 실제 값이 저장됨.  
3) 값에 의한 전달 : 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달됨.  
<br>

- <h3> 객체 타입의 값 </h3>
1) 변경 가능한 값  
2) 객체를 변수에 할당하면 변수 (확보된 메모리 공간) 에는 참조 값이 저장됨.  
3) 참조에 의한 전달 : 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달됨.

## 11.1 원시 값
변경 불가능한 값 => 한번 생성된 원시 값은 읽기 전용값으로서 변경할 수 없음.  
<br>

```jsx
// const 키워드를 사용하여 선언한 변수는 재할당이 금지됨. 
const o = {};

// const 키워드를 사용하여 선언한 변수에 할당한 원시 값 (상수) 은 변경 X
// But, const 키워드를 사용하여 선언한 변수에 할당한 객체는 변경 O
o.a = 1;
console.log(o);   // {a : 1}
```  
<br>

![df](https://blog.kakaocdn.net/dn/bGnhFj/btrdpqzpKIE/DAs9hoDetuuljcQkhRPCk1/img.jpg)

<h3> 원시 값을 할당한 변수에 새로운 원시 값을 재할당하면 </h3>

- 메모리 공간에 저장되어 있는 재할당 이전의 원시 값을 변경하는 것. (X)

- 새로운 메모리 공간을 확보하고 재할당 원시 값을 저장한 후, 변수는 새롭게 재할당한 원시 값을 가리킴. (O)

&ensp;&ensp; -> 즉,  '원시 값 변경이 불가능하다' 는 말은 <u> 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것이 아님. </u>   

&ensp;&ensp; -> 이러한 특성을 <em> '불변성 ' </em> 이라고 하고, 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없음.    
<br>

```jsx
// 자바스크립트의 문자열 또한 원시 타입이기 때문에 변경 불가능
var str = "Hello";
str = 'world!'

// 문자열은 유사 배열 객체이면서 이터러블이므로 배열과 유사하게 인덱스를 통해 각 문자에 접근 가능
console.log(str[1]);      // e

// 이미 생성된 문자열의 일부 문자를 변경해도 반영 X 
// (새로운 문자열 재할당은 가능)
str[2] = 'T';
console.log(str)          // Hello
```

-> 첫 번째문이 실행되면서 식별자 str 은 문자열 'Hello' 가 저장된 첫 번째 메모리 셀 주소를 가리킴.  

-> 두 번째 문이 실행되면서 새로운 문자열 'world' 를 메모리에 생성하고 식별자 str 은 이것을 가리키게 됨.  

-> 'Hello' 와 'world' 모두 메모리에 존재하지만, 식별자 str 이 'Hello' 를 가리키다가 'world' 를 가리키도록 변경되었을 뿐임.    
<br>

```jsx
// copy 변수에는 score 변수의 값 80 이 복사되어 전달됨.
var score = 80;
var copy = score;

console.log(score, copy);           // 80 80
console.log(score === copy);        // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값
// -> score 변수의 값을 변경해도 copy 변수의 값은 영향 받지 않음.
score = 100;
console.log(score,  copy);         // 100, 80
console.log(score === copy);       // false
```

<h3> *변수에 변수를 할당했을 때 </h3> 

![img](https://blog.kakaocdn.net/dn/7e6OS/btrdo862oUX/lnv8cYKhqCsKU6NBs3mKV0/img.jpg)

 <h3> -> '값에 의한 전달' </h3>
&ensp;&ensp; : 변수에 원시 값을 갖는 변수를 할당하면 할당받는 변수 (copy) 에는  할당되는 변수 (score) 의 원시 값이 복사되어 전달됨.     
<br><br>
  
> <h3><em >두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값으로서, 한쪽에서 재할당을 통해 값을 변경하더라도 서로 간섭할 수 없음. </em>  
<br>  

## 11.2 객체

변경 가능한 값 => 객체 (참조) 타입의 값은 원시 타입과 달리 변경 가능
<br>

```jsx
// 할당이 이루어지는 시점에 객체 리터럴이 해석되고, 그 결과 객체가 생성됨.
var person = {
  name : 'Lee'
};

// person 변수에 저장되어 있는 참조 값으로 실제 객체에 접근함.
console.log(person);    // {name : 'Lee'}
```
-> 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값에 접근 가능  

-> <em> 참조 값은 생성된 객체가 저장된 메모리 공간의 주소, 그 자체를 의미하기 때문에 변수는 이 참조 값을 통해 객체에 접근 가능 </em>    
<br><br>

![img](https://blog.kakaocdn.net/dn/dKRoZe/btrdr4hPpQq/eshJuNbTRN6Sb0FGIwVvu1/img.jpg)

-> 원시 값을 할당한 변수를 참조하면 메모리에 저장되어 있는 원시 값에 접근  

-> But, 객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 참조 값을 통해 실제 객체에 접근  
<br>

```jsx
var person = {
  name : 'Lee'
};

// 프로퍼티 값 갱신 및 추가 생성
person.name = 'Kim';
person.address = 'Seoul';

console.log(person);      // {name : 'Kim', address : 'Seoul'}
```

-> 객체는 변수와 다르게 재할당 없이도 프로퍼티를 동적으로 추가 / 갱신 / 삭제 가능 (변경 가능)   
-> 생겨난 부작용 : 원시 값과는 다르게 <u>여러 개의 식별자가 하나의 객체를 공유할 수 있게 됨. </u>  
<br>  

```jsx
var person = {
  name : 'Lee'
};

// 참조 값을 복사 (얕은 복사)
// -> copy 와 person 은 동일한 참조 값을 가짐.
var copy = person;

// -> 원본 person 과 사본 copy 는 저장된 메모리 주소는 다르지만 동일한 참조 값을 가짐.
// -> 즉, 원본 person 과 사본 copy 모두 동일한 객체를 가리킨다는 것. 
// (두 개의 식별자가 하나의 객체를 공유)

// copy 와 person 은 동일한 객체를 참조함.
console.log(copy === person);     // true

// copy 를 통해 객체 변경
copy.name = 'Kim';

// person 을 통해 객체 변경
person.address = 'Seoul';

// 동일한 객체를 참조하기 때문에 어느 한쪽이라도 객체를 변경하면 서로 영향을 주고받음.
console.log(person);        // {name : 'Kim', address  : 'Seoul'}
console.log(copy);          // {name : 'Kim', address  : 'Seoul'}
```
<br>

![img2](https://blog.kakaocdn.net/dn/uAhPs/btrdnjAPqZp/gZtXVt3JeN9MvFXUjkDMNK/img.jpg)

<h3> -> '참조에 의한 전달' </h3>

&ensp;&ensp; : 객체를 가리키는 변수 (원본, person) 를 다른 변수 (사본, copy) 에 할당하면 <em> 원본의 참조 값이 복사되어 전달됨. </em>
<br>

> <h3><em> 참조 값이 복사되어 전달되기 때문에 원본 또는 사본 중 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받게 됨. </em>  
<br>

```jsx 
var person1 = {
  name : 'Lee'
};

var person2 = {
  name : 'Lee'
};

console.log(person1 === person2);               // false
console.log(person1.name === person2.name);     // true
```
-> 일치 비교 연산자 (===) 를 통해 객체를 할당한 변수일 땐 참조값을 비교, 원시 값을 할당한 변수일 땐 원시 값 비교  

-> 객체 리터럴은 평가될 때마다 객체를 생성하기 때문에 person1 변수와 person2 변수가 가리키는 객체는 다른 메모리에 저장된 별개의 객체이기 때문에 false  

-> 프로퍼티 값을 참조하는 person1.name 과 person2.name 은 값으로 평가될 수 있는 표현식인데, 두 표현식 모두 원시 값 'Lee' 로 평가되기 때문에 true 
<br><br>
** 이미지 출처 : https://tooo1.tistory.com/366?category=960189