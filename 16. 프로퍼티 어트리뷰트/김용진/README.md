# 16. 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

> 💡 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다

- ECMAScript 사양에 등장하는 이중 대괄호(\[[...]])로 감싼 이름들이 내부 슬롯과 내부 메서드이다.

![image](https://user-images.githubusercontent.com/55246584/148978680-0cb2635a-7d44-42d9-8ca6-b61504629562.png)

- 내부 슬롯과 내부 메서드는 자바스크립트 엔진에서 실제로 동작
- 개발자가 직접 접근할 수는 없음
- 일부 내부 슬롯과 내부 메서드에 한하여 간접적 접근 수단 제공

```js
const obj = {};

// 내부 슬롯은 접근할 수 없음
obj.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['
// 일부 내부 슬롯과 내부 메서드에 한해 접근할 수 있는 수단 제공
obj.__proto__ // Object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

> 💡 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 어트리뷰트를 기본값으로 자동 정의

- `프로퍼티 어트리뷰트` : 자바스크립트 엔진이 관리하는 내부 상태 값
  - \[[Value]], \[[Writable]], \[[Enumerable]], \[[Configurable]]

```js
const person = { name: 'Kim' };

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
Object.getOwnPropertyDescriptor(person, 'name');
// { value: "Kim", writable: true, enumerable: true, configurable: true }
```

- ES8 에서 도입된 `Object.getOwnPropertyDescriptors` 메서드는 모든 프로퍼티의 프로퍼티 디스크립터 객체를 반환

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

### 데이터 프로퍼티

> 💡 키와 값으로 구성된 일반적인 프로퍼티

- **\[[Value]]**
  - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값
  - 프로퍼티 키를 통해 프로퍼티 값을 변경하면 \[[Value]]에 값을 재할당. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 \[[Value]]에 값을 저장
- **\[[Writable]]**
  - 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값
  - \[[Writable]]의 값이 false인 경우 해당 프로퍼티의 \[[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티
- **\[[Enumerable]]**
  - 프로퍼티의 열거 가능 여부를 나타내며 불리언 값
  - \[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없음
- **\[[Configurable]]**
  - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값
  - \[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지됨.
    - 단, \[[Writable]]이 true인 경우, \[[Value]]의 변경과 \[[Writable]]을 false를 변경하는 것은 허용

```js
const person = { name: 'Lee' };

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true }
```

### 접근자 프로퍼티

> 💡 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

- **\[[Get]]**
  - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수
  - 접근자 프로퍼티 키로 값에 접근하면 프로퍼티 어트리뷰트 \[[Get]]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환
- **\[[Set]]**
  - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수
  - 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 \[[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장
- **\[[Enumerable]]**
  - 데이터 프로퍼티의 \[[Enumerable]]과 같음
- **\[[Configurable]]**
  - 데이터 프로퍼티의 \[[Configurable]]과 같음

<br>

> 접근자 함수는 getter/setter 함수라고도 부름
> 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수 있고, 하나만 정의 가능

```js
const person = {
	// 데이터 프로퍼티
	firstName: 'yong',
	lastName: 'Kim',

	// fullName은 접근자 함수로 구성된 접근자 프로퍼티
	// getter
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	},
	// setter
	set fullName(name) {
		[this.firstName, this.lastName] = name.split(' ');
	},
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastName); // yong Kim
// 접근자 프로퍼티를 통한 값의 저장
person.fullName = 'Hae Kim';
console.log(person); // {firstName: 'Hae', lastName: 'Kim'}

// 접근자 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.fullName); // Hae Kim
```

## 16.4 프로퍼티 정의

> 💡 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나. 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말함

`Object.defineProperty` : 프로퍼티의 어트리뷰트를 정의할 수 있음

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
	value: 'yong',
	writable: true,
	enumerable: true,
	configurable: true,
});
```

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| ----------------------------------- | ---------------------------- | -------------------- |
| value                               | \[[Value]]                   | undefined            |
| get                                 | \[[Get]]                     | undefined            |
| set                                 | \[[Set]]                     | undefined            |
| writable                            | \[[Writable]]                | false                |
| enumerable                          | \[[Enumerable]]              | false                |
| configurable                        | \[[Configurable]]            | false                |

## 16.5 객체 변경 방지

> 💡 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공

| 구분           | 메서드                   | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | ------------------------ | :-----------: | :-----------: | :--------------: | :--------------: | :------------------------: |
| 객체 확장 금지 | Object.preventExtensions |       X       |       O       |        O         |        O         |             O              |
| 객체 밀봉      | Object.seal              |       X       |       X       |        O         |        O         |             X              |
| 객체 동결      | Object.freeze            |       X       |       X       |        O         |        X         |             X              |

**1. 객체 확장 금지**

- **Object.preventExtensions**
  - 프로퍼티 추가가 금지
  - `Object.isExtensible` 메서드로 확인 가능

**2. 객체 밀봉**

- **Object.seal**
  - 읽기와 쓰기만 가능
  - `Object.isSealed` 메서드로 확인 가능

**3. 객체 동결**

- Object.freeze
  - 읽기만 가능
  - `Object.isFrozen` 메서드로 확인 가능

**4. 불변 객체**

- 앞의 메서드들은 중첩 객체까지는 영향을 주지 못함
- 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면
  - 재귀적으로 Object.freeze 메서드를 호출하여야 함
