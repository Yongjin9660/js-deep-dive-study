# 11. 원시값과 객체의 비교

### 💡 원시 타입의 값과 객체 타입의 값의 차이점

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

```js
// const 키워드를 사용하여 선언한 변수는 재할당이 금지됨.
const o = {};

// const 키워드를 사용하여 선언한 변수에 할당한 원시 값 (상수) 은 변경 X
// But, const 키워드를 사용하여 선언한 변수에 할당한 객체는 변경 O
o.a = 1;
console.log(o); // {a : 1}
```

<br>

![df](https://blog.kakaocdn.net/dn/bGnhFj/btrdpqzpKIE/DAs9hoDetuuljcQkhRPCk1/img.jpg)

- **원시 값을 할당한 변수에 새로운 원시 값을 재할당**
  - 메모리 공간에 저장되어 있는 재할당 이전의 원시 값을 변경하는 것 X
  - 새로운 메모리 공간을 확보하고 재할당 원시 값을 저장 => 재할당한 원시 값을 가리킴

> 즉, '원시 값 변경이 불가능하다' 는 말은 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것이 아님
>
> - 이러한 특성을 **불변성** 이라고 함
>   - 불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없음

**2. 문자열과 불변성**

- 문자열 또한 원시 타입이며, 변경 불가능
- 읽기 전용 값

```jsx
// 자바스크립트의 문자열 또한 원시 타입이기 때문에 변경 불가능
var str = 'Hello';
str = 'world!';

// 문자열은 유사 배열 객체이면서 이터러블이므로 배열과 유사하게 인덱스를 통해 각 문자에 접근 가능
console.log(str[1]); // e

// 이미 생성된 문자열의 일부 문자를 변경해도 반영 X
// (새로운 문자열 재할당은 가능)
str[2] = 'T';
console.log(str); // Hello
```

**3. 값에 의한 전달**

```jsx
// copy 변수에는 score 변수의 값 80 이 복사되어 전달됨.
var score = 80;
var copy = score;

console.log(score, copy); // 80 80
console.log(score === copy); // true

// score 변수와 copy 변수의 값은 다른 메모리 공간에 저장된 별개의 값
// -> score 변수의 값을 변경해도 copy 변수의 값은 영향 받지 않음.
score = 100;
console.log(score, copy); // 100, 80
console.log(score === copy); // false
```

- **변수에 변수를 할당했을 때**

![img](https://blog.kakaocdn.net/dn/7e6OS/btrdo862oUX/lnv8cYKhqCsKU6NBs3mKV0/img.jpg)

- score에 저장된 80이 새로 생성되어 copy에 할당됨 => 값에 의한 전달
- score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값

## 11.2 객체

**1. 변경 가능한 값**

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

- **객체는 변경 가능한 값**
  - 재할당 없이 객체를 직접 변경할 수 있음
  - 동적으로 프로퍼티 추가, 갱신, 삭제 가능
  - 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값에 접근 가능
  - 참조 값은 생성된 객체가 저장된 메모리 공간의 주소, 그 자체를 의미하기 때문에 변수는 이 참조 값을 통해 객체에 접근 가능

![img](https://blog.kakaocdn.net/dn/dKRoZe/btrdr4hPpQq/eshJuNbTRN6Sb0FGIwVvu1/img.jpg)

- **부작용**
  - 원시 값과는 다르게 여러 개의 식별자가 하나의 객체를 공유할 수 있게 됨

```jsx
var person = {
	name: 'Lee',
};

// 참조 값을 복사 (얕은 복사)
// -> copy 와 person 은 동일한 참조 값을 가짐.
var copy = person;

// -> 원본 person 과 사본 copy 는 저장된 메모리 주소는 다르지만 동일한 참조 값을 가짐.
// -> 즉, 원본 person 과 사본 copy 모두 동일한 객체를 가리킨다는 것.
// (두 개의 식별자가 하나의 객체를 공유)

// copy 와 person 은 동일한 객체를 참조함.
console.log(copy === person); // true

// copy 를 통해 객체 변경
copy.name = 'Kim';

// person 을 통해 객체 변경
person.address = 'Seoul';

// 동일한 객체를 참조하기 때문에 어느 한쪽이라도 객체를 변경하면 서로 영향을 주고받음.
console.log(person); // {name : 'Kim', address  : 'Seoul'}
console.log(copy); // {name : 'Kim', address  : 'Seoul'}
```

<br>

![img2](https://blog.kakaocdn.net/dn/uAhPs/btrdnjAPqZp/gZtXVt3JeN9MvFXUjkDMNK/img.jpg)

**2. 참조에 의한 전달**

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

2.  참조에 의한 전달

> - 객체를 가리키는 변수 (원본, person) 를 다른 변수 (사본, copy) 에 할당하면 <em> 원본의 참조 값이 복사되어 전달됨
> - 참조 값이 복사되어 전달되기 때문에 원본 또는 사본 중 어느 한쪽에서 객체를 변경하면 서로 영향을 주고받게 됨

```jsx
var person1 = {
	name: 'Lee',
};

var person2 = {
	name: 'Lee',
};

console.log(person1 === person2); // false
console.log(person1.name === person2.name); // true
```

- 일치 비교 연산자 (===) 를 통해 객체를 할당한 변수일 땐 참조값을 비교, 원시 값을 할당한 변수일 땐 원시 값 비교

- 객체 리터럴은 평가될 때마다 객체를 생성 -> person1 변수와 person2 변수가 가리키는 객체는 다른 메모리에 저장된 별개의 객체 => false

- 프로퍼티 값을 참조하는 person1.name 과 person2.name 은 값으로 평가될 수 있는 표현식 => 두 표현식 모두 원시 값 'Lee' 로 평가 => true
  <br>
  \*\* 이미지 출처 : https://tooo1.tistory.com/366?category=960189
