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


<br><br>
** 이미지 출처 : https://velog.io/@nareum/%EB%B0%B0%EC%97%B4Array