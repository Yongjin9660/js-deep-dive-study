# 43. Ajax

## 43.1 Ajax란?

> 💡 Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식

**전통적인 방식**

<img src="https://user-images.githubusercontent.com/55246584/156117065-2a718736-dd33-44fa-9ab5-4f20222fa2f4.png" width="400px"></img>

1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생
2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링함. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생
3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹

**Ajax**

<img src="https://user-images.githubusercontent.com/55246584/156117559-a8a3a67f-f90f-4be8-8231-402d88dc28f6.png" width="400px"></img>

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않음. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않음
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 떄문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않음

## 43.2 JSON

> 💡 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷

**JSON 표기 방식**

> 💡 키와 값으로 구성된 순수한 텍스트

```json
{
	"name": "Kim",
	"age": 20,
	"alive": true,
	"hobby": ["traveling", "tennis"]
}
```

**JSON.stringify**

> 💡 객체를 JSON 포맷의 문자열로 변환

- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열해야 하는데 이를 `직렬화`라고 함

```js
const obj = {
	name: 'kim',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"kim","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 함
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

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않음
function filter(key, value) {
	// undefined: 반환하지 않음
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

- 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환

```js
const todos = [
	{ id: 1, content: 'HTML', completed: false },
	{ id: 2, content: 'CSS', completed: true },
	{ id: 3, content: 'JavaScript', completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환
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

> 💡 JSON 포맷의 문자열을 객체로 변환

- 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 `역직렬화`라고 함

```js
const obj = {
	name: 'kim',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis'],
};

// 객체를 JSON 포맷의 문자열을 객체로 변환
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환
const parsed = JSON.parse(json);

console.log(typeof parsed);
// object
```

- 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 문자열을 배열 객체로 변환
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

```js
const todos = [
	{ id: 1, content: 'HTML', completed: false },
	{ id: 2, content: 'CSS', completed: true },
	{ id: 3, content: 'JavaScript', completed: false },
];

// 배열을 JSON 포맷의 문자열로 변환
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환. 배열의 요소까지 객체로 변환
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

> 💡 Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 수신을 위한 다양한 메서드와 프로퍼티를 제공

### XMLHttpRequest 객체 생성

```js
const xhr = new XHLHttpRequest();
```

### XMLHttpRequest 객체의 프로퍼티와 메서드

**XMLHttpRequest 객체의 프로토타입 프로퍼티**

| 프로토타입 프로퍼티 | 설명                                                                                                                        |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| readyState          | HTTP 요청의 현재 상태를 나타내는 정수 </br> UNSENT: 0</br> OPENED: 1</br> HEADERS_RECEIVED: 2</br> LOADING: 3 </br> DONE: 4 |
| status              | HTTP 요청에 대한 응답 상태를 나타내는 정수                                                                                  |
| statusText          | HTTP 요청에 대한 응답 메시지를 나타내는 문자열 </br> 예) "OK"                                                               |
| responseType        | HTTP 응답 타입 </br> 예) document, json, text, blob, arraybuffer                                                            |
| response            | HTTP 요청에 대한 응답 몸체. responseType 따라 타입이 다름                                                                   |
| responseText        | 서버가 전송한 HTTP 요청에 대한 응답 문자열                                                                                  |

**XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**

| 이벤트 핸들러 프로퍼티 | 설명                                                         |
| ---------------------- | ------------------------------------------------------------ |
| onreadystatechange     | readystate 프로퍼티 값이 변경된 경우                         |
| onloadstart            | HTTP 요청에 대한 응답을 받기 시작한 경우                     |
| onprogress             | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생            |
| onabort                | abort 메서드에 의해 HTTP 요청이 중단된 경우                  |
| onerror                | HTTP 요청에 에러가 발생한 경우                               |
| onload                 | HTTP 요청이 성공적으로 완료한 경우                           |
| ontimeout              | HTTP 요청 시간이 초과한 경우                                 |
| onloadend              | HTTP 요청이 완료한 경우. HTTP 요청이 성공 또는 실패하면 발생 |

**XMLHttpRequest 객체의 메서드**

| 메서드           | 설명                                     |
| ---------------- | ---------------------------------------- |
| open             | HTTP 요청 초기화                         |
| send             | HTTP 요청 전송                           |
| abort            | 이미 전송된 HTTP 요청 중단               |
| setRequestHeader | 특정 HTTP 요청 헤더의 값을 설정          |
| getRequestHeader | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

**XMLHttpRequest 객체의 정적 프로퍼티**

| 정적 프로퍼티    | 값  | 설명                                   |
| ---------------- | --- | -------------------------------------- |
| UNSENT           | 0   | open 메서드 호출 이전                  |
| OPENED           | 1   | open 메서드 호출 이후                  |
| HEADERS_RECEIVED | 2   | send 메서드 호출 이후                  |
| LOADING          | 3   | 서버 응답 중 (응답 데이터 미완성 상태) |
| DONE             | 4   | 서버 응답 완료                         |

### HTTP 요청 전송

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

**XMLHttpRequest.prototype.open**

> xhr.open(method, url[, async])

| 매개변수 | 설명                                                             |
| -------- | ---------------------------------------------------------------- |
| method   | HTTP 요청 메서드 ("GET", "POST", "PUT", "DELETE" 등)             |
| url      | HTTP 요청을 전송할 URL                                           |
| async    | 비동기 요청 여부로 옵션. 기본값은 true 이며 비동기 방식으로 동작 |

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

**XMLHttpRequest.prototype.send**

- open 메서드로 초기화된 HTTP 요청을 서버로 전송
  - GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송
  - POST 요청 메서드의 경우 데이터를 요청 몸체에 담아 전송

```jsx
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

**XMLHttpRequest.prototype.setRequestHeader**

- 특정 HTTP 요청의 헤더 값을 설정

| MIME 타입   | 서브타입                                           |
| ----------- | -------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
| multipart   | multipart/formed-data                              |

```js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('POST', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

- HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있음

```js
// 서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('accept', 'application/json');
```

### HTTP 응답 처리

> 💡 서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 함

- HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP 응답을 처리 가능

```js
const xhr = XMLHttpRequest();

// Fake REST API 를 제공하는 서비스
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는
// readyState 프로퍼티가 변경될 때마다 발생
xhr.onreadystatechange = () => {
	// readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타냄
	// readyState 프로퍼티 값이 4 (XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않은 상태

	// 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리 X
	if (xhr.readyState !== XMLHttpRequest.DONE) return;

	// status 프로퍼티는 응답 상태 코드를 나타냄
	// 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있음
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};
```

- readystatechange 이벤트 대신 load 이벤트를 캐치해도 됨
- load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생

```js
const xhr = XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생
xhr.onload = () => {
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};
```

### 참고

- https://poiemaweb.com/js-ajax
