# 11. 원시값과 객체의 비교

- **원시 타입의 값**
  - immutable value
  - 변경 불가능한 값
  - 변수를 할당 => 확보된 메모리 공간에 실제 값 저장
- **객체**
  - mutable value
  - 변경 가능한 값
  - 할당 시 메모리 공간에 참조 값 저장

## 11.1 원시 값

1. **변경 불가능한 값**

- 원시 값은 변경 불가능한 값이기 때문에 값을 직접 변경할 수 없음
- 값을 변경하기 위해 원시 값을 재할당하면
  - 새로운 메모리 공간을 확보 => 재할당한 값을 저장 => 참조하던 메모리 공간의 주소를 변경
    -> `불변성`

> 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없음

2. **문자열과 불변성**

- 문자열 또한 원시 타입이며, 변경 불가능
- 읽기 전용 값

3. **값에 의한 전달**

```js
var score = 80;
var copy = score;

console.log(score); // 80
console.log(copy); // 80

score = 100;

console.log(score); // 100
console.log(copy); // 80
```

- score에 저장된 80이 새로 생성되어 copy에 할당됨 => 값에 의한 전달
- score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값

## 11.2 객체

1. 변경 가능한 값

```js
// 할당이 이루어지는 시점에 객체 리터럴이 해석 => 객체 생성
var person = { name: 'Kim' };

// person 변수에 저장되어 있는 참조 값으로 식제 객체에 접근
console.log(person); // { name:'Kim" }

// 프로퍼티 값 갱신
person.name = 'Lee';
// 프로퍼티 동적 생성
person.address = 'Seoul';
```

- 객체는 변경 가능한 값
  - 재할당 없이 객체를 직접 변경할 수 있음
  - 동적으로 프로퍼티 추가, 갱신, 삭제 가능

2. 참조에 의한 전달

```js
var person = { name: 'Kim' };

// 참조 값을 복사(얕은 복사)
// copy와 person은 동일한 참조 값을 가짐
var copy = person;

// 동일한 객체를 참조
console.log(copy === person); // true

// copy, person 둘다 변경됨
copy.name = 'Lee';
```
