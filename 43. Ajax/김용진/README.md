# 43. Ajax

## 43.1 Ajaxλ?

> π‘ Ajaxλ μλ°μ€ν¬λ¦½νΈλ₯Ό μ¬μ©νμ¬ λΈλΌμ°μ κ° μλ²μκ² λΉλκΈ° λ°©μμΌλ‘ λ°μ΄ν°λ₯Ό μμ²­νκ³ , μλ²κ° μλ΅ν λ°μ΄ν°λ₯Ό μμ νμ¬ μΉνμ΄μ§λ₯Ό λμ μΌλ‘ κ°±μ νλ νλ‘κ·Έλλ° λ°©μ

**μ ν΅μ μΈ λ°©μ**

<img src="https://user-images.githubusercontent.com/55246584/156117065-2a718736-dd33-44fa-9ab5-4f20222fa2f4.png" width="400px"></img>

1. μ΄μ  μΉνμ΄μ§μ μ°¨μ΄κ° μμ΄μ λ³κ²½ν  νμκ° μλ λΆλΆκΉμ§ ν¬ν¨λ μμ ν HTMLμ μλ²λ‘λΆν° λ§€λ² λ€μ μ μ‘λ°κΈ° λλ¬Έμ λΆνμν λ°μ΄ν° ν΅μ μ΄ λ°μ
2. λ³κ²½ν  νμκ° μλ λΆλΆκΉμ§ μ²μλΆν° λ€μ λ λλ§ν¨. μ΄λ‘ μΈν΄ νλ©΄ μ νμ΄ μΌμ΄λλ©΄ νλ©΄μ΄ μκ°μ μΌλ‘ κΉλ°μ΄λ νμμ΄ λ°μ
3. ν΄λΌμ΄μΈνΈμ μλ²μμ ν΅μ μ΄ λκΈ° λ°©μμΌλ‘ λμνκΈ° λλ¬Έμ μλ²λ‘λΆν° μλ΅μ΄ μμ λκΉμ§ λ€μ μ²λ¦¬λ λΈλ‘νΉ

**Ajax**

<img src="https://user-images.githubusercontent.com/55246584/156117559-a8a3a67f-f90f-4be8-8231-402d88dc28f6.png" width="400px"></img>

1. λ³κ²½ν  λΆλΆμ κ°±μ νλ λ° νμν λ°μ΄ν°λ§ μλ²λ‘λΆν° μ μ‘λ°κΈ° λλ¬Έμ λΆνμν λ°μ΄ν° ν΅μ μ΄ λ°μνμ§ μμ
2. λ³κ²½ν  νμκ° μλ λΆλΆμ λ€μ λ λλ§νμ§ μμ. λ°λΌμ νλ©΄μ΄ μκ°μ μΌλ‘ κΉλ°μ΄λ νμμ΄ λ°μνμ§ μμ
3. ν΄λΌμ΄μΈνΈμ μλ²μμ ν΅μ μ΄ λΉλκΈ° λ°©μμΌλ‘ λμνκΈ° λλ¬Έμ μλ²μκ² μμ²­μ λ³΄λΈ μ΄ν λΈλ‘νΉμ΄ λ°μνμ§ μμ

## 43.2 JSON

> π‘ ν΄λΌμ΄μΈνΈμ μλ² κ°μ HTTP ν΅μ μ μν νμ€νΈ λ°μ΄ν° ν¬λ§·

**JSON νκΈ° λ°©μ**

> π‘ ν€μ κ°μΌλ‘ κ΅¬μ±λ μμν νμ€νΈ

```json
{
	"name": "Kim",
	"age": 20,
	"alive": true,
	"hobby": ["traveling", "tennis"]
}
```

**JSON.stringify**

> π‘ κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν

- ν΄λΌμ΄μΈνΈκ° μλ²λ‘ κ°μ²΄λ₯Ό μ μ‘νλ €λ©΄ κ°μ²΄λ₯Ό λ¬Έμμ΄ν΄μΌ νλλ° μ΄λ₯Ό `μ§λ ¬ν`λΌκ³  ν¨

```js
const obj = {
	name: 'kim',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"kim","age":20,"alive":true,"hobby":["traveling","tennis"]}

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ννλ©΄μ λ€μ¬μ°κΈ° ν¨
const prettyJson = JSON.stringify(obj, null, 2);
/*
string {
  "name": "kim",
  "age": 20,
  "alive: true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
 */

// replacer ν¨μ. κ°μ νμμ΄ Numberμ΄λ©΄ νν°λ§λμ΄ λ°νλμ§ μμ
function filter(key, value) {
	// undefined: λ°ννμ§ μμ
	return typeof value === 'number' ? undefined : value;
}

const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredObject, strFilteredObject);
/*
string {
  "name": "kim",
  "alive: true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
 */
```

- κ°μ²΄λΏλ§ μλλΌ λ°°μ΄λ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν

```js
const todos = [
	{ id: 1, content: 'HTML', completed: false },
	{ id: 2, content: 'CSS', completed: true },
	{ id: 3, content: 'JavaScript', completed: false },
];

// λ°°μ΄μ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);
/*
string [
  {
    "id": 1,
    "content": "HTML",
    "completed": false
  },
  ...
]
 */
```

**JSON.parse**

> π‘ JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄λ‘ λ³ν

- λ¬Έμμ΄μ κ°μ²΄λ‘μ μ¬μ©νλ €λ©΄ JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄νν΄μΌ νλλ° μ΄λ₯Ό `μ­μ§λ ¬ν`λΌκ³  ν¨

```js
const obj = {
	name: 'kim',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

// κ°μ²΄λ₯Ό JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄λ‘ λ³ν
const json = JSON.stringify(obj);

// JSON ν¬λ§·μ λ¬Έμμ΄μ κ°μ²΄λ‘ λ³ν
const parsed = JSON.parse(json);

console.log(typeof parsed);
// object
```

- λ°°μ΄μ΄ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³νλμ΄ μλ κ²½μ° λ¬Έμμ΄μ λ°°μ΄ κ°μ²΄λ‘ λ³ν
- λ°°μ΄μ μμκ° κ°μ²΄μΈ κ²½μ° λ°°μ΄μ μμκΉμ§ κ°μ²΄λ‘ λ³ν

```js
const todos = [
	{ id: 1, content: 'HTML', completed: false },
	{ id: 2, content: 'CSS', completed: true },
	{ id: 3, content: 'JavaScript', completed: false },
];

// λ°°μ΄μ JSON ν¬λ§·μ λ¬Έμμ΄λ‘ λ³ν
const json = JSON.stringify(todos);

// JSON ν¬λ§·μ λ¬Έμμ΄μ λ°°μ΄λ‘ λ³ν. λ°°μ΄μ μμκΉμ§ κ°μ²΄λ‘ λ³ν
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
/*
object [
  { id: 1, content: 'HTML', completed: false },
  ...
]
 */
```

## 43.3 XMLHttpRequest

> π‘ Web APIμΈ XMLHttpRequest κ°μ²΄λ HTTP μμ²­ μ μ‘κ³Ό HTTP μμ μ μν λ€μν λ©μλμ νλ‘νΌν°λ₯Ό μ κ³΅

### XMLHttpRequest κ°μ²΄ μμ±

```js
const xhr = new XHLHttpRequest();
```

### XMLHttpRequest κ°μ²΄μ νλ‘νΌν°μ λ©μλ

**XMLHttpRequest κ°μ²΄μ νλ‘ν νμ νλ‘νΌν°**

| νλ‘ν νμ νλ‘νΌν° | μ€λͺ                                                                                                                        |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| readyState          | HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ μ μ </br> UNSENT: 0</br> OPENED: 1</br> HEADERS_RECEIVED: 2</br> LOADING: 3 </br> DONE: 4 |
| status              | HTTP μμ²­μ λν μλ΅ μνλ₯Ό λνλ΄λ μ μ                                                                                  |
| statusText          | HTTP μμ²­μ λν μλ΅ λ©μμ§λ₯Ό λνλ΄λ λ¬Έμμ΄ </br> μ) "OK"                                                               |
| responseType        | HTTP μλ΅ νμ </br> μ) document, json, text, blob, arraybuffer                                                            |
| response            | HTTP μμ²­μ λν μλ΅ λͺΈμ²΄. responseType λ°λΌ νμμ΄ λ€λ¦                                                                   |
| responseText        | μλ²κ° μ μ‘ν HTTP μμ²­μ λν μλ΅ λ¬Έμμ΄                                                                                  |

**XMLHttpRequest κ°μ²΄μ μ΄λ²€νΈ νΈλ€λ¬ νλ‘νΌν°**

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

**XMLHttpRequest κ°μ²΄μ λ©μλ**

| λ©μλ           | μ€λͺ                                     |
| ---------------- | ---------------------------------------- |
| open             | HTTP μμ²­ μ΄κΈ°ν                         |
| send             | HTTP μμ²­ μ μ‘                           |
| abort            | μ΄λ―Έ μ μ‘λ HTTP μμ²­ μ€λ¨               |
| setRequestHeader | νΉμ  HTTP μμ²­ ν€λμ κ°μ μ€μ           |
| getRequestHeader | νΉμ  HTTP μμ²­ ν€λμ κ°μ λ¬Έμμ΄λ‘ λ°ν |

**XMLHttpRequest κ°μ²΄μ μ μ  νλ‘νΌν°**

| μ μ  νλ‘νΌν°    | κ°  | μ€λͺ                                   |
| ---------------- | --- | -------------------------------------- |
| UNSENT           | 0   | open λ©μλ νΈμΆ μ΄μ                   |
| OPENED           | 1   | open λ©μλ νΈμΆ μ΄ν                  |
| HEADERS_RECEIVED | 2   | send λ©μλ νΈμΆ μ΄ν                  |
| LOADING          | 3   | μλ² μλ΅ μ€ (μλ΅ λ°μ΄ν° λ―Έμμ± μν) |
| DONE             | 4   | μλ² μλ΅ μλ£                         |

### HTTP μμ²­ μ μ‘

1. XMLHttpRequest.prototype.open λ©μλλ‘ HTTP μμ²­μ μ΄κΈ°ν
2. νμμ λ°λΌ XMLHttpRequest.prototype.setRequestHeader λ©μλλ‘ νΉμ  HTTP μμ²­μ ν€λ κ°μ μ€μ 
3. XMLHttpRequest.prototype.send λ©μλλ‘ HTTP μμ²­μ μ μ‘

```js
// XMLHttpRequest κ°μ²΄ μμ±
const xhr = new XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
xhr.open('GET', '/users');

// HTTP μμ²­ ν€λ μ€μ 
// ν΄λΌμ΄μΈνΈκ° μλ²λ‘ μ μ‘ν  λ°μ΄ν°μ MIME νμ μ§μ : json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP μμ²­ μ μ‘
xhr.send();
```

**XMLHttpRequest.prototype.open**

> xhr.open(method, url[, async])

| λ§€κ°λ³μ | μ€λͺ                                                             |
| -------- | ---------------------------------------------------------------- |
| method   | HTTP μμ²­ λ©μλ ("GET", "POST", "PUT", "DELETE" λ±)             |
| url      | HTTP μμ²­μ μ μ‘ν  URL                                           |
| async    | λΉλκΈ° μμ²­ μ¬λΆλ‘ μ΅μ. κΈ°λ³Έκ°μ true μ΄λ©° λΉλκΈ° λ°©μμΌλ‘ λμ |

- HTTP μμ²­ λ©μλλ ν΄λΌμ΄μΈνΈκ° μλ²μκ² μμ²­μ μ’λ₯μ λͺ©μ (λ¦¬μμ€μ λν νμ)μ μλ¦¬λ λ°©λ²

| HTTP μμ²­ λ©μλ | μ’λ₯           | λͺ©μ                   | νμ΄λ‘λ |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | λͺ¨λ /νΉμ  λ¦¬μμ€ μ·¨λ | X        |
| POST             | create         | λ¦¬μμ€ μμ±           | O        |
| PUT              | replace        | λ¦¬μμ€μ μ μ²΄ κ΅μ²΄    | O        |
| PATCH            | modify         | λ¦¬μμ€μ μΌλΆ μμ     | O        |
| DELETE           | delete         | λͺ¨λ /νΉμ  λ¦¬μμ€ μ­μ  | X        |

**XMLHttpRequest.prototype.send**

- open λ©μλλ‘ μ΄κΈ°νλ HTTP μμ²­μ μλ²λ‘ μ μ‘
  - GET μμ²­ λ©μλμ κ²½μ° λ°μ΄ν°λ₯Ό URLμ μΌλΆλΆμΈ μΏΌλ¦¬ λ¬Έμμ΄λ‘ μλ²μ μ μ‘
  - POST μμ²­ λ©μλμ κ²½μ° λ°μ΄ν°λ₯Ό μμ²­ λͺΈμ²΄μ λ΄μ μ μ‘

```jsx
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

**XMLHttpRequest.prototype.setRequestHeader**

- νΉμ  HTTP μμ²­μ ν€λ κ°μ μ€μ 

| MIME νμ   | μλΈνμ                                           |
| ----------- | -------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
| multipart   | multipart/formed-data                              |

```js
const xhr = new XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
xhr.open('POST', '/users');

// HTTP μμ²­ ν€λ μ€μ 
// ν΄λΌμ΄μΈνΈκ° μλ²λ‘ μ μ‘ν  λ°μ΄ν°μ MIME νμ μ§μ : json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP μμ²­ μ μ‘
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

- HTTP ν΄λΌμ΄μΈνΈκ° μλ²μ μμ²­ν  λ μλ²κ° μλ΅ν  λ°μ΄ν°μ MIME νμμ Acceptλ‘ μ§μ ν  μ μμ

```js
// μλ²κ° μλ΅ν  λ°μ΄ν°μ MIME νμ μ§μ : json
xhr.setRequestHeader('accept', 'application/json');
```

### HTTP μλ΅ μ²λ¦¬

> π‘ μλ²κ° μ μ‘ν μλ΅μ μ²λ¦¬νλ €λ©΄ XMLHttpRequest κ°μ²΄κ° λ°μμν€λ μ΄λ²€νΈλ₯Ό μΊμΉν΄μΌ ν¨

- HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ readyState νλ‘νΌν° κ°μ΄ λ³κ²½λ κ²½μ° λ°μνλ readystatechange μ΄λ²€νΈλ₯Ό μΊμΉνμ¬ HTTP μλ΅μ μ²λ¦¬ κ°λ₯

```js
const xhr = XMLHttpRequest();

// Fake REST API λ₯Ό μ κ³΅νλ μλΉμ€
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP μμ²­ μ μ‘
xhr.send();

// readystatechange μ΄λ²€νΈλ HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ΄λ
// readyState νλ‘νΌν°κ° λ³κ²½λ  λλ§λ€ λ°μ
xhr.onreadystatechange = () => {
	// readyState νλ‘νΌν°λ HTTP μμ²­μ νμ¬ μνλ₯Ό λνλ
	// readyState νλ‘νΌν° κ°μ΄ 4 (XMLHttpRequest.DONE)κ° μλλ©΄ μλ² μλ΅μ΄ μλ£λμ§ μμ μν

	// λ§μ½ μλ² μλ΅μ΄ μμ§ μλ£λμ§ μμλ€λ©΄ μλ¬΄λ° μ²λ¦¬ X
	if (xhr.readyState !== XMLHttpRequest.DONE) return;

	// status νλ‘νΌν°λ μλ΅ μν μ½λλ₯Ό λνλ
	// μ μμ μΌλ‘ μλ΅λ μνλΌλ©΄ response νλ‘νΌν°μ μλ²μ μλ΅ κ²°κ³Όκ° λ΄κ²¨ μμ
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};
```

- readystatechange μ΄λ²€νΈ λμ  load μ΄λ²€νΈλ₯Ό μΊμΉν΄λ λ¨
- load μ΄λ²€νΈλ HTTP μμ²­μ΄ μ±κ³΅μ μΌλ‘ μλ£λ κ²½μ° λ°μ

```js
const xhr = XMLHttpRequest();

// HTTP μμ²­ μ΄κΈ°ν
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP μμ²­ μ μ‘
xhr.send();

// load μ΄λ²€νΈλ HTTP μμ²­μ΄ μ±κ³΅μ μΌλ‘ μλ£λ κ²½μ° λ°μ
xhr.onload = () => {
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};
```

### μ°Έκ³ 

- https://poiemaweb.com/js-ajax
