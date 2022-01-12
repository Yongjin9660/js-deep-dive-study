# 27. 배열

## 27.1 배열이란?

> 💡 배열은 여러 개의 값을 순차적으로 나열한 자료구조

```js
const arr = ['apple', 'banana', 'orange'];
```

- `요소` : 배열이 가지고 있는 값
  - 자바스크립트의 모든 값은 배열의 요소가 될 수 있음
  - ex. 원시값, 객체, 함수, 배열 등
- `인덱스` : 배열의 요소가 자신의 위치를 나타내는 0 이상의 정수
- `length 프로퍼티` : 요소의 개수, 배열의 길이를 나타냄

```js
// 배열의 순회
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 'apple', 'banana', 'orange'
}
```

- 자바스크립트에서 배열이라는 타입은 존재하지 않음
  - **배열은 객체 타입**

```js
typeof arr; // object
```

| 구분            |           객체            |     배열      |
| :-------------- | :-----------------------: | :-----------: |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       |        프로퍼티 키        |    인덱스     |
| 값의 순서       |             X             |       O       |
| length 프로퍼티 |             X             |       O       |

- 일반 객체와 배열을 가장 구분하는 차이는 `값의 순서`와 `length 프로퍼티`

> 👉 인덱스로 표현되는 값의 순서와 length 프로퍼티를 갖는 배열은 반복문을 통해 순차적으로 값에 접근하기 적합한 자료구조

## 27.2 자바스크립트 배열은 배열이 아니다

### 자료구조에서 말하는 배열

- 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조
- 배열의 요소는 하나의 데이터 타입
- `밀집배열(dense array)`이라 함

  <image src="https://user-images.githubusercontent.com/55246584/149067739-ca0f9b93-c2d0-4efe-93b2-5c6a04d053a9.png" height="150px">

- 인덱스를 통해 단 한 번의 연산으로 임의의 요소 접근 `O(1)`
  - 검색 대상 요소의 메모리 주소 = 배열의 시작 메모리 주소 + 인덱스 \* 요소의 바이트 수

### 자바스크립트의 배열

- 일반적인 의미의 배열과 다름
  - 각각의 메모리 공간은 동일한 크기를 갖지 않아도 됨
  - 연속적으로 이어져 있지 않아도 됨
  - `희소 배열(sparse array)`이라고 함

```js
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  ...
  ...
  length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```

> 👉 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지고, length 프로퍼티를 갖는 특수한 객체

- **일반적인 배열**은 인덱스로 요소를 빠르게 접근할 수 있음
  - 하지만 요소를 삽인 또는 삭제하는 경우에 효율적이지 않음
- **자바스크립트 배열**은 해시 테이블로 구현된 객체
  - 인덱스로 요소에 접근하는 것보다는 성능적인 면에서 느릴 수 밖에 없음
  - 요소를 삽입 또는 삭제하는 경우에 일반적인 배열보다 더 빠른 성능을 기대할 수 있음

## 27.3 length 프로퍼티와 희소 배열

> 💡 length 프로퍼티는 요소의 개수, 배열의 길이를 나타내는 0 이상의 정수를 값으로 가짐

### length 프로퍼티

- 배열에 요소를 추가하거나 삭제하면 자동 갱신
- 임의의 숫자 값을 명시적으로 할당 가능

  - 현재 값보다 더 작은 값을 할당

    ```js
    const arr = [1, 2, 3, 4, 5];
    console.log(arr.length); // 5

    arr.length = 3;
    // 배열의 길이가 5에서 3으로 줄음
    console.log(arr); // [1, 2, 3]
    ```

  - 현재 값보다 더 큰 값을 할당

    ```js
    const arr = [1];

    arr.length = 3;
    // length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않음
    console.log(arr.length); // 3
    console.log(arr); // [1, empty * 2]

    console.log(Object.getOwnPropertyDescriptors(arr));
    /*
      {
        '0': {value: 1, writable: true, enumerable: true, configurable: true}
        length: {value: 3, writable: true, enumerable: false, configurable: false}
      }
    */
    ```

    - 값 없이 비어있는 요소를 위해 메모리 공간을 확보하지 않으며 빈 요소를 생성하지 않음

  ```js
  const sparse = [, 2, , 4];
  // 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않음
  console.log(sparse.length);
  console.log(sparse); // [empty, 2, empty, 4]
  ```

  - 희소 배열의 length 는 희소 배열의 실제 요소 개수보다 언제나 큼

## 27.4 배열 생성

**1. 배열 리터럴**

```js
const arr = [1, 2, 3];
```

**2. Array 생성자 함수**

```js
const arr = new Array(10);

console.log(arr); // [empty * 10]
console.log(arr.length); // 10
```

**3. Array.of**

```js
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of('string'); // ['string']
```

**4. Array.from**

- **유사 배열 객체** 또는 **이터러블 객체**를 인수로 전달받음

```js
// 유사 배열 객체를 변환하여 배열 생성
Array.from({ length: 2, 0: 'a', 1: 'b' });

// 이터러블을 변환하여 배열을 생성
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
```

`유사 배열 객체` : 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체
`이터러블 객체` : Symbol.iterator 메서드를 구현하여 for .. of 문으로 순회할 수 있고, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있는 객체

## 27.5 배열 요소의 참조

```js
const arr = [1, 2];

// 인덱스가 0인 요소
console.log(arr[0]); // 1
```

## 27.6 배열 요소의 추가와 갱신

```js
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [0, 1]
console.log(arr.length); // 2

// 프로퍼티 추가
arr['foo'] = 3;
arr[1.1] = 5;

console.log(arr); // [0, 1, foo: 3, '1.1': 5]

// 프로퍼티는 length에 영향을 주지 않음
console.log(arr.length); // 2
```

## 27.7 배열 요소의 삭제

- 배열은 객체이기 때문에 delete 연산자를 사용할 수 있음
  - 하지만, length 값을 바꾸지 않음

```js
const arr = [1, 2, 3];

// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]

console.log(arr.length); // 3
```

## 27.8 배열 메서드

- 배열에는 **원본을 직접 변경하는 메서드**와 원본 배열을 **직접 변경하지 않고 새로운 배열을 반환하는 메서드**가 있음
- 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 것이 좋음

**1. Array.isArray**

> 💡 전달된 인수가 배열이면 true, 배열이 아니면 false 를 반환

```js
// true
Array.isArray([]);
Array.isArray([1]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
```

**2. Array.prototype.indexOf**

> 💡 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스 반환

- 첫 번째로 검색된 인덱스를 반환
- 존재하지 않는다면 -1 반환

```js
const arr = [1, 2, 2, 3];

arr.indexOf(2); // 1
arr.indexOf(4); // -1

// 두 번째 인수는 검색을 시작할 인덱스
arr.indexOf(2, 2); // 2
```

**3. Array.prototype.push**

> 💡 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환

```js
const arr = [1, 2, 3];

const result = arr.push(4, 5);
console.log(result); // 5

console.log(arr); // [1, 2, 3, 4, 5]
```

- push 메서드는 성능 면에서 좋지 않음 => length 프로퍼티를 사용하거나 스프레드 문법을 사용하는 것이 좋음

```js
const arr = [1, 2];
arr[arr.length] = 3;
console.log(arr); // [1, 2, 3];

const newArr = [...arr, 4];
console.log(newArr); // [1,2,3,4]
```

**4. Array.prototype.pop**

> 💡 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환

```js
const arr = [1, 2];

let result = arr.pop();
console.log(result); // 2

console.log(arr); // [1, 2]
```

**5. Array.prototype.unshift**

> 💡 인수로 전달받은 모든 값을 원본 배열의 선두에 추가하고 length 프로퍼티 값을 반환

```js
const arr = [1, 2];

let result = arr.unshift(3, 4);
console.log(result); // 4

console.log(arr); // [3, 4, 1, 2]
```

- 원본 배열을 직접 변경하는 부수 효과가 있음
- 스프레드 문법을 사용하는 것이 좋음
  ```js
  const arr = [1, 2];
  const newArr = [3, ...arr];
  console.log(newArr); // [3, 1, 2]
  ```

**6. Array.prototype.shift**

> 💡 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환

```js
const arr = [1, 2];
const result = arr.shift();
console.log(result); // 1

// 원본 배열을 직접 변경
console.log(arr); // [2]
```

**7. Array.prototype.concat**

> 💡 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환

```js
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가하고 새로운 배열을 반환
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]
```

- 스프레드 문법으로 대체 가능

```js
const result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

### 참조

- https://velog.io/@nareum/%EB%B0%B0%EC%97%B4Array

```

```
