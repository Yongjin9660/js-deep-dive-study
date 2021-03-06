# 37. Setκ³Ό Map

## 37.1 Set

> π‘ Set κ°μ²΄λ μ€λ³΅λμ§ μλ μ μΌν κ°λ€μ μ§ν©

| κ΅¬λΆ                                | λ°°μ΄ | Set κ°μ²΄ |
| ----------------------------------- | :--: | :------: |
| λμΌν κ°μ μ€λ³΅νμ¬ ν¬ν¨ν  μ μμ |  O   |    X     |
| μμ μμμ μλ―Έκ° μμ             |  O   |    X     |
| μΈλ±μ€λ‘ μμμ μ κ·Ό κ°λ₯           |  O   |    X     |

### Set κ°μ²΄μ μμ±

> π‘ Set μμ±μ ν¨μλ μ΄ν°λ¬λΈμ μΈμλ‘ μ λ¬λ°μ Set κ°μ²΄λ₯Ό μμ±

```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

- μ€λ³΅μ νμ©νμ§ μλ Set κ°μ²΄μ νΉμ±μ νμ©νμ¬ λ°°μ΄μμ μ€λ³΅λ μμλ₯Ό μ κ±°

```js
// λ°°μ΄μ μ€λ³΅ μμ μ κ±°
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);

// Setμ μ¬μ©ν λ°°μ΄μ μ€λ³΅ μμ μ κ±°
const uniq = (array) => [...new Set(array)];
```

### μμ κ°μ νμΈ

> π‘ Set κ°μ²΄μ μμ κ°μλ₯Ό νμΈν  λλ Set.prototype.size νλ‘νΌν°λ₯Ό μ¬μ©

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### μμ μΆκ°

> π‘ Set κ°μ²΄μ μμλ₯Ό μΆκ°ν  λλ Set.prototype.add λ©μλλ₯Ό μ¬μ©

```js
const set = new Set();
console.log(set); // Set(0)

set.add(1);
console.log(set); // Set(1) {1}
```

- Set κ°μ²΄λ κ°μ²΄λ λ°°μ΄κ³Ό κ°μ΄ μλ°μ€ν¬λ¦½νΈμ λͺ¨λ  κ°μ μμλ‘ μ μ₯ν  μ μμ

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

### μμ μ‘΄μ¬ μ¬λΆ νμΈ

> π‘ Set κ°μ²΄μ νΉμ  μμκ° μ‘΄μ¬νλμ§ νμΈνλ €λ©΄ Set.prototype.has λ©μλλ₯Ό μ¬μ©

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### μμ μ­μ 

> π‘ Set κ°μ²΄μ νΉμ  μμλ₯Ό μ­μ νλ €λ©΄ Set.prototype.delete λ©μλλ₯Ό μ¬μ©

```js
const set = new Set([1, 2, 3]);

// μμ 2λ₯Ό μ­μ 
set.delete(2);
console.log(set); // Set(2) {1, 3}

// μ‘΄μ¬νμ§ μλ Set κ°μ²΄μ μμλ₯Ό μ­μ νλ € νλ©΄ μλ¬ μμ΄ λ¬΄μ
set.delete(0);
console.log(set); // Set(2) {1, 3}
```

- delete λ©μλλ μ­μ  μ±κ³΅ μ¬λΆλ₯Ό λνλ΄λ λΆλ¦¬μΈ κ°μ λ°ν

```js
const set = new Set([1, 2, 3]);

// deleteλ λΆλ¦¬μΈ κ°μ λ°ν
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```

### μμ μΌκ΄ μ­μ 

> π‘ Set κ°μ²΄μ λͺ¨λ  μμλ₯Ό μΌκ΄ μ­μ νλ €λ©΄ Set.prototype.clear λ©μλλ₯Ό μ¬μ©

```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### μμ μν

> π‘ Set κ°μ²΄μ μμλ₯Ό μννλ €λ©΄ Set.prototype.forEach λ©μλλ₯Ό μ¬μ©

- μ²« λ²μ§Έ μΈμ: νμ¬ μν μ€μΈ μμκ°
- λ λ²μ§Έ μΈμ: νμ¬ μν μ€μΈ μμκ°
- μΈ λ²μ§Έ μΈμ: νμ¬ μν μ€μΈ Set κ°μ²΄ μμ²΄

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
 */
```

- **Set κ°μ²΄λ μ΄ν°λ¬λΈ**
  - for ... of λ¬ΈμΌλ‘ μν κ°λ₯
  - μ€νλ λ λ¬Έλ²κ³Ό λ°°μ΄ λμ€νΈλ­μ²λ§μ λμμ΄ λ  μ μμ

```js
const set = new Set([1, 2, 3]);

// Set κ°μ²΄λ Set.prototypeμ Symbol.iterator λ©μλλ₯Ό μμλ°λ μ΄ν°λ¬λΈ
console.log(Symbol.iterator in set); // true

for (const value of set) {
	console.log(value); // 1 2 3
}

// μ΄ν°λ¬λΈμΈ Set κ°μ²΄λ μ€νλ λ λ¬Έλ²μ λμμ΄ λ  μ μμ
console.log([...set]); // [1, 2, 3]

// μ΄ν°λ¬λΈμΈ Set κ°μ²΄λ λ°°μ΄ λμ€νΈλ­μ²λ§ ν λΉμ λμμ΄ λ¨
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

π Set κ°μ²΄λ μμμ μμμ μλ―Έλ₯Ό κ°μ§ μμ§λ§ Set κ°μ²΄λ₯Ό μννλ μμλ μμκ° μΆκ°λ μμλ₯Ό λ°λ¦

### μ§ν© μ°μ°

> π‘ Set κ°μ²΄λ₯Ό ν΅ν΄ κ΅μ§ν©, ν©μ§ν©, μ°¨μ§ν© λ±μ κ΅¬ν κ°λ₯

**κ΅μ§ν©**

```js
Set.prototype.intersection = function (set) {
	const result = new Set();

	for (const value of set) {
		// 2κ°μ setμ μμκ° κ³΅ν΅λλ μμμ΄λ©΄ κ΅μ§ν©μ λμ
		if (this.has(value)) result.add(value);
	}

	return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setAμ setBμ κ΅μ§ν©
console.log(setA.intersection(setB)); // Set(2) {2, 4}
```

μλμ κ°μ λ°©λ²μΌλ‘λ κ°λ₯

```js
Set.prototype.intersection = function (set) {
	return new Set([...this].filter((v) => set.has(v)));
};
```

**ν©μ§ν©**

```js
Set.prototype.union = function (set) {
	// this(Set κ°μ²΄)λ₯Ό λ³΅μ¬
	const result = new Set(this);

	for (const value of set) {
		result.add(value);
	}

	return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setAμ setBμ ν©μ§ν©
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
```

μλμ κ°μ λ°©λ²μΌλ‘λ κ°λ₯

```js
Set.prototype.union = function (set) {
	return new Set([...this, ...set]);
};
```

**μ°¨μ§ν©**

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

μλμ κ°μ λ°©λ²μΌλ‘λ κ°λ₯

```js
Set.prototype.difeerence = function (set) {
	return new Set([...this].filter((v) => !set.has(v)));
};
```

**λΆλΆ μ§ν©κ³Ό μμ μ§ν©**

```js
// thisκ° subsetμ μμ μ§ν©μΈμ§ νμΈ
Set.prototype.isSuperset = function (subset) {
	for (const value of subset) {
		// supersetμ λͺ¨λ  μμκ° subsetμ λͺ¨λ  μμλ₯Ό ν¬ν¨νλμ§ νμΈ
		if (!this.has(value)) return false;
	}
	return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setAκ° setBμ μμ μ§ν©μΈμ§ νμΈ
console.log(setA.isSuperset(setB)); // true
```

μλμ κ°μ λ°©λ²μΌλ‘λ κ°λ₯

```js
Set.prototype.isSuperset = function (subset) {
	const supersetArr = [...this];
	return [...subset].every((v) => supersetArr.includes(v));
};
```

## 37.2 Map

> π‘ Map κ°μ²΄λ ν€μ κ°μ μμΌλ‘ μ΄λ£¨μ΄μ§ μ»¬λ μ

| κ΅¬λΆ                   |          κ°μ²΄           |       Map κ°μ²΄        |
| ---------------------- | :---------------------: | :-------------------: |
| ν€λ‘ μ¬μ©ν  μ μλ κ° |   λ¬Έμμ΄ λλ μ¬λ² κ°   | κ°μ²΄λ₯Ό ν¬ν¨ν λͺ¨λ  κ° |
| μ΄ν°λ¬λΈ               |            X            |           O           |
| μμ κ°μ νμΈ         | Object.keys(obj).length |       map.size        |

### Map κ°μ²΄μ μμ±

> π‘ Map κ°μ²΄λ Map μμ±μ ν¨μλ‘ μμ±

- Map μμ±μ ν¨μλ μ΄ν°λ¬λΈμ μΈμλ‘ μ λ¬λ°μ Map κ°μ²΄λ₯Ό μμ±
  - μΈμλ‘ μ λ¬λλ μ΄ν°λ¬λΈμ ν€μ κ°μ μμΌλ‘ μ΄λ£¨μ΄μ§ μμλ‘ κ΅¬μ±λμ΄μΌ ν¨

```js
const map1 = new Map([
	['key1', 'value1'],
	['key2', 'value2'],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeEror: Iterator value 1 is not an entry object
```

### μμ κ°μ

> π‘ Map κ°μ²΄μ μμ κ°μλ₯Ό νμΈν  λλ Map.prototype.size νλ‘νΌν°λ₯Ό μ¬μ©

```js
const { size } = new Map([
	['key1', 'value1'],
	['key2', 'value2'],
]);
console.log(size); // 2
```

### μμ μΆκ°

> π‘ Map κ°μ²΄μ μμλ₯Ό μΆκ°ν  λλ Map.prototype.set λ©μλλ₯Ό μ¬μ©

```js
const map = new Map();

map.set('key1', 'value1').set('key2', 'value2');
console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```

### μμ μ·¨λ

> π‘ Map κ°μ²΄μμ νΉμ  μμλ₯Ό μ·¨λνλ €λ©΄ Map.prototype.get λ©μλλ₯Ό μ¬μ©

```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### μμ μ‘΄μ¬ μ¬λΆ νμΈ

> π‘ Map κ°μ²΄μ νΉμ  μμκ° μ‘΄μ¬νλμ§ νμΈνλ €λ©΄ Map.prototype.has λ©μλλ₯Ό μ¬μ©

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

### μμ μ­μ 

> π‘ Map κ°μ²΄μ μμλ₯Ό μ­μ νλ €λ©΄ Map.prototype.delete λ©μλλ₯Ό μ¬μ©

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

- delete λ©μλλ μ­μ  μ±κ³΅ μ¬λΆλ₯Ό λνλ΄λ λΆλ¦¬μΈ κ°μ λ°ν

  - μ°μ νΈμΆ λΆκ°λ₯

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

map.delete(lee).delete(kim); // TypeError: map.delete(...).delete is not a function
```

### μμ μΌκ΄ μ­μ 

> π‘ Map κ°μ²΄μ μμλ₯Ό μΌκ΄ μ­μ νλ €λ©΄ Map.prototype.clear λ©μλλ₯Ό μ¬μ©

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

### μμ μν

> π‘ Map κ°μ²΄μ μμλ₯Ό μννλ €λ©΄ Map.prototype.forEach λ©μλλ₯Ό μ¬μ©

- **μ²« λ²μ§Έ μΈμ**: νμ¬ μν μ€μΈ μμκ°
- **λ λ²μ§Έ μΈμ**: νμ¬ μν μ€μΈ μμν€
- **μΈ λ²μ§Έ μΈμ**: νμ¬ μν μ€μΈ Map κ°μ²΄ μμ²΄

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

**Map κ°μ²΄λ μ΄ν°λ¬λΈ**

- for ... of λ¬ΈμΌλ‘ μν κ°λ₯
- μ€νλ λ λ¬Έλ²κ³Ό λ°°μ΄ λμ€νΈλ­μ²λ§ ν λΉμ λμμ΄ λ¨

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

// Map κ°μ²΄λ Map.prototypeμ Symbol.iterator λ©μλλ₯Ό μμλ°λ μ΄ν°λ¬λΈ
console.log(Symbol.iterator in map); // true

// μ΄ν°λ¬λΈμΈ Map κ°μ²΄λ for ... of λ¬ΈμΌλ‘ μν κ°λ₯
for (const entry of map) {
	console.log(entry);
}

// μ΄ν°λ¬λΈμΈ Map κ°μ²΄λ μ€νλ λ λ¬Έλ²μ λμ
console.log([...map]);

// μ΄ν°λ¬λΈμΈ Map κ°μ²΄λ λ°°μ΄ λμ€νΈλ­μ²λ§ ν λΉμ λμ
const [a, b] = map;
console.log(a, b);
```

| Mapλ©μλ             | μ€λͺ                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------ |
| Map.prototype.keys    | Map κ°μ²΄μμ μμν€λ₯Ό κ°μΌλ‘ κ°λ μ΄ν°λ¬λΈμ΄λ©΄μ λμμ μ΄ν°λ μ΄ν°μΈ κ°μ²΄λ₯Ό λ°ν     |
| Map.prototype.values  | Map κ°μ²΄μμ μμκ°μ κ°μΌλ‘ κ°λ μ΄ν°λ¬λΈμ΄λ©΄μ λμμ μ΄ν°λ μ΄ν°μΈ κ°μ²΄λ₯Ό λ°ν     |
| Map.prototype.entries | Map κ°μ²΄μμ μμν€μ μμκ°μΌλ‘ κ°λ μ΄ν°λ¬λΈμ΄λ©΄μ λμμ μ΄ν°λ μ΄ν°μΈ κ°μ²΄λ₯Ό λ°ν |

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
	[lee, 'developer'],
	[kim, 'designer'],
]);

// Map.prototype.keysλ Map κ°μ²΄μμ μμν€λ₯Ό κ°μΌλ‘ κ°λ μ΄ν°λ μ΄ν°λ₯Ό λ°ν
for (const key of map.keys()) {
	console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.valuesλ Map κ°μ²΄μμ μμκ°μ κ°μΌλ‘ κ°λ μ΄ν°λ μ΄ν°λ₯Ό λ°ν
for (const value of map.values()) {
	console.log(value); // developer designer
}

// Map.prototype.entires λ Map κ°μ²΄μμ μμν€μ μμκ°μ κ°μΌλ‘ κ°λ μ΄ν°λ μ΄ν° λ°ν
for (const entry of map.entries()) {
	console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}
```
