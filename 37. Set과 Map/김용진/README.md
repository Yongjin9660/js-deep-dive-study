# 37. Set과 Map

## 37.1 Set

> 💡 Set 객체는 중복되지 않는 유일한 값들의 집합

| 구분                                | 배열 | Set 객체 |
| ----------------------------------- | :--: | :------: |
| 동일한 값을 중복하여 포함할 수 있음 |  O   |    X     |
| 요소 순서에 의미가 있음             |  O   |    X     |
| 인덱스로 요소에 접근 가능           |  O   |    X     |

### Set 객체의 생성

> 💡 Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성

```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

- 중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거

```js
// 배열의 중복 요소 제거
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);

// Set을 사용한 배열의 중복 요소 제거
const uniq = (array) => [...new Set(array)];
```

### 요소 개수 확인

> 💡 Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 요소 추가

> 💡 Set 객체의 요소를 추가할 때는 Set.prototype.add 메서드를 사용

```js
const set = new Set();
console.log(set); // Set(0)

set.add(1);
console.log(set); // Set(1) {1}
```

- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있음

```js
const set = new Set();

set
	.add(1)
	.add('a')
	.add(true)
	.add(undefined)
	.add(null)
	.add({})
	.add([])
	.add(() => {});

console.log(set); // Set(8) {1, "a", true, undefined, null, {}, [], () => {}}
```

### 요소 존재 여부 확인

> 💡 Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### 요소 삭제

> 💡 Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용

```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시
set.delete(0);
console.log(set); // Set(2) {1, 3}
```

- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환

```js
const set = new Set([1, 2, 3]);

// delete는 불리언 값을 반환
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```

### 요소 일괄 삭제

> 💡 Set 객체의 모든 요소를 일괄 삭제하려면 Set.prototype.clear 메서드를 사용

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### 요소 순회

> 💡 Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
 */
```

- **Set 객체는 이터러블**
  - for ... of 문으로 순회 가능
  - 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있음

```js
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in set); // true

for (const value of set) {
	console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있음
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 됨
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

👉 Set 객체는 요소의 순서에 의미를 갖지 않지만 Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름

### 집합 연산

> 💡 Set 객체를 통해 교집합, 합집합, 차집합 등을 구현 가능

**교집합**

```js
Set.prototype.intersection = function (set) {
	const result = new Set();

	for (const value of set) {
		// 2개의 set의 요소가 공통되는 요소이면 교집합의 대상
		if (this.has(value)) result.add(value);
	}

	return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
```

아래와 같은 방법으로도 가능

```js
Set.prototype.intersection = function (set) {
	return new Set([...this].filter((v) => set.has(v)));
};
```

**합집합**

```js
Set.prototype.union = function (set) {
	// this(Set 객체)를 복사
	const result = new Set(this);

	for (const value of set) {
		result.add(value);
	}

	return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
```

아래와 같은 방법으로도 가능

```js
Set.prototype.union = function (set) {
	return new Set([...this, ...set]);
};
```

**차집합**

```js
Set.prototype.difference = function (set) {
	const result = new Set(this);

	for (const value of set) {
		result.delete(value);
	}

	return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

아래와 같은 방법으로도 가능

```js
Set.prototype.difeerence = function (set) {
	return new Set([...this].filter((v) => !set.has(v)));
};
```

**부분 집합과 상위 집합**

```js
// this가 subset의 상위 집합인지 확인
Set.prototype.isSuperset = function (subset) {
	for (const value of subset) {
		// superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
		if (!this.has(value)) return false;
	}
	return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인
console.log(setA.isSuperset(setB)); // true
```

아래와 같은 방법으로도 가능

```js
Set.prototype.isSuperset = function (subset) {
	const supersetArr = [...this];
	return [...subset].every((v) => supersetArr.includes(v));
};
```

## 37.2 Map

> 💡 Map 객체는 키와 값의 쌍으로 이루어진 컬렉션

| 구분                   |          배열           |       Map 객체        |
| ---------------------- | :---------------------: | :-------------------: |
| 키로 사용할 수 있는 값 |   문자열 또는 심벌 값   | 객체를 포함한 모든 값 |
| 이터러블               |            X            |           O           |
| 요소 개수 확인         | Object.keys(obj).length |       map.size        |

### Map 객체의 생성

> 💡 Map 객체는 Map 생성자 함수로 생성

- Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성
  - 인수로 전닯되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 함

```js
const map1 = new Map([
	['key1', 'value1'],
	['key2', 'value2'],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeEror: Iterator value 1 is not an entry object
```

### 요소 개수

> 💡 Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용

```js
const { size } = new Map([
	['key1', 'value1'],
	['key2', 'value2'],
]);
console.log(size); // 2
```

### 요소 추가

> 💡 Map 객체에 요소를 추가할 때는 Map.prototype.set 메서드를 사용

```js
const map = new Map();

map.set('key1', 'value1').set('key2', 'value2');
console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

### 요소 취득

> 💡 Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### 요소 존재 여부 확인

> 💡 Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

### 요소 삭제

> 💡 Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```

- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환

  - 연속 호출 불가능

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

map.delete(lee).delete(kim); // TypeError: map.delete(...).delete is not a function
```

### 요소 일괄 삭제

> 💡 Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

map.clear();
console.log(map); // Map(0) {}
```

### 요소 순회

> 💡 Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용

- **첫 번째 인수**: 현재 순회 중인 요소값
- **두 번째 인수**: 현재 순회 중인 요소키
- **세 번째 인수**: 현재 순회 중인 Map 객체 자체

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
	{name: "Lee"} => "developer"
	{name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
	{name: "Lee"} => "developer"
	{name: "Kim"} => "designer"
}
 */
```

**Map 객체는 이터러블**

- for ... of 문으로 순회 가능
- 스프레드 문법과 배열 디스트럭처링 할당의 대상이 됨

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for ... of 문으로 순회 가능
for (const entry of map) {
	console.log(entry);
}

// 이터러블인 Map 객체는 스프레드 문법의 대상
console.log([...map]);

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상
const [a, b] = map;
console.log(a, b);
```

| Map메서드             | 설명                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------ |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.entries | Map 객체에서 요소키와 요소값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
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
