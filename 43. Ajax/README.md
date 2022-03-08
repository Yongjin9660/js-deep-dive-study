# 43. Ajax

## 43.1 Ajax 란?

> 💡 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식

<br>

- Ajax 는 브라우저에서 제공하는 Web API 인 XMLHttpRequest 객체를 기반으로 동작
  - XMLHttpRequest 는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공

<br>

#### \*\* `Ajax 등장 이전`

<br>

<img style="height:450px;" src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png">

<br>

##### [ 전통적인 방식의 단점 ]

- 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 `완전한 HTML 을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.`
- 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 `화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.`
- 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 `서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.`

<br>

---

<br>

#### \*\* `Ajax 등장 이후`

- **서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 한정적으로 렌더링하는 방식을 가능하게 함.**
  - 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 퍼포먼스와 부드러운 화면 전환이 가능해짐.

<br>

<img style="height:450px;" src="https://poiemaweb.com/img/traditional-webpage-lifecycle.png">

<br>

##### [ Ajax 의 장점 ]

- `변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.`
- 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 `화면이 순간적으로 깜박이는 현상이 발생하지 않는다.`
- 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 `서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.`

<br>

## 43.2 JSON

> 💡 JSON 은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷을 의미
> → 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용 가능

<br>

#### 1) JSON 표기 방식

```jsx
{
  "name" : "Lee",
  "age" : 20,
  "alive" : true,
  "hobby" : ["traveling", "tennis"]
}
```

- JSON 은 자바스크립트의 객체 리터럴과 유사하게 **키와 값으로 구성된 순수한 텍스트**
  - `키는 반드시 큰따옴표 (작은 따옴표 X) 로 묶어야 함.`
  - `값은 객체 리터럴과 같은 표기법을 그대로 사용 가능하지만, 문자열은 반드시 큰따옴표 (작은 따옴표 X) 로 묶어야 함.`

<br>

#### 2) JSON.stringify

> 💡 JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환

<br>

- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데, 이를 **`직렬화`** 라고 부름.

<br>

```jsx
const obj = {
	name: "Lee",
	age: 20,
	alive: true,
	hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee", "age":20, "alive":true, "hobby":["traveling", "tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기
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

// 값의 타입이 Number 이면 필터링되어 반환되지 않는 함수 정의
function filter(key, value) {
	// undefined : 반환하지 않음
	return typeof value === "number" ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 filter 함수 전달
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

// 배열을 JSON 포맷의 문자열로 변환
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

- JSON.stringify 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환 가능
  <br>

#### 3) JSON.parse

> 💡 JSON.parse 메서드는 JSON 포맷의 문자열을 객체나 배열로 변환

<br>

- 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열
  - 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데, 이를 **`역직렬화`** 라고 부름.

<br>

```jsx
const obj = {
	name: "Lee",
	age: 20,
	alive: true,
	hobby: ["traveling", "tennis"],
};

// 객체를 JSON 포맷의 문자열로 변환
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {name:"Lee", age:20, alive:true, hobby:["traveling", "tennis"]}
```

```jsx
const todos = [
	{ id: 1, content: "HTML", completed: false },
	{ id: 2, content: "CSS", completed: true },
];

// 배열을 JSON 포맷의 문자열로 변환
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환
// 배열의 요소까지 객체로 변환
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);

/*
object [
  { id : 1, content : 'HTML', completed : false }, 
  { id : 2, content : 'CSS', completed : true }
]
*/
```

- JSON.parse 는 배열이 JSON 포맷의 문자열로 변환되어 있는 경우에도 문자열을 배열 객체로 변환
  - 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

<br>

## 43.3 XMLHttpRequest

- 브라우저는 주소창이나 HTML 의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공
- `자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용해야 함.`
  - Wep API 인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공

<br>

#### 1) XMLHttpRequest 객체 생성

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();
```

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성
  - XMLHttpRequest 객체는 브라우저에서 제공하는 Wep API 이므로 브라우저 환경에서만 정상적으로 실행됨.
    <br>

#### 2) XMLHttpRequest 객체의 프로퍼티와 메서드

##### 1) XMLHttpRequest 객체의 프로토타입 프로퍼티

| 프로토타입 프로퍼티 | 설명                                                                                                |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| readyState          | HTTP 요청의 현재 상태를 나타내는 정수. 다음과 같은 XMLHttpRequest 의 정적 프로퍼티를 값으로 갖는다. |
|                     | UNSENT : 0                                                                                          |
|                     | OPENED : 1                                                                                          |
|                     | HEADERS_RECEIVED : 2                                                                                |
|                     | LOADING : 3                                                                                         |
|                     | DONE : 4                                                                                            |
| status              | HTTP 요청에 대한 응답 상태 (HTTP 상태 코드) 를 나타내는 정수                                        |
|                     | ex) 200                                                                                             |
| statusText          | HTTP 요청에 대한 응답 메시지를 나타내는 문자열                                                      |
|                     | ex) "OK"                                                                                            |
| responseType        | HTTP 응답 타입                                                                                      |
|                     | ex) document, json, text, blob, arraybuffer                                                         |
| response            | HTTP 요청에 대한 응답 몸체, responseType 에 따라 타입이 다르다.                                     |
| responseText        | 서버가 전송한 HTTP 요청에 대한 응답 문자열                                                          |

<br>

##### 2) XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

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

<br>

##### 3) XMLHttpRequest 객체의 메서드

| 메서드           | 설명                                     |
| ---------------- | ---------------------------------------- |
| open             | HTTP 요청 초기화                         |
| send             | HTTP 요청 전송                           |
| abort            | 이미 전송된 HTTP 요청 중단               |
| setRequestHeader | 특정 HTTP 요청 헤더의 값을 설정          |
| getRequestHeader | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

<br>

##### 4) XMLHttpRequest 객체의 정적 프로퍼티

| 정적 프로퍼티    | 값  | 설명                                   |
| ---------------- | --- | -------------------------------------- |
| UNSENT           | 0   | open 메서드 호출 이전                  |
| OPENED           | 1   | open 메서드 호출 이후                  |
| HEADERS_RECEIVED | 2   | send 메서드 호출 이후                  |
| LOADING          | 3   | 서버 응답 중 (응답 데이터 미완성 상태) |
| DONE             | 4   | 서버 응답 완료                         |

<br>

#### 3) HTTP 요청 전송

[ HTTP 요청을 전송하는 순서]

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

1. XMLHttpRequest.prototype.open 메서드로 `HTTP 요청을 초기화`
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 `특정 HTTP 요청의 헤더 값 설정`
3. XMLHttpRequest.prototype.send 메서드로 `HTTP 요청 전송`

<br>

###### 1) `XMLHttpRequest.prototype.open`

- **open 메서드는 서버에 전송할 HTTP 요청을 초기화**

```jsx
xhr.open(method, url[, async])
```

<br>

| 매개변수 | 설명                                                             |
| -------- | ---------------------------------------------------------------- |
| method   | HTTP 요청 메서드 ("GET", "POST", "PUT", "DELETE" 등)             |
| url      | HTTP 요청을 전송할 URL                                           |
| async    | 비동기 요청 여부로 옵션. 기본값은 true 이며 비동기 방식으로 동작 |

<br>

**[ HTTP 요청 메서드 ]**

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적 (리소스에 대한 행위) 을 알리는 방법
- 주로 5가지 요청 메서드 (GET, POST, PUT, PATCH, DELETE) 를 사용하여 CRUD 구현
  <br>

##### 2) `XMLHttpRequest.prototype.send`

- **send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송**
- 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이 O
  - `GET` 요청 메서드의 경우 데이터를 URL 의 일부분인 쿼리 문자열로 서버에 전송
  - `POST` 요청 메서드의 경우 데이터 (페이로드) 를 요청 몸체에 담아 전송

<br>

```jsx
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- **send 메서드에는 요청 몸체에 담아 전송할 데이터 (페이로드) 를 인수로 전달 가능**
  - `페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달해야 함.`
    <br>
- HTTP 요청 메서드가 GET 인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null 로 설정됨.
  <br>

##### 3) `XMLHttpRequest.prototype.setRequestHeader`

- **setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 결정**
- 반드시 open 메서드를 호출한 이후에 호출해야 함.
- 자주 사용하는 HTTP 요청 메서드 : Content-type, Accept
  - `Content-type` : 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현
  - `Accept` : HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입 지정
    <br>

##### ※ 자주 사용되는 MIME 타입

| MIME 타입   | 서브타입                                           |
| ----------- | -------------------------------------------------- |
| text        | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
| multipart   | multipart/formed-data                              |

<br>

```jsx
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader("content-type", "application/json");

// 서버가 응답할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader("accept", "application/json");
```

<br>

#### 4) HTTP 응답 처리

- **서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 함.**

  - readyState 프로퍼티 값이 변경된 경우 발생하는 `readystatechange` 이벤트를 캐치하여 HTTP 응답 처리 가능
    <br>

- HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP 응답을 처리 가능

<br>

```jsx
//  XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com 은 Fake REST API 를 제공하는 서비스
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는
// readyState 프로퍼티가 변경될 때마다 발생
xhr.onreadystatechange = () => {
	// readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타냄.
	// readyState 프로퍼티 값이 4 (XMLHttpRequest.DONE) 가 아니면 서버 응답이 완료되지 않은 상태

	// 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리 X
	if (xhr.readyState !== XMLHttpRequest.DONE) return;

	// status 프로퍼티는 응답 상태 코드를 나타냄.
	// status 프로퍼티 값이 200 이면 정상적으로 응답한 상태이고
	// status 프로퍼티 값이 200 이 아니면 에러가 발생한 상태
	// 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있음.
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error("Error", xhr.status, xhr.statusText);
	}
};
```

- **send 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환**

  - 클라이언트에게 언제 응답이 도달할 지는 알 수 없으므로 readystatechange 이벤트를 통해 HTTP 요청의 현재 상태를 확인
    <br>

- **xhr.readyState 가 XMLHttpRequest.DONE 인지 확인하여 서버의 응답이 완료되었는지 확인**
  - 서버의 응답이 완료되면 xhr.status 가 200 인지 확인하여 정상 처리와 에러 처리를 구분
  - `정상 처리` : xhr.response 에 서버가 전송한 데이터를 취득
  - `에러 처리` : 에러가 발생한 상태이므로 필요한 에러 처리 수행

<br>

```jsx
//  XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com 은 Fake REST API 를 제공하는 서비스
xhr.open("GET", "https://jsonplaceholder.typicode.com/todos/1");

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생
xhr.onload = () => {
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId : 1, id : 1, title : "delectus aut autem", completed : false}
	} else {
		console.error("Error", xhr.status, xhr.statusText);
	}
};
```

- **readystatechance 이벤트 대신 load 이벤트를 통해 캐치하면 xhr.readyState 가 XMLHttpRequest.DONE 인지 확인할 필요 X**
  - load 이벤트는 HTTP 요청이 성공적으로 완료된 경우에만 발생하기 때문
