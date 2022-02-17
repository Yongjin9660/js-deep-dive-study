# 37. Set과 Map

## 37.1 Set

- Set 객체는 `중복되지 않는 유일한 값들의 집합`
- Set 객체의 특성은 수학접 집합의 특성과 일치하며, 교집합/합집합/차집합/여집합 등 구현 가능
- 배열과 유사하지만 다음과 같은 차이가 있음.
  <br>

| 구분                                 | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | O    | X        |
| 요소 순서에 의미가 있다.             | O    | X        |
| 인덱스로 요소에 접근할 수 있다.      | O    | X        |

<br>

#### 1) Set 객체의 생성

> Set 객체는 **Set 생성자 함수로 생성**

<br>

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set("hello");
console.log(set2); // Set(4) {'h', 'e', 'l', 'o'}
```

- Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체를 생성
  - Set 생성자 함수는 `이터러블을 인수로 전달받아 Set 객체 생성`
  - `이때 이터러블의 중복된 값은 Set 객체에 요소로 저장 X`

<br>

#### 2) 요소 개수 확인

> Set 객체의 요소 개수를 확인할 때는 **Set.prototype.size** 프로퍼티 사용

<br>

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

```jsx
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, "size"));
// {set : undefined, enumerable : false, configurable : true, get : f}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - `size 프로퍼티에 생성자를 할당하여 Set 객체의 요소 개수 변경 X`

<br>

#### 3) 요소 추가

> Set 객체에 요소를 추가할 때는 **Set.prototype.add** 메서드 사용

<br>

```jsx
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

```jsx
const set = new Set();

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```

- add 메서드는 새로운 요소가 추가된 Set 객체를 반환

  - add 메서드를 호출한 후에 `add 메서드를 연속적으로 호출하는 것도 가능`
  - 중복된 요소의 추가는 허용 X => 에러 발생없이 무시됨.
    <br>

- `Set 객체는 자바스크립트의 모든 값을 요소로 저장 가능`

<br>

#### 4) 요소 존재 여부 확인

> Set 객체에 특정 요소가 존재하는지 확인하려면 **Set.prototype.has** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

- `has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환`

<br>

#### 5) 요소 삭제

> Set 객체의 특정 요소를 삭제하려면 **Set.prototype.delete** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

// 요소 2를 삭제
set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(0);
console.log(set); // Set(2) {1, 3}

set.delete(1).delete(3); // TypeError
```

- `delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환`

  - add 메서드와 달리 `연속적으로 호출 X`
    <br>

- 인덱스가 아닌 삭제하려는 `요소값 자체를 인수로 전달`해야 함.
  - 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시됨.
    <br>

#### 6) 요소 일괄 삭제

> Set 객체의 모든 요소를 일괄 삭제하려면 **Set.prototype.clear** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

- `clear 메서드는 언제나 undefined 반환`

<br>

#### 7) 요소 순회

> Set 객체의 요소를 순회하려면 **Set.prototype.forEach** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v1, v2, set) => console.log(v1, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- forEach 메서드에는 다음과 같이 3개의 인수를 전달
  - 첫 번째 인수 : `현재 순회 중인 요소값`
  - 두 번째 인수 : `현재 순회 중인 요소값`
  - 세 번째 인수 : `현재 순회 중인 Set 객체 자체`

<br>

```jsx
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for... of 문으로 순회 가능
for (const value of set) {
	console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

- Set 객체는 이터러블이기 때문에 for... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있음.
  <br>

- Set 객체는 요소의 순서에 의미를 갖지 않지만 `Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름.`
  - 다른 이터러블의 순회와 호환성을 유지하기 위함

<br>

#### 8) 집합 연산

##### 1) 교집합

> 집합 A 와 집합 B 의 공통 요소로 구성

<br>

```jsx
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v = > set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB 의 교집합
console.log(setA.intersection(setB));   // Set(2) {2, 4}
```

<br>

##### 2) 합집합

> 집합 A 와 집합 B 의 중복 없는 모든 요소로 구성

<br>

```jsx
Set.prototype.union = function (set) {
	return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB 의 합집합
console.log(setA.union(setB)); // Set(2) {1, 2, 3, 4}
```

<br>

##### 3) 차집합

> A - B 는 집합 A 에는 존재하지만 집합 B 에는 존재하지 않는 요소로 구성

<br>

```jsx
Set.prototype.difference = function (set) {
	return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 에 대한 setB 의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}

// setB 에 대한 setA 의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

<br>

##### 4) 부분 집합과 상위 집합

> 집합 A 가 집합 B 에 포함되는 경우 집합 A 는 집합 B 의 부분 집합이며, 집합 B 는 집합 A 의 상위 집합

<br>

```jsx
// this 가 subset 의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
	const supersetArr = [...this];
	return [...subset].every((v) => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 가 setB 의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true

// setB 가 setA 의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

<br>

## 37.2 Map

- Map 객체는 `키와 값의 쌍으로 이루어진 컬렉션`
- 객체와 유사하지만 다음과 같은 차이가 있음.
  <br>

| 구분                   | 객체                    | Map 객체              |
| ---------------------- | ----------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든 값 |
| 이터러블               | X                       | O                     |
| 요소 개수 확인         | Object.keys(obj).length | map.size              |

<br>

#### 1) Map 객체의 생성

> Map 객체는 **Map 생성자 함수로 생성**

<br>

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

```jsx
const map1 = new Map([
	["key1", "value1"],
	["key2", "value2"],
]);
console.log(map1); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

const map2 = new Map([1, 2]); // TypeError
```

- Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체를 생성
  - Map 생성자 함수는 `이터러블을 인수로 전달받아 Map 객체 생성`
  - `이때 이터러블은 키와 값의 쌍으로 이루어진 요소`

<br>

```jsx
const map1 = new Map([
	["key1", "value1"],
	["key1", "value2"],
]);
console.log(map); // Map(1) {'key1' => 'value1'}
```

- `Map 객체에는 중복된 키를 갖는 요소 X`
  - 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어씌워짐.
    <br>

#### 2) 요소 개수 확인

> Map 객체의 요소 개수를 확인할 때는 **Map.prototype.size** 프로퍼티 사용

<br>

```jsx
const { size } = new Map([
	["key1", "value1"],
	["key2", "value2"],
]);

console.log(size); // 2
```

```jsx
const map = new Map([
	["key1", "value1"],
	["key2", "value2"],
]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, "size"));
// {set : undefined, enumerable : false, configurable : true, get : f}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - `size 프로퍼티에 생성자를 할당하여 Map 객체의 요소 개수 변경 X`

<br>

#### 3) 요소 추가

> Map 객체에 요소를 추가할 때는 **Map.prototype.set** 메서드 사용

<br>

```jsx
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1").set("key2", "value2");

console.log(map); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}
```

- set 메서드는 새로운 요소가 추가된 Map 객체를 반환

  - set 메서드를 호출한 후에 `set 메서드를 연속적으로 호출하는 것도 가능`
  - 중복된 키를 갖는 요소의 추가는 허용 X => 에러 발생없이 무시됨.
    <br>

```jsx
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

// 객체도 키로 사용 가능
map.set(lee, "developer").set(kim, "designer");

console.log(map); // Map(2) { {name : "Lee"} => 'developer', {name : "Kim"} => 'designer' }
```

- `Map 객체는 객체를 포함한 모든 값을 키로 사용 가능`

<br>

#### 4) 요소 취득

> Map 객체에서 특정 요소를 취득하려면 **Map.prototype.get** 메서드 사용

<br>

```jsx
const map = new Map();

const lee = { name: "Lee" };
const kim = { name: "Kim" };

map.set(lee, "developer").set(kim, "designer");

console.log(map.get(lee)); // developer
console.log(map.get("key")); // undefined
```

- get 메서드의 인수로 키를 전달하면 `Map 객체에서 인수로 전달한 키를 갖는 값을 반환`
  - Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined 반환

<br>

#### 5) 요소 존재 여부 확인

> Map 객체에서 특정 요소가 존재하는지 확인하려면 **Map.prototype.has** 메서드 사용

<br>

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

console.log(map.has(lee)); // true
console.log(map.has("key")); // false
```

- `has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환`

<br>

#### 6) 요소 삭제

> Map 객체의 요소를 삭제하려면 **Map.prototype.delete** 메서드 사용

<br>

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

map.delete(kim);
console.log(map); // Map(1) { {name : "Lee"} => 'developer'}

map.delete("");
console.log(map); // Map(1) { {name : "Lee"} => 'developer'}
```

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

map.delete(lee).delete(kim); // TypeError
```

- `delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환`
  - set 메서드와 달리 `연속적으로 호출 X`
  - 존재하지 않는 키로 Map 객체의 요소를 삭제하려 하면 에러 없이 무시됨.

<br>

#### 7) 요소 일괄 삭제

> Map 객체의 모든 요소를 일괄 삭제하려면 **Set.prototype.clear** 메서드 사용

<br>

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

map.clear();
console.log(map); // Map(0) {}
```

- `clear 메서드는 언제나 undefined 반환`

<br>

#### 7) 요소 순회

> Map 객체의 요소를 순회하려면 **Map.prototype.forEach** 메서드 사용

<br>

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name : "Lee"} Map(2) {
  {name : "Lee"} => "developer"
  {name : "Kim"} => "designer"
}
designer {name : "Kim"} Map(2) {
  {name : "Lee"} => "developer"
  {name : "Kim"} => "designer"
}
*/
```

- forEach 메서드에는 다음과 같이 3개의 인수를 전달
  - 첫 번째 인수 : `현재 순회 중인 요소값`
  - 두 번째 인수 : `현재 순회 중인 요소값`
  - 세 번째 인수 : `현재 순회 중인 Set 객체 자체`

<br>

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

// Map 객체는 Map.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for... of 문으로 순회 가능
for (const value of map) {
	console.log(value); // [{name : "Lee"}, "developer"] [{name : "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]); // [[{name : "Lee"}, "developer"], [{name : "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = map;
console.log(a, rest); // [{name : "Lee"}, "developer"] [{name : "Kim"}, "designer"]
```

- Map 객체는 이터러블이기 때문에 for... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있음.

<br>

\*\* **Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공**

| Map 메서드            | 설명                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------ |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.entries | Map 객체에서 요소키와 요소값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |

<br>

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
	[lee, "developer"],
	[kim, "designer"],
]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환
for (const key of map.keys()) {
	console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환
for (const value of map.values()) {
	console.log(value); // developer designer
}

// Map.prototype.entires 는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터 반환
for (const entry of map.entries()) {
	console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}
```

- Map 객체는 요소의 순서에 의미를 갖지 않지만 `Map 객체를 순회하는 순서는 요소가 추가된 순서를 따름.`
  - 다른 이터러블의 순회와 호환성을 유지하기 위함
