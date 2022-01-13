# 27. 배열
<br>


## 27.1 배열이란?
> 여러 개의 값을 순차적으로 나열한 자료구조  

<br>

``` jsx
const arr = ['apple', 'banana', 'orange'];

arr[0]          // apple
arr.length      // 3

typeof arr;     // object
```

- 요소 : 배열이 가지고 있는 값  
-> 자바스크립트의 모든 값은 배열의 요소가 될 수 있음.  
-> 배열의 요소는 배열에서 자신의 위치를 나타내는 0 이상의 정수인 ```인덱스```를 가짐.

- 배열은 요소의 개수, 즉 배열의 길이를 나타내는 ``` length 프로퍼티 ``` 를 가짐.


- 배열은 ``` 객체 타입``` (배열이라는 타입은 존재하지 않음)  

<br>

``` ** 배열과 일반 객체의 구별되는 특징  ```
<BR>

| 구분 | 객체 | 배열 |
| --- | --- | --- |
| 구조 | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조 | 프로퍼티 키 | 인덱스 |
| 값의 순서 | X | O |
| length 프로퍼티 | X | O |

-> 일반 객체와 배열을 구분하는 가장 명확한 차이는 ```'값의 순서'``` 와 ```'length 프로퍼티'```     
<br>

``` jsx 
const arr = [1, 2, 3];

// 배열의 순회
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);    // 1 2 3 
}
```
- 배열은 처음부터 순차적으로 요소에 접근할 수도 있고, 마지막부터 역순으로 요소에 접근할 수도 있으며, 특정 위치부터 순차적으로 요소에 접근하는 것도 가능함.  

&ensp; &ensp; ``` -> 배열이 인덱스, 즉 '값의 순서'와 'length 프로퍼티'를 갖기 때문 ```   
<br>

## 27.2 자바스크립트 배열은 배열이 아니다
<br>

### 1) 밀집 배열
> 배열의 요소가 ``` 하나의 데이터 타입으로 통일 ```되어 있으며, ``` 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 인접 ``` 해 있는 배열  

<br>

![img](https://media.vlpt.us/images/nareum/post/18550694-1e55-4eec-a54d-e4c7ccce76b5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.53.58.png)

<br>

```
검색 대상 요소의 메모리 주소 = 배열의 시작 메모리 주소 + 인덱스 * 요소의 바이트 수

ex) 인덱스가 0인 요소의 메모리 주소 : 1000 + 0 * 8 = 1000
    인덱스가 1인 요소의 메모리 주소 : 1000 + 1 * 8 = 1008
```  

-> <u>단 한 번의 연산</u>으로 임의의 요소에 접근 (임의 접근, 시간 복잡도 O(1)) 할 수 있기 때문에 매우 효율적이며 <u> 고속으로 동작</u>함.  
<br>

``` jsx
// 선형 검색을 통해 배열 (array) 에 특정 요소 (target) 가 존재하는지 확인
// 배열에 특정 요소가 존재하면 특정 요소의 인덱스를 반환, 존재하지 않으면 -1 반환
function linearSearch(array, target) {
  const length = array.length;

  for (let i = 0; i < length; i++) {
    if (array[i] === target) return i;
  }
  return -1;
}

console.log(linearSearch([1, 2, 3, 4, 5, 6], 3));     // 2
```  

-> But, 정렬되지 않은 배열에서 특정 요소를 검색하는 경우 배열의 모든 요소를 처음부터 특정 요소를 발견할 때까지 차례대로 검색해야 한다는 단점 O  
<br>

![img](https://poiemaweb.com/assets/fs-images/27-2.png)  

<br>

-> 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 하는 단점 O  
<br>

### 2) 희소 배열
> 배열의 요소를 위한 ``` 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 요소들이 연속적으로 이어져 있지 않은 배열 ```  

> ★ 자바스크립트 배열은 일반적 의미의 배열이 아닌 희소 배열  

<br>

``` jsx
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));

/*
{
  '0' : {value : 1, writable : true, enumerable : true, configurable : true}
  '1' : {value : 2, writable : true, enumerable : true, configurable : true}
  '2' : {value : 3, writable : true, enumerable : true, configurable : true}
  length : {value : 3, writable : true, enumerable : false, configurable : false}}
}
*/
```

### ``` -> 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체 ``` ###  

<br>

``` jsx
const arr = [
  'string', 
  10,
  true,
  null,
  undefined,
  NaN,
  Infinity,
  [ ],
  { },
  function () {}
];
```

-> 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 <u> 어떤 타입의 값이라도 배열의 요소가 될 수 있음. </u>  
<br>

``` ** 일반적인 배열과 자바스크립트 배열의 장단점 ** ``` 
- 일반적인 배열은 인덱스로 <u>요소에 빠르게 접근</u>할 수 있다. 하지만 특정 요소를 검색하거나 요소를 삽입/삭제하는 경우에는 효율적이지 않다.  

- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수밖에 없는 구조적인 단점이 있다. 하지만 특정 요소를 검색하거나 요소를 삽입/삭제하는 경우에는 일반적인 배열보다 <u>빠른 성능</u>을 기대할 수 있다.  
<br>

## 27.3 length 프로퍼티와 희소 배열

> length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 ``` 자동 갱신 ```됨.  
> length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 ``` 임의의 숫자 값을 명시적으로 할당할 수도 있음. ``` 

<br>

``` jsx
const arr = [1, 2, 3, 4, 5];

arr.length = 3;

console.log(arr.length);   // 3
console.log(arr);          // [1, 2, 3]
```

- 현재 length 프로퍼티 값보다 작은 숫자 값을 할당한 경우  
=> 배열의 길이가 줄어듦  
<br>

``` jsx
const arr = [1];
arr.length = 3;

console.log(arr.length);    // 3
console.log(arr);           // [1, empty * 2] -> arr[1] 과 arr[2] 에는 값 존재 X
```

- 현재 length 프로퍼티 값보다 큰 숫자 값을 할당한 경우  
=> length 프로퍼티 값은 변경되지만 실제 배열 길이가 늘어나지는 않음.  
=> 값 없이 비어 있는 요소를 위해 메모리 공간을 확보하지 않으며, 빈 요소를 생성하지도 않음.  
<br>  

``` jsx
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않음.
console.log(sparse.length);     // 4
console.log(sparse);            // [empty, 2, empty, 4]

// 배열 sparse 에는 인덱스가 0, 2 인 요소가 존재하지 않음.
console.log(Object.getOwnPropertyDescriptors(sparse));

/*
  '1' : {value : 2, writable : true, enumerable : true, configurable : true}
  '3' : {value : 4, writable : true, enumerable : true, configurable : true}
  length : {value : 4, writable : true, enumerable : false, configurable : false}}
*/
```

- ### 일반적인 배열의 length 는 배열 요소의 개수, 즉 배열의 길이와 언제나 일치함.

- ### 희소 배열의 length 는 희소 배열의 실제 요소 개수보다 언제나 큼.  
<br>

=> 자바스크립트는 문법적으로 희소 배열을 허용하지만 ``` 희소 배열은 사용하지 않는 것이 좋음. ```  

=> 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선  
<br>

## 27.4 배열 생성
<br>

### 1) 배열 리터럴
<br>

``` jsx
const arr1 = [1, 2, 3];
console.log(arr1.length);      // 3

const arr2 = [];               // 빈 배열 생성
console.log(arr2.length);      // 0

const arr3 = [1, , 3];         // 희소 배열 (언제나 length > 실제 요소 개수)
console.log(arr3.length);      // 3
console.log(arr3);             // [1, empty, 3]
console.log(arr3[1]);          // undefined
```

-> 배열 리터럴은 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재하며, 0개 이상의 요소를 쉼표로 구분하여 대괄호 ([]) 로 묶음.

-> 배열 리터럴에 요소를 하나도 추가하지 않으면 length 프로퍼티 값이 0인 빈 배열이 됨.  

``` -> 배열 리터럴에 요소를 생략하면 희소 배열이 생성됨. ```  
<br>

### 2) Array 생성자 함수
<br>

``` jsx
new Array();      // [] -> 빈 배열 생성 (배열 리터럴 [] 과 동일)

// 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열 생성
new Array(1, 2, 3);     // [1, 2, 3]

// 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열 생성
new Array({});          // [{}]
```

``` -> Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작함. ```  

- 전달된 인수가 없는 경우 빈 배열 생성  

- 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열 생성   
<br>

```jsx
const arr = new Array(10);

console.log(arr);             // [empty * 10] -> 희소 배열 생성
console.log(arr.length);      // 10


// 배열은 최대 2^32 - 1 개의 요소를 가질 수 있음.
new Array(4294967295);        

new Array(4294967296);        // RangeError
new Array(-1);                // RangeError
```

-> Array 생성자 함수에 전달할 인수는 <em> 0 또는 2^32 - 1 이하인 양의 정수</em> 이어야 함.  
<br>

### 3) Array.of 메서드
<br>

``` jsx
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열 생성
Array.of(1);            // [1]

Array.of(1, 2, 3);      // [1, 2, 3]

Array.of('String');     // ['String']
```

``` -> ES6 에서 도입된 Array.of 메서드는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열 생성 ```  
<br>

### 4) Array.from 메서드
<br>

``` jsx
// 유사 배열 객체를 변환하여 배열 생성
Array.from({'length' : 2, 0 : 'a', 1 : 'b'});     // ['a', 'b']

// 이터러블을 변환하여 배열 생성 (문자열은 이터러블)
Array.from('Hello');        // ['H', 'e', 'l', 'l', 'o']
```

``` -> ES6 에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환함. ```  
<br>

``` jsx
// Array.from 에 length 만 존재하는 유사 배열 객체를 전달하면 undefined 를 요소로 채움.
Array.from({length : 3});       // [undefined, undefined, undefined]

// Array.from 은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열 반환
Array.from({length : 3}, (_, i) => i);      // [0, 1, 2]
```

-> Array.from 을 사용하면 ``` 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있음. ```  

-> 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환  
<br>

### ** 유사 배열 객체와 이터러블 객체 **
- 유사 배열 객체

``` jsx
// 유사 배열 객체
const arrayLike = {
  '0' : 'apple', 
  '1' : 'banana', 
  '2' : 'orange',
  length : 3
};

// 유사 배열 객체는 마치 배열처럼 for 문으로 순회 가능
for (let i = 0; i < arrayLike.length; i++) {
  console.log(arrayLike[i]);        // apple banana orange
}
```

-> 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체를 ``` '유사 배열 객체' ``` 라고 부름.  
<br>

- 이터러블 객체

-> Symbol.iterator 메서드를 구현하여 for... of 문으로 순회 가능하고, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있는 객체를 ``` '이터러블 객체'``` 라고 부름.

-> ES6 에서 제공하는 빌트인 이터러블은 Array, String, Map, Set, DOM 컬렉션, arguments 등이 있음.  
<br>

## 27.5 배열 요소의 참조
<br>

``` jsx
const arr1 = [1, 2];

// 인덱스가 1인 요소를 참조
console.log(arr1[1]);      // 2

// 인덱스가 2인 요소를 참조 -> 존재 X
cnosole.log(arr1[2]);      // undefined

// 희소 배열
const arr2 = [1, , 3];

// 배열 arr2 에는 인덱스가 1인 요소 존재 X
console.log(arr[1]);       // undefined
```

-> <em> 배열 </em> 은 ```인덱스를 나타내는 문자열을 프로퍼티 키로 갖는 객체``` 이기 때문에 존재하지 않는 요소를 참조하면 undefined 반환  
<br>

## 27.6 배열 요소의 추가와 갱신
<br>

- 배열 요소의 추가

``` jsx
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr);            // [0, 1]
console.log(arr.length);     // 2

arr[100] = 100;

console.log(arr);             // [0, 1, empty * 98, 100]
console.log(arr.length);      // 101
```

-> ``` 존재하지 않는 인덱스에 값을 할당하면 새로운 요소가 추가됨.``` 이때 length 프로퍼티 값은 자동으로 갱신됨.   

-> 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 됨. 이때 인덱스로 요소에 접근하여 값을 할당하지 않은 요소는 생성되지 않음.  
<br>

- 배열 요소의 갱신

``` jsx
const arr = [1, 2, 3];

// 요소값의 갱신
arr[1]= 10;

console.log(arr);             // [1, 10, 3]
```

-> ``` 이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신됨.```  
<br>

``` jsx
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr);             // [1, 2, foo : 3, bar : 4, 1.1 : 5, -1 : 6]

// 프로퍼티는 length 에 영향 X
console.log(arr.length);      // 2
```

-> ``` 인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수 (또는 정수 형태의 문자열) 를 사용해야 함. ```  
 
-> 정수 이외의 값을 인덱스처럼 사용하면 요소가 아닌 프로퍼티가 생성됨. 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않음.  
<br>

## 27.7 배열 요소의 삭제
<br>

- delete 연산자

``` jsx
const arr = [1, 2, 3];

// 배열 요소 삭제
delete arr[1];
console.log(arr);             // [1, empty, 3]

// length 프로퍼티에 영향을 주지 않음. 즉, 희소 배열이 됨.
console.log(arr.length);      // 3
```

-> ``` delete 연산자 ```를 사용하여 배열의 특정 요소 삭제  

-> 이때 length 프로퍼티 값은 변하지 않기 때문에 희소 배열이 되므로 delete 연산자는 되도록 사용하지 않는 것이 좋음.  
<br>

- splice 메서드

``` jsx
const arr = [1, 2, 3];

// arr[1] 부터 1개의 요소 제거
arr.splice(1, 1);
console.log(arr);           // [1, 3]

// length 프로퍼티가 자동 갱신됨.
console.log(arr.length;)    // 2
```

``` -> 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드 사용 ```

-> Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)  
<br>

## 27.8 배열 메서드

- 원본 배열을 직접 변경하는 메서드

- 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드  
<br>

### 1) Array.isArray 

> Array 생성자 함수의 정적 메서드  
> -> ``` 전달된 인수가 배열이면 true, 배열이 아니면 false 반환```

<br>

``` jsx
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({0 : 1, length : 1});
```
<br>

### 2) Array.prototype.indexOf

> ``` 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스 반환 ```  
> -> 원본 배열에 인수로 전달한 요소와 중복되는 요소가 있다면 첫 번째로 검색된 요소의 인덱스 반환  
> -> 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1 반환

<br>

``` jsx
// indexOf 메서드 사용한 경우 (부수 효과 O)

const arr = [1, 2, 2, 3];

// 배열 arr 에서 요소 2 검색 -> 인덱스 번호 1 반환
arr.indexOf(2);       // 1

// 배열 arr 에서 요소 4 검색 -> 없으므르 -1 반환
arr.indexOf(4);       // -1

// 두 번째 인수는 검색을 시작할 인덱스
arr.indexOf(2, 2);    // 2
```

``` jsx
// Array.prototype.includes 메서드 사용한 경우 (가독성 ↑)

const foods = ['apple', 'banana', 'orange'];

if (!foods.includes('orange')) {
  foods.push('orange')
}

console.log(foods);     // ['apple', 'banana', 'orange']
``` 

- indexOf 메서드 대신 ES7 에서 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋음.    
-> includes 메서드는 요소 존재 유무에 따라 true/false 반환   
<br>

### 3) Array.prototype.push

> ``` 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값 반환 ```  
> -> push 메서드는 원본 배열을 직접 변경  

<br>

``` jsx
// push 메서드 사용한 경우

const arr = [1, 2];

// 배열의 마지막 요소로 추가하고, 변경된 length 값 반환
let result = arr.push(3, 4);
console.log(result);      // [1, 2, 3, 4]

// 원본 배열에 영향 O
console.log(arr);         // [1, 2, 3, 4]
```

``` jsx
// length 프로퍼티를 사용한 경우 (속도 ↑)

const arr = [1, 2];

// arr.push(3) 과 동일한 처리하지만 이 방법이 push 메서드보다 빠름
arr[arr.length] = 3;
console.log(arr);         // [1, 2, 3]
```

-> push 메서드 대신 length 프로퍼티를 사용하여 배열 마지막에 요소 추가하는 방법이 더 빠름.  
<br>

``` jsx
// 스프레드 문법 사용한 경우 (부수 효과 X)
const arr = [1, 2];

const newArr = [...arr, 3];
console.log(newArr);
```

-> ES6 에서 도입된 스프레드 문법은 함수 호출없이 표현식으로 마지막에 요소를 추가할 수 있으며 push 메서드와 달리 부수 효과도 없음.  
<br>

### 4) Array.prototype.pop

> ``` 원본 배열에서 마지막 요소를 제거하고 제거한 요소 반환 ```  
> -> pop 메서드는 원본 배열을 직접 변경  

<br>

``` jsx
const arr = [1, 2];

let result = arr.pop();
console.log(result);        // 2


// 원본 배열에 영향 O
console.log(arr);          // [1]
```
<br>

### ``` ** push 메서드와 pop 메서드를 통해 스택 구현 ```  
<br>

- 스택 : 후입 선출 (LIFO - Last In First Out) 방식의 자료구조  
<br>

![img](https://miro.medium.com/max/563/1*Bh6Vm9QFj7PfCRw1_0r8Tw.png)

<br>

``` jsx
// 생성자 함수로 구현한 스택

const Stack = function() {
  function Stack(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  Stack.prototype = {
    constructor : Stack,

    // 스택의 가장 마지막에 데이터 밀어 넣기
    push(value) {
      return this.array.push(value);
    },

    // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터 꺼내기
    pop() {
      return this.array.pop();
    },

    // 스택의 복사본 배열 반환
    entries() {
      return [...this.array];
    }
  };

  return Stack;
}());

const stack = new Stack([1, 2]);
console.log(stack.entries());     // [1, 2]

stack.push(3);  
console.log(stack.entries());     // [1, 2, 3]

stack.pop();
console.log(stack.entries());     // [1, 2]
```

``` jsx
// 클래스로 구현한 스택

class Stack {
  #array;

  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 스택의 가장 마지막에 데이터 밀어 넣기
  push(value) {
    return this.#array.push(value);
  }
  
  // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터 꺼내기
  pop() {
    return this.#array.pop();
  }

  // 스택의 복사본 배열 반환
  entries() {
    return [...this.#array];
  }
}

const stack = new Stack([1, 2]);
console.log(stack.entries());       // [1, 2]

stack.push(3);  
console.log(stack.entries());       // [1, 2, 3]

stack.pop();
console.log(stack.entries());       // [1, 2]
```
<br>

### 5) Array.prototype.unshift

> ``` 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값 반환 ```  
> -> unshift 메서드는 원본 배열을 직접 변경

<br>

``` jsx
// unshift 메서드 사용한 경우 (부수 효과 O)

const arr = [1, 2];

let result = arr.unshift(3, 4);
console.log(result);        // 4

// 원본 배열에 영향 O
console.log(arr);          // [3, 4, 1, 2]
```

``` jsx
// 스프레드 문법 사용한 경우 (부수 효과 X)

const arr = [1, 2];

const newArr = [3, ...arr];
console.log(newArr);        // [3, 1, 2]
```

->  ES6 에서 도입된 스프레드 문법은 함수 호출없이 표현식으로 선두에 요소를 추가할 수 있으며 unshift 메서드와 달리 부수 효과도 없음.  
<br>

### 6) Array.prototype.shift

> ``` 원본 배열에서 첫 번째 요소를 제거하고, 제거한 요소 반환 ```  
> -> shift 메서드는 원본 배열을 직접 변경

<br>

``` jsx
const arr = [1, 2];

let result = arr.shift();
console.log(result);      // 1

// 원본 배열에 영향 O
console.log(arr);         // [2]
```

### ``` ** shift 메서드와 pop 메서드를 통해 큐 구현 ```  
<br>

- 큐 : 선입 선출 (FIFO - First In First Out) 방식의 자료구조  
<br>

![img](https://miro.medium.com/max/600/1*9rHSt4KIYz5rvT_lZPdNcg.png)

<br>

``` jsx
// 생성자 함수로 구현한 큐
const Queue = (function() {
  function Queue(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.array = array;
  }

  Queue.prototype = {
    constructor : Queue,

    // 큐의 가장 마지막에 데이터 밀어 넣기
    enqueue(value) {
      return this.array.push(value);
    },

    // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터 꺼내기 
    dequeue() {
      return this.array.shift();
    },

    // 큐의 복사본 배열 반환
    entries() {
      return [...this.array];
    }
  };

  return Queue;
}());

const queue = new Queue([1, 2]);
console.log(queue.entries());       // [1, 2]

queue.push(3);
console.log(queue.entries());       // [1, 2, 3]

queue.shift();
console.log(queue.entries());       // [2, 3]
```

``` jsx
// 클래스로 구현한 큐

class Queue {
  #array;
  
  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 큐의 가장 마지막에 데이터 밀어 넣기
  enqueue(value) {
    return this.#array.push(value);
  },

  // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터 꺼내기 
  dequeue() {
    return this.#array.shift();
  },

  // 큐의 복사본 배열 반환
  entries() {
    return [...this.#array];
  }
}

const queue = new Queue([1, 2]);
console.log(queue.entries());       // [1, 2]

queue.push(3);
console.log(queue.entries());       // [1, 2, 3]

queue.shift();
console.log(queue.entries());       // [2, 3]
```
<br>

### 7) Array.prototype.concat

> ``` 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열 반환 ```  
> -> 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가  
> -> concat 메서드는 원본 배열 변경 X

<br>

``` jsx
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2 를 원본 배열 arr1 의 마지막 요소로 추가한 새로운 배열 반환
// 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가
let result = arr1.concat(arr2);
console.log(result);      // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1 의 마지막 요소로 추가한 새로운 배열 반환
result = arr1.concat(arr2, 5);
console.log(result);      // [1, 2, 3, 4, 5]

// 원본 배열에 영향 X
console.log(arr1);        // [1, 2]
```
<br>

### ** push 와 unshift 메서드는 concat 메서드로 대체 가능  

-> push 와 unshift 메서드는 원본 배열을 직접 변경하지만 concat 메서드는 원본 배열을 변경하지 않고 새로운 배열 반환  

- push/unshift 메서드를 사용할 때는 원본 배열을 반드시 변수에 저장해두어야 함.  

- unshift 메서드를 사용할 때는 반환값을 반드시 변수에 할당받아야 함.  
<br>

``` jsx
const arr = [3, 4];

// unshift 와 push 메서드는 인수로 전달받은 배열을 그대로 원본 배열의 요소로 추가
arr.unshift([1, 2]);
arr.push([5, 6]);
console.log(arr);       // [[1, 2], 3, 4, [5, 6]]

// concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 요소로 추가
let result = [1, 2].concat([3, 4]);
result = result.concat([5, 6]);
console.log(result);    // [1, 2, 3, 4, 5, 6]
```
<br>

### ** concat 메서드는 ES6 의 스프레드 문법으로 대체 가능

``` jsx
// concat 메서드를 사용한 경우
let result = [1, 2].concat([3, 4]);
console.log(result);      // [1, 2, 3, 4]

// 스프레드 문법을 사용한 경우
result = [...[1, 2], ...[3, 4]];
console.log(result0);     // [1, 2, 3, 4]
```

``` -> 결론적으로 push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6 의 스프레드 문법을 일관성 있게 사용하는 것을 권장 ```  
<br>

### 8) Array.prototype.splice
> ``` 원본 배열의 중간에 요소를 추가/제거하는 경우 사용 ```  
> -> splice 메서드는 3개의 매개변수가 있으며, 원본 배열을 직접 변경

<br>  

``` jsx
splice(start, deleteCount, items)
```

- start : 요소를 제거하기 시작할 인덱스로, start 만 지정하면 원본 배열의 start 부터 모든 요소를 제거함.

- deleteCount : start 부터 제거할 요소의 개수 (옵션)

- items : 제거한 위치에 삽입할 요소들의 목록 (옵션)  
<br>

``` jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고, 그 자리에 새로운 요소인 20, 30 삽입
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환됨
console.log(result);      // [2, 3]

// 원본 배열에 영향 O
console.log(arr);         // [1, 20, 30, 4]
```

``` jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 0개의 요소를 제거하고, 그 자리에 새로운 요소인 100 삽입
const result = arr.splice(1, 0, 100);

console.log(result);    // []
console.log(arr);       // [1, 100, 2, 3, 4]
```

``` jsx
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 모든 요소 제거
const result = arr.splice(1);

console.log(reesult);     // [2, 3, 4]
console.log(arr);         // [1]
```
<br>

### 9) Array.prototype.slice
> ``` 인수로 전달된 범위의 요소들을 복사하여 배열로 반환  ```  
> -> slice 메서드는 원본 배열을 변경하지 않음.

<br>

``` jsx
slice(start, end)
``` 

- start : 복사를 시작할 인덱스 (음수인 경우 배열의 끝에서의 인덱스를 나타냄)
- end : 복사를 종료할 인덱스 (종료 인덱스는 포함 x)  
<br>  

``` jsx
const arr = [1, 2, 3];

// arr[0] 브터 arr[1] 이전 (미포함) 까지 복사하여 반환
arr.slice(0,1);       // [1]

// 원본 배열에 영향 X
console.log(arr);     // [1, 2, 3]
```

``` jsx
const arr = [1, 2, 3];

// arr[1] 부터 이후의 모든 요소를 복사하여 반환
arr.slice(1);         // [2, 3]

// 배열의 끝에서부터 요소를 2개 복사하여 반환
arr.slice(-2);        // [2, 3]
```

``` jsx
const arr = [1, 2, 3];

// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환
const copy = arr.slice();
console.log(copy)                    // [1, 2, 3]

// 이때 생성된 복사본은 '얕은 복사' 를 통해 생성됨
// 원본 배열과 복사본 자체는 참조값이 다른 별개의 객체
console.log(copy === arr);           // false

// But, 배열 요소의 참조값은 동일함. 즉, 얕은 복사되었다는 것
console.log(copy[1] === arr[1])      // true
```

``` ** 얕은 복사와 깊은 복사 ```
- 얕은 복사 : 한 단계까지만 복사  
-> slice 메서드, 스프레드 문법, Object.assgin 메서드 사용  

- 깊은 복사 : 객체에 중첩되어 있는 객체까지 모두 복사  
 -> ClondeDeep 메서드 사용  
<br>

### 10) Array.prototype.join
> ```원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 구분자로 연결한 문자열을 반환```    
> -> 기본 구분자는 콤마(,)

<br>

``` jsx
const arr = [1, 2, 3, 4];

arr.join();        // '1, 2, 3, 4'

arr.join('');      // '1234'

arr.join(':');     // '1:2:3:4'
```
<br>

### 11) Array.prototype.reverse
> ```원본 배열의 순서를 반대로 뒤집으며, 변경된 배열을 반환함.```    
> -> reverse 메서드는 원본 배열을 변경함.

<br>

``` jsx
const arr = [1, 2, 3];
const result = arr.reverse();

// 반대로 뒤집은 배열을 반환
console.log(result);      // [3, 2, 1]

// 원본 배열에 영향 O
console.log(arr);         // [3, 2, 1]
```
<br>

### 12) Array.prototype.fill
> 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채움.
> -> ES6에서 도입된 fill 메서드는 원본 배열을 변경함.

<br>

``` jsx
fill(element, start, end)
```

- element : 채울 요소값
- start : 요소 채우기를 시작할 인덱스 (옵션)
- end : 요소 채우기를 멈출 인덱스로, 마지막 인덱스는 포함 X (옵션)

<br>

``` jsx
const arr = [1, 2, 3];

// 인수로 전달받은 값 0을 배열의 처음부터 끝까지 요소로 채움
arr.fill(0);           // [0, 0, 0]

// 원본 배열에 영향 O
console.log(arr);      // [0, 0, 0]
```

``` jsx
const arr = [1, 2, 3, 4, 5];

// 인수로 전달받은 값 0을 배열의 인덱스 1부터 3 이전까지 요소로 채움
arr.fill(0, 1, 3);      // [1, 0, 0, 4, 5]
```

-> fill 메서드로 요소를 채울 경우 모든 요소를 하나의 값만으로 채울 수밖에 없다는 단점 O  
<br>

### 13) Array.prototype.includes
> ``` 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false 반환``` 

<br>

``` jsx
includes(element, start)
```

- element : 검색할 대상   
- start : 검색을 시작할 인덱스

<br>

``` jsx
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인
arr.includes(2);          // true

// 배열에 요소 1이 포함되어 있는지 인덱스 1 부터 확인
arr. includes(1, 1);      // false 

// 배열에 요소 3이 포함되어 있는지 마지막 인덱스부터 확인
arr.includes(3, -1);      // true
```

### 14) Array.prototype.flat
> ```인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화함.```  
> -> 중첩 배열을 평탄화할 깊이를 인수로 전달하고, 인수를 생략하면 기본값은 1

<br>

``` jsx
// 중첩 배열을 평탄화하기 위한 깊이 값의 기본값은 1
[1, [2, [3, [4]]]].flat();            // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(1);           // [1, 2, [3, [4]]]

// 중첩 배열을 평탄화하기 위한 깊이 값을 2로 지정 -> 2단계 깊이까지 평탄화
[1, [2, [3, [4]]]].flat(2);           // [1, 2, 3, [4]]

// 2번 평탄화한 것과 동일
[1, [2, [3, [4]]]].flat().flat();     // [1, 2, [3, [4]]]

// 중첩 배열 모두를 평탄화
[1, [2, [3, [4]]]].flat(Infinity);    // [1, 2, 3, 4]
```
<br>

## 27.9 배열 고차 함수
>  

### 1) Array.prototype.sort
> ```배열의 요소를 정렬하며, 정렬된 배열을 반환하는데 기본적으로 오름차순 정렬 ```  
> -> sort 메서드는 원본 배열을 직접 변경함.

<br>

``` jsx
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순 정렬
fruits.sort();

// 원본 배열에 영향 O
console.log(fruits);      // ['Apple', 'Banana', 'Orange']

// 내림차순 정렬
fruits.reverse();

// 원본 배열에 영향 O
console.log(fruits);      // ['Orange', 'Banana', 'Apple']
``` 

-> 숫자 정렬ㅉ
<br><br>
** 이미지 출처 : https://velog.io/@nareum/%EB%B0%B0%EC%97%B4Array 
https://medium.com/@lyhlg0201/immersive-sprint-js-stack-queue-426ccfbdb602