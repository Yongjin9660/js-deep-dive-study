# 43. Ajax

## 43.1 Ajax λ?

> π‘ μλ°μ€ν¬λ¦½νΈλ₯Ό μ¬μ©νμ¬ λΈλΌμ°μ κ° μλ²μκ² λΉλκΈ° λ°©μμΌλ‘ λ°μ΄ν°λ₯Ό μμ²­νκ³ , μλ²κ° μλ΅ν λ°μ΄ν°λ₯Ό μμ νμ¬ μΉνμ΄μ§λ₯Ό λμ μΌλ‘ κ°±μ νλ νλ‘κ·Έλλ° λ°©μ

<br>

- Ajax λ λΈλΌμ°μ μμ μ κ³΅νλ Web API μΈ XMLHttpRequest κ°μ²΄λ₯Ό κΈ°λ°μΌλ‘ λμ
  - XMLHttpRequest λ HTTP λΉλκΈ° ν΅μ μ μν λ©μλμ νλ‘νΌν°λ₯Ό μ κ³΅

<br>

#### \*\* `Ajax λ±μ₯ μ΄μ `

<br>

<img style="height:450px;" src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png">

<br>

##### [ μ ν΅μ μΈ λ°©μμ λ¨μ  ]

- μ΄μ  μΉνμ΄μ§μ μ°¨μ΄κ° μμ΄μ λ³κ²½ν  νμκ° μλ λΆλΆκΉμ§ ν¬ν¨λ `μμ ν HTML μ μλ²λ‘λΆν° λ§€λ² λ€μ μ μ‘λ°κΈ° λλ¬Έμ λΆνμν λ°μ΄ν° ν΅μ μ΄ λ°μνλ€.`
- λ³κ²½ν  νμκ° μλ λΆλΆκΉμ§ μ²μλΆν° λ€μ λ λλ§νλ€. μ΄λ‘ μΈν΄ `νλ©΄ μ νμ΄ μΌμ΄λλ©΄ νλ©΄μ΄ μκ°μ μΌλ‘ κΉλ°μ΄λ νμμ΄ λ°μνλ€.`
- ν΄λΌμ΄μΈνΈμ μλ²μμ ν΅μ μ΄ λκΈ° λ°©μμΌλ‘ λμνκΈ° λλ¬Έμ `μλ²λ‘λΆν° μλ΅μ΄ μμ λκΉμ§ λ€μ μ²λ¦¬λ λΈλ‘νΉλλ€.`

<br>

---

<br>

#### \*\* `Ajax λ±μ₯ μ΄ν`

- **μλ²λ‘λΆν° μΉνμ΄μ§μ λ³κ²½μ νμν λ°μ΄ν°λ§ λΉλκΈ° λ°©μμΌλ‘ μ μ‘λ°μ νμ μ μΌλ‘ λ λλ§νλ λ°©μμ κ°λ₯νκ² ν¨.**
  - λΈλΌμ°μ μμλ λ°μ€ν¬ν± μ νλ¦¬μΌμ΄μκ³Ό μ μ¬ν λΉ λ₯Έ νΌν¬λ¨Όμ€μ λΆλλ¬μ΄ νλ©΄ μ νμ΄ κ°λ₯ν΄μ§.

<br>

<img style="height:450px;" src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png">

<br>

##### [ Ajax μ μ₯μ  ]

- `λ³κ²½ν  λΆλΆμ κ°±μ νλ λ° νμν λ°μ΄ν°λ§ μλ²λ‘λΆν° μ μ‘λ°κΈ° λλ¬Έμ λΆνμν λ°μ΄ν° ν΅μ μ΄ λ°μνμ§ μλλ€.`
- λ³κ²½ν  νμκ° μλ λΆλΆμ λ€μ λ λλ§νμ§ μλλ€. λ°λΌμ `νλ©΄μ΄ μκ°μ μΌλ‘ κΉλ°μ΄λ νμμ΄ λ°μνμ§ μλλ€.`
- ν΄λΌμ΄μΈνΈμ μλ²μμ ν΅μ μ΄ λΉλκΈ° λ°©μμΌλ‘ λμνκΈ° λλ¬Έμ `μλ²μκ² μμ²­μ λ³΄λΈ μ΄ν λΈλ‘νΉμ΄ λ°μνμ§ μλλ€.`

<br>

## 43.2 JSON

> π‘ JSON μ ν΄λΌμ΄μΈνΈμ μλ² κ°μ HTTP ν΅μ μ μν νμ€νΈ λ°μ΄ν° ν¬λ§·μ μλ―Έ
> β μλ°μ€ν¬λ¦½νΈμ μ’μλμ§ μλ μΈμ΄ λλ¦½ν λ°μ΄ν° ν¬λ§·μΌλ‘, λλΆλΆμ νλ‘κ·Έλλ° μΈμ΄μμ μ¬μ© κ°λ₯

<br>

#### 1) JSON νκΈ° λ°©μ

```jsx
{
  "name" : "Lee",
  "age" : 20,
  "alive" : true,
  "hobby" : ["traveling", "tennis"]
}
```

- JSON μ μλ°μ€ν¬λ¦½νΈμ κ°μ²΄ λ¦¬ν°λ΄κ³Ό μ μ¬νκ² **ν€μ κ°μΌλ‘ κ΅¬μ±λ μμν νμ€νΈ**
  - `ν€λ λ°λμ ν°λ°μ΄ν (μμ λ°μ΄ν X) λ‘ λ¬Άμ΄μΌ ν¨.`
  - `κ°μ κ°μ²΄ λ¦¬ν°λ΄κ³Ό κ°μ νκΈ°λ²μ κ·Έλλ‘ μ¬μ© κ°λ₯νμ§λ§, λ¬Έμμ΄μ λ°λμ ν°λ°μ΄ν (μμ λ°μ΄ν X) λ‘ λ¬Άμ΄μΌ ν¨.`

<br>

#### 2) JSON.stringify

> π‘ JSON.stringify λ©μλλ κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν

<br>

- ν΄λΌμ΄μΈνΈκ° μλ²λ‘ κ°μ²΄λ₯Ό μ μ‘νλ €λ©΄ κ°μ²΄λ₯Ό λ¬Έμμ΄νν΄μΌ νλλ°, μ΄λ₯Ό **`μ§λ ¬ν`** λΌκ³  λΆλ¦.

<br>

```jsx
const obj = {
	name: "Lee",
	age: 20,
	alive: true,
	hobby: ["traveling", "tennis"],
};

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee", "age":20, "alive":true, "hobby":["traveling", "tennis"]}

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ννλ©΄μ λ€μ¬μ°κΈ°
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/*
string {
  "name" : "Lee",
  "age" : 20,
  "alive" : true,
  "hobby" : [
    "traveling", 
    "tennis"
  ]
}
*/

// κ°μ νμμ΄ Number μ΄λ©΄ νν°λ§λμ΄ λ°νλμ§ μλ ν¨μ μ μ
function filter(key, value) {
	// undefined : λ°ννμ§ μμ
	return typeof value === "number" ? undefined : value;
}

// JSON.stringify λ©μλμ λ λ²μ§Έ μΈμλ‘ filter ν¨μ μ λ¬
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name" : "Lee",
  "alive" : true,
  "hobby" : [
    "traveling", 
    "tennis"
  ]
}
*/
```

```jsx
const todos = [
	{ id: 1, content: "HTML", completed: false },
	{ id: 2, content: "CSS", completed: true },
];

// λ°°μ΄μ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);

/*
string [
  { 
    "id" : 1,
    "content" : "HTML", 
    "completed" : false
  },
  { 
    "id" : 2,
    "content" : "CSS", 
    "completed" : true
  }
]
*/
```

- JSON.stringify λ©μλλ κ°μ²΄λΏλ§ μλλΌ λ°°μ΄λ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν κ°λ₯
  <br>

#### 3) JSON.parse

> π‘ JSON.parse λ©μλλ JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄λ λ°°μ΄λ‘ λ³ν

<br>

- μλ²λ‘λΆν° ν΄λΌμ΄μΈνΈμκ² μ μ‘λ JSON λ°μ΄ν°λ λ¬Έμμ΄
  - μ΄ λ¬Έμμ΄μ κ°μ²΄λ‘μ μ¬μ©νλ €λ©΄ JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄νν΄μΌ νλλ°, μ΄λ₯Ό **`μ­μ§λ ¬ν`** λΌκ³  λΆλ¦.

<br>

```jsx
const obj = {
	name: "Lee",
	age: 20,
	alive: true,
	hobby: ["traveling", "tennis"],
};

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(obj);

// JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄λ‘ λ³ν
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name:"Lee", age:20, alive:true, hobby:["traveling", "tennis"]}
```

```jsx
const todos = [
	{ id: 1, content: "HTML", completed: false },
	{ id: 2, content: "CSS", completed: true },
];

// λ°°μ΄μ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(todos);

// JSON ν¬λ§·μ λ¬Έμμ΄μ λ°°μ΄λ‘ λ³ν
// λ°°μ΄μ μμκΉμ§ κ°μ²΄λ‘ λ³ν
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);

/*
object [
  { id : 1, content : 'HTML', completed : false }, 
  { id : 2, content : 'CSS', completed : true }
]
*/
```

- JSON.parse λ λ°°μ΄μ΄ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³νλμ΄ μλ κ²½μ°μλ λ¬Έμμ΄μ λ°°μ΄ κ°μ²΄λ‘ λ³ν
  - λ°°μ΄μ μμκ° κ°μ²΄μΈ κ²½μ° λ°°μ΄μ μμκΉμ§ κ°μ²΄λ‘ λ³ν

<br>

## 43.3 XMLHttpRequest

- λΈλΌμ°μ λ μ£Όμμ°½μ΄λ HTML μ form νκ·Έ λλ a νκ·Έλ₯Ό ν΅ν΄ HTTP μμ²­ μ μ‘ κΈ°λ₯μ κΈ°λ³Έ μ κ³΅
- `μλ°μ€ν¬λ¦½νΈλ₯Ό μ¬μ©νμ¬ HTTP μμ²­μ μ μ‘νλ €λ©΄ XMLHttpRequest κ°μ²΄λ₯Ό μ¬μ©ν΄μΌ ν¨.`
  - Wep API μΈ XMLHttpRequest κ°μ²΄λ HTTP μμ²­ μ μ‘κ³Ό HTTP μλ΅ μμ μ μν λ€μν λ©μλμ νλ‘νΌν°λ₯Ό μ κ³΅

<br>

#### 1) XMLHttpRequest κ°μ²΄ μμ±

```jsx
// XMLHttpRequest κ°μ²΄ μμ±
const xhr = new XMLHttpRequest();
```

- XMLHttpRequest κ°μ²΄λ XMLHttpRequest μμ±μ ν¨μλ₯Ό νΈμΆνμ¬ μμ±
  - XMLHttpRequest κ°μ²΄λ λΈλΌμ°μ μμ μ κ³΅νλ Wep API μ΄λ―λ‘ λΈλΌμ°μ  νκ²½μμλ§ μ μμ μΌλ‘ μ€νλ¨.
    <br>

#### 2) XMLHttpRequest κ°μ²΄μ νλ‘νΌν°μ λ©μλ

##### 1) XMLHttpRequest κ°μ²΄μ νλ‘ν νμ νλ‘νΌν°

| νλ‘ν νμ νλ‘νΌν° | μ€λͺ                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| readyState          | HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ μ μ. λ€μκ³Ό κ°μ XMLHttpRequest μ μ μ  νλ‘νΌν°λ₯Ό κ°μΌλ‘ κ°λλ€. |
|                     | UNSENT : 0                                                                                          |
|                     | OPENED : 1                                                                                          |
|                     | HEADERS_RECEIVED : 2                                                                                |
|                     | LOADING : 3                                                                                         |
|                     | DONE : 4                                                                                            |
| status              | HTTP μμ²­μ λν μλ΅ μν (HTTP μν μ½λ) λ₯Ό λνλ΄λ μ μ                                        |
|                     | ex) 200                                                                                             |
| statusText          | HTTP μμ²­μ λν μλ΅ λ©μμ§λ₯Ό λνλ΄λ λ¬Έμμ΄                                                      |
|                     | ex) "OK"                                                                                            |
| responseType        | HTTP μλ΅ νμ                                                                                      |
|                     | ex) document, json, text, blob, arraybuffer                                                         |
| response            | HTTP μμ²­μ λν μλ΅ λͺΈμ²΄, responseType μ λ°λΌ νμμ΄ λ€λ₯΄λ€.                                     |
| responseText        | μλ²κ° μ μ‘ν HTTP μμ²­μ λν μλ΅ λ¬Έμμ΄                                                          |

<br>

##### 2) XMLHttpRequest κ°μ²΄μ μ΄λ²€νΈ νΈλ€λ¬ νλ‘νΌν°

| μ΄λ²€νΈ νΈλ€λ¬ νλ‘νΌν° | μ€λͺ                                                         |
| ---------------------- | ------------------------------------------------------------ |
| onreadystatechange     | readystate νλ‘νΌν° κ°μ΄ λ³κ²½λ κ²½μ°                         |
| onloadstart            | HTTP μμ²­μ λν μλ΅μ λ°κΈ° μμν κ²½μ°                     |
| onprogress             | HTTP μμ²­μ λν μλ΅μ λ°λ λμ€ μ£ΌκΈ°μ μΌλ‘ λ°μ            |
| onabort                | abort λ©μλμ μν΄ HTTP μμ²­μ΄ μ€λ¨λ κ²½μ°                  |
| onerror                | HTTP μμ²­μ μλ¬κ° λ°μν κ²½μ°                               |
| onload                 | HTTP μμ²­μ΄ μ±κ³΅μ μΌλ‘ μλ£ν κ²½μ°                           |
| ontimeout              | HTTP μμ²­ μκ°μ΄ μ΄κ³Όν κ²½μ°                                 |
| onloadend              | HTTP μμ²­μ΄ μλ£ν κ²½μ°. HTTP μμ²­μ΄ μ±κ³΅ λλ μ€ν¨νλ©΄ λ°μ |

<br>

##### 3) XMLHttpRequest κ°μ²΄μ λ©μλ

| λ©μλ           | μ€λͺ                                     |
| ---------------- | ---------------------------------------- |
| open             | HTTP μμ²­ μ΄κΈ°ν                         |
| send             | HTTP μμ²­ μ μ‘                           |
| abort            | μ΄λ―Έ μ μ‘λ HTTP μμ²­ μ€λ¨               |
| setRequestHeader | νΉμ  HTTP μμ²­ ν€λμ κ°μ μ€μ           |
| getRequestHeader | νΉμ  HTTP μμ²­ ν€λμ κ°μ λ¬Έμμ΄λ‘ λ°ν |

<br>

##### 4) XMLHttpRequest κ°μ²΄μ μ μ  νλ‘νΌν°

| μ μ  νλ‘νΌν°    | κ°  | μ€λͺ                                   |
| ---------------- | --- | -------------------------------------- |
| UNSENT           | 0   | open λ©μλ νΈμΆ μ΄μ                   |
| OPENED           | 1   | open λ©μλ νΈμΆ μ΄ν                  |
| HEADERS_RECEIVED | 2   | send λ©μλ νΈμΆ μ΄ν                  |
| LOADING          | 3   | μλ² μλ΅ μ€ (μλ΅ λ°μ΄ν° λ―Έμμ± μν) |
| DONE             | 4   | μλ² μλ΅ μλ£                         |

<br>

#### 3) HTTP μμ²­ μ μ‘

[ HTTP μμ²­μ μ μ‘νλ μμ]

```jsx
// XMLHttpRequest κ°μ²΄ μμ±
const xhr = new XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
xhr.open("GET", "/users");

// HTTP μμ²­ ν€λ μ€μ 
// ν΄λΌμ΄μΈνΈκ° μλ²λ‘ μ μ‘ν  λ°μ΄ν°μ MIME νμ μ§μ  : json
xhr.setRequestHeader("content-type", "application/json");

// HTTP μμ²­ μ μ‘
xhr.send();
```

1. XMLHttpRequest.prototype.open λ©μλλ‘ `HTTP μμ²­μ μ΄κΈ°ν`
2. νμμ λ°λΌ XMLHttpRequest.prototype.setRequestHeader λ©μλλ‘ `νΉμ  HTTP μμ²­μ ν€λ κ° μ€μ `
3. XMLHttpRequest.prototype.send λ©μλλ‘ `HTTP μμ²­ μ μ‘`

<br>

###### 1) `XMLHttpRequest.prototype.open`

- **open λ©μλλ μλ²μ μ μ‘ν  HTTP μμ²­μ μ΄κΈ°ν**

```jsx
xhr.open(method, url[, async])
```

<br>

| λ§€κ°λ³μ | μ€λͺ                                                             |
| -------- | ---------------------------------------------------------------- |
| method   | HTTP μμ²­ λ©μλ ("GET", "POST", "PUT", "DELETE" λ±)             |
| url      | HTTP μμ²­μ μ μ‘ν  URL                                           |
| async    | λΉλκΈ° μμ²­ μ¬λΆλ‘ μ΅μ. κΈ°λ³Έκ°μ true μ΄λ©° λΉλκΈ° λ°©μμΌλ‘ λμ |

<br>

**[ HTTP μμ²­ λ©μλ ]**

| HTTP μμ²­ λ©μλ | μ’λ₯           | λͺ©μ                   | νμ΄λ‘λ |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | λͺ¨λ /νΉμ  λ¦¬μμ€ μ·¨λ | X        |
| POST             | create         | λ¦¬μμ€ μμ±           | O        |
| PUT              | replace        | λ¦¬μμ€μ μ μ²΄ κ΅μ²΄    | O        |
| PATCH            | modify         | λ¦¬μμ€μ μΌλΆ μμ     | O        |
| DELETE           | delete         | λͺ¨λ /νΉμ  λ¦¬μμ€ μ­μ  | X        |

- HTTP μμ²­ λ©μλλ ν΄λΌμ΄μΈνΈκ° μλ²μκ² μμ²­μ μ’λ₯μ λͺ©μ  (λ¦¬μμ€μ λν νμ) μ μλ¦¬λ λ°©λ²
- μ£Όλ‘ 5κ°μ§ μμ²­ λ©μλ (GET, POST, PUT, PATCH, DELETE) λ₯Ό μ¬μ©νμ¬ CRUD κ΅¬ν
  <br>

##### 2) `XMLHttpRequest.prototype.send`

- **send λ©μλλ open λ©μλλ‘ μ΄κΈ°νλ HTTP μμ²­μ μλ²μ μ μ‘**
- μλ²λ‘ μ μ‘νλ λ°μ΄ν°λ GET, POST μμ²­ λ©μλμ λ°λΌ μ μ‘ λ°©μμ μ°¨μ΄ O
  - `GET` μμ²­ λ©μλμ κ²½μ° λ°μ΄ν°λ₯Ό URL μ μΌλΆλΆμΈ μΏΌλ¦¬ λ¬Έμμ΄λ‘ μλ²μ μ μ‘
  - `POST` μμ²­ λ©μλμ κ²½μ° λ°μ΄ν° (νμ΄λ‘λ) λ₯Ό μμ²­ λͺΈμ²΄μ λ΄μ μ μ‘

<br>

```jsx
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- **send λ©μλμλ μμ²­ λͺΈμ²΄μ λ΄μ μ μ‘ν  λ°μ΄ν° (νμ΄λ‘λ) λ₯Ό μΈμλ‘ μ λ¬ κ°λ₯**
  - `νμ΄λ‘λκ° κ°μ²΄μΈ κ²½μ° λ°λμ JSON.stringify λ©μλλ₯Ό μ¬μ©νμ¬ μ§λ ¬νν λ€μ μ λ¬ν΄μΌ ν¨.`
    <br>
- HTTP μμ²­ λ©μλκ° GET μΈ κ²½μ° send λ©μλμ νμ΄λ‘λλ‘ μ λ¬ν μΈμλ λ¬΄μλκ³  μμ²­ λͺΈμ²΄λ null λ‘ μ€μ λ¨.
  <br>

##### 3) `XMLHttpRequest.prototype.setRequestHeader`

- **setRequestHeader λ©μλλ νΉμ  HTTP μμ²­μ ν€λ κ°μ κ²°μ **
- λ°λμ open λ©μλλ₯Ό νΈμΆν μ΄νμ νΈμΆν΄μΌ ν¨.
- μμ£Ό μ¬μ©νλ HTTP μμ²­ λ©μλ : Content-type, Accept
  - `Content-type` : μμ²­ λͺΈμ²΄μ λ΄μ μ μ‘ν  λ°μ΄ν°μ MIME νμμ μ λ³΄λ₯Ό νν
  - `Accept` : HTTP ν΄λΌμ΄μΈνΈκ° μλ²μ μμ²­ν  λ μλ²κ° μλ΅ν  λ°μ΄ν°μ MIME νμ μ§μ 
    <br>

##### β» μμ£Ό μ¬μ©λλ MIME νμ

| MIME νμ   | μλΈνμ                                           |
| ----------- | -------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
| multipart   | multipart/formed-data                              |

<br>

```jsx
// ν΄λΌμ΄μΈνΈκ° μλ²λ‘ μ μ‘ν  λ°μ΄ν°μ MIME νμ μ§μ  : json
xhr.setRequestHeader("content-type", "application/json");

// μλ²κ° μλ΅ν  λ°μ΄ν°μ MIME νμ μ§μ  : json
xhr.setRequestHeader("accept", "application/json");
```

<br>

#### 4) HTTP μλ΅ μ²λ¦¬

- **μλ²κ° μ μ‘ν μλ΅μ μ²λ¦¬νλ €λ©΄ XMLHttpRequest κ°μ²΄κ° λ°μμν€λ μ΄λ²€νΈλ₯Ό μΊμΉν΄μΌ ν¨.**

  - readyState νλ‘νΌν° κ°μ΄ λ³κ²½λ κ²½μ° λ°μνλ `readystatechange` μ΄λ²€νΈλ₯Ό μΊμΉνμ¬ HTTP μλ΅ μ²λ¦¬ κ°λ₯
    <br>

- HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ readyState νλ‘νΌν° κ°μ΄ λ³κ²½λ κ²½μ° λ°μνλ readystatechange μ΄λ²€νΈλ₯Ό μΊμΉνμ¬ HTTP μλ΅μ μ²λ¦¬ κ°λ₯

<br>

```jsx
//  XMLHttpRequest κ°μ²΄ μμ±
const xhr = new XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
// https://jsonplaceholder.typicode.com μ Fake REST API λ₯Ό μ κ³΅νλ μλΉμ€
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP μμ²­ μ μ‘
xhr.send();

// readystatechange μ΄λ²€νΈλ HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ
// readyState νλ‘νΌν°κ° λ³κ²½λ  λλ§λ€ λ°μ
xhr.onreadystatechange = () => {
	// readyState νλ‘νΌν°λ HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ.
	// readyState νλ‘νΌν° κ°μ΄ 4 (XMLHttpRequest.DONE) κ° μλλ©΄ μλ² μλ΅μ΄ μλ£λμ§ μμ μν

	// λ§μ½ μλ² μλ΅μ΄ μμ§ μλ£λμ§ μμλ€λ©΄ μλ¬΄λ° μ²λ¦¬ X
	if (xhr.readyState !== XMLHttpRequest.DONE) return;

	// status νλ‘νΌν°λ μλ΅ μν μ½λλ₯Ό λνλ.
	// status νλ‘νΌν° κ°μ΄ 200 μ΄λ©΄ μ μμ μΌλ‘ μλ΅ν μνμ΄κ³ 
	// status νλ‘νΌν° κ°μ΄ 200 μ΄ μλλ©΄ μλ¬κ° λ°μν μν
	// μ μμ μΌλ‘ μλ΅λ μνλΌλ©΄ response νλ‘νΌν°μ μλ²μ μλ΅ κ²°κ³Όκ° λ΄κ²¨ μμ.
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error("Error", xhr.status, xhr.statusText);
	}
};
```

- **send λ©μλλ₯Ό ν΅ν΄ HTTP μμ²­μ μλ²μ μ μ‘νλ©΄ μλ²λ μλ΅μ λ°ν**

  - ν΄λΌμ΄μΈνΈμκ² μΈμ  μλ΅μ΄ λλ¬ν  μ§λ μ μ μμΌλ―λ‘ readystatechange μ΄λ²€νΈλ₯Ό ν΅ν΄ HTTP μμ²­μ νμ¬ μνλ₯Ό νμΈ
    <br>

- **xhr.readyState κ° XMLHttpRequest.DONE μΈμ§ νμΈνμ¬ μλ²μ μλ΅μ΄ μλ£λμλμ§ νμΈ**
  - μλ²μ μλ΅μ΄ μλ£λλ©΄ xhr.status κ° 200 μΈμ§ νμΈνμ¬ μ μ μ²λ¦¬μ μλ¬ μ²λ¦¬λ₯Ό κ΅¬λΆ
  - `μ μ μ²λ¦¬` : xhr.response μ μλ²κ° μ μ‘ν λ°μ΄ν°λ₯Ό μ·¨λ
  - `μλ¬ μ²λ¦¬` : μλ¬κ° λ°μν μνμ΄λ―λ‘ νμν μλ¬ μ²λ¦¬ μν

<br>

```jsx
//  XMLHttpRequest κ°μ²΄ μμ±
const xhr = new XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
// https://jsonplaceholder.typicode.com μ Fake REST API λ₯Ό μ κ³΅νλ μλΉμ€
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP μμ²­ μ μ‘
xhr.send();

// load μ΄λ²€νΈλ HTTP μμ²­μ΄ μ±κ³΅μ μΌλ‘ μλ£λ κ²½μ° λ°μ
xhr.onload = () => {
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error("Error", xhr.status, xhr.statusText);
	}
};
```

- **readystatechance μ΄λ²€νΈ λμ  load μ΄λ²€νΈλ₯Ό ν΅ν΄ μΊμΉνλ©΄ xhr.readyState κ° XMLHttpRequest.DONE μΈμ§ νμΈν  νμ X**
  - load μ΄λ²€νΈλ HTTP μμ²­μ΄ μ±κ³΅μ μΌλ‘ μλ£λ κ²½μ°μλ§ λ°μνκΈ° λλ¬Έ
