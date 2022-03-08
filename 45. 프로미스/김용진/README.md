# 45. 프로미스

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 콜백 헬

```js
// GET 요청을 위한 비동기 함수
const get = (url) => {
	const xhr = new XMLHttpRequest();
	xhr.open('GET', url);
	xhr.send();

	xhr.onload = () => {
		if (xhr.status === 200) {
			// 서버의 응답을 콘솔에 출력
			console.log(JSON.parse(xhr.response));
		} else {
			console.error(`${xhr.status} ${xhr.statusText}`);
		}
	};
};

get('https://jsonplaceholder.typicode.com/posts/1');
```

- get 함수는 비동기 함수
- 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다해도 기다리지 않고 즉시 종료됨
- 즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료됨
- 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않음

👉 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야함

```js
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
	const xhr = new XMLHttpRequest();
	xhr.open('GET', url);
	xhr.send();

	xhr.onload = () => {
		if (xhr.status === 200) {
			// 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 함
			successCallback(JSON.parse(xhr.response));
		} else {
			// 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 함
			failureCallback(xhr.status);
		}
	};
};

get('https://jsonplaceholder.typicode.com/posts/1', console.log, console.error);
```

`콜백 헬`

```js
get('/step1', (a) => {
	get(`/step2/${a}`, (b) => {
		get(`/step2/${b}`, (c) => {
			get(`/step2/${c}`, (d) => {
				console.log(d);
			});
		});
	});
});
```

### 에러 처리의 한계

```js
try {
	setTimeout(() => {
		throw new Error('Error!');
	}, 1000);
} catch (e) {
	// 에러를 캐치하지 못함
	console.error('캐치한 에러', e);
}
```

- 비동기 함수인 setTimeout이 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸시되어 실행됨
- setTimeout은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거됨
- 이후 타이머가 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행됨
- setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태
- 에러는 호출자 방향으로 전파됨
  - 콜 스택의 아래 방향으로 전파
- 따라서 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않음

👉 콜백 헬이나 에러 처리를 극복하기 위해 ES6에서 프로미스가 도입

## 45.2 프로미스의 생성

> 💡 Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스(Promise 객체)를 생성

- ES6에서 도입된 Promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체

```js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
	// Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행
	if(/* 비동기 처리 성공 */){
		resolve('result');
	} else { /* 비동기 처리 실패 */
		reject('failure reason');
	}
});
```

- Promise 생성자 함수가 인수로 전달받은 콜백 함수에서 비동기 처리를 수행
- 이때 비동기 처리가 성공하면 콜백 함수의 인수로 전달받은 resolve 함수를 호출하고, 비동기 처리가 실패하면 reject 함수를 호출

```js
// GET 요청을 위한 비동기 함ㅅ
const promiseGET = (url) => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest();
		xhr.open('GET', url);
		xhr.send();

		xhr.onload = () => {
			if (xhr.status === 200) {
				// 성공적으로 응답을 전달받으면 resolve 함수를 호출
				resolve(JSON.parse(xhr.response));
			} else {
				// 에러 처리를 위해 reject 함수를 호출
				reject(new Error(xhr.status));
			}
		};
	});
};

promiseGet('https://jsonplaceholder.typicode.com/posts/1');
```

프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 가짐

| 프로미스의 상태 정보 | 의미                                  | 상태 변경 조건                  |
| -------------------- | ------------------------------------- | ------------------------------- |
| pending              | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본상태 |
| fulfilled            | 비동기 처리가 수행된 상태(성공)       | resolve 함수 호출               |
| rejected             | 비동기 처리가 수행된 상태(실패)       | reject 함수 호출                |

생성된 직후 프로미스는 기본적으로 pending 상태

- `비동기 처리 성공`: resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
- `비동기 처리 실패`: reject 함수를 호출해 프로미스를 rejected 상태로 변경

![image](https://user-images.githubusercontent.com/55246584/156882239-ddc6df34-903f-45ba-9293-895b8e02ade7.png)

- fulfilled 또는 rejected 상태를 settled 상태라고 함

👉 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체

## 45.3 프로미스의 후속 처리 메서드

> 💡 프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야함. 위를 위해 후속 메서드 then, catch, finally를 제공

### Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받음

- 첫 번째 콜백 함수는 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받음
- 두 번째 콜백 함수는 프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받음

```js
// filfilled
new Promise((resolve) => resolve('fulfilled')).then(
	(v) => console.log(v),
	(e) => console.log(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error('rejected'))).then(
	(v) => console.log(v),
	(e) => console.error(e)
); // Error: reject
```

👉 then 메서드는 언제나 프로미스를 반환

### Promise.prototype.catch

- catch 메서드는 한 개의 콜백 함수를 인수로 전달받음
- 프로미스가 rejected 상태인 경우만 호출됨

```js
// rejected
new Promise((_, reject) => reject(new Error('rejected'))).catch((e) => console.log(e)); // Error: rejected
```

- catch 메서드는 then 메서드와 동일하게 프로미스를 반환

### Promise.prototype.finally

- 한 개의 콜백 함수를 인수로 전달받음
- finally 메서드의 콜백 함수는 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출
- 프로미스를 반환

```js
new Promise(() => {}).finally(() => console.log('finally')); // finally
```

```js
new Promise(() => {}).finally(() => console.log('finally')); // finally
```

```js
const promiseGET = (url) => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest();
		xhr.open('GET', url);
		xhr.send();

		xhr.onload = () => {
			if (xhr.status === 200) {
				// 성공적으로 응답을 전달받으면 resolve 함수를 호출
				resolve(JSON.parse(xhr.response));
			} else {
				// 에러 처리르 위해 reject 함수를 호출
				reject(new Error(xhr.status));
			}
		};
	});
};

promiseGET('https://jsonplaceholde.typicode.com/posts/1')
	.then((res) => console.log(res))
	.catch((err) => console.error(err))
	.finally(() => console.log('Bye'));
```

## 45.4 프로미스의 에러 처리

```js
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

// 부적절한 URL이 지정되었기 때문에 에러가 발생
promiseGet(worngUrl).then(
	(res) => console.log(res),
	(err) => console.error(err)
); // Error: 404
```

- 비동기 처리에서 발생한 에러는 포로미스의 후속 처리 메서드 catch를 이용해 처리

```js
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

// 부적절한 URL이 지정되었기 때문에 에러가 발생
promiseGet(worngUrl)
	.then((res) => console.log(res))
	.catch((err) => console.error(err)); // Error: 404
```

## 45.5 프로미스 체이닝

```js
const url = 'https://jsonplaceholder.typicode.com';

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
	// 취득한 post의 userId로 user 정보를 취득
	.then(({ userId }) => promiseGet(`${url}/users/${userId}`))
	.then((userInfo) => console.log(userInfo))
	.catch((err) => console.error(err));
```

- then -> then -> catch 순서로 후속 처리 메서드를 호출
- 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있음
  👉 이를 `프로미스 체이닝`이라고 함

| 후속 처리 메서드 | 콜백 함수의 인수                                                                       | 후속 처리 메서드의 반환값                             |
| ---------------- | -------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| then             | promiseGet 함수가 반환한 프로미스가 resolve한 값(id가 1인 post)                        | 콜백 함수가 반환한 프로미스                           |
| then             | 첫 번째 then 메서드가 반환한 프로미스가 resolve한 값(post의 userId로 취득한 user 정보) | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |
| catch            | promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 reject한 값             | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |

👉 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않음

## 45.6 프로미스의 정적 메서드

**Promise.resolve / Promise.reject**

> 💡 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용

```js
// 배열을 resolve하는 프로미스를 생성
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```

- 위와 동일

```js
const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]
```

- Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성

```js
// 에러 객체를 reject하는 프로미스를 생성
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.log); // Error: Error!
```

- 위와 동일

```js
const rejectedPromise = new Promise((_, reject) => reject(new Error('Error')));
rejectedPromise.catch(console.log); // Error: Error!
```

**Promise.all**```

> 💡 여러 개의 비동기 처리를 모두 병렬처리할 때 사용

```js
const requestData1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 순차적으로 처리
const res = [];
requestData1()
	.then((data) => {
		res.push(data);
		return requestData2();
	})
	.then((data) => {
		res.push(data);
		return requestData3();
	})
	.then((data) => {
		res.push(data);
		console.log(res); // [1, 2, 3] => 약 6초 소요
	})
	.catch(console.error);
```

- 위 예제는 세 개의 비동기 처리를 순차적으로 처리
- 총 6초 소요
- 하지만 세 개의 비동기 처리는 서로 의존하지 않고 개별적으로 수행됨
- 순차적으로 처리할 필요가 없음

```js
const requestData1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 세 개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
	.then(console.log) // [1, 2, 3] => 약 3초 소요
	.catch(console.error);
```

- 위 예제의 경우 Promise.all 메서드는 3개의 프로미스를 요소로 갖는 배열을 전달받음
  - 첫 번째 프로미스는 3초 후에 1을 resolve
  - 두 번째 프로미스는 2초 후에 2를 resolve
  - 세 번째 프로미스는 1초 후에 3을 resolve

👉 모든 프로미스가 fulfilled 상태가 되면 resolve된 처리 결과를 모두 배열에 저장해 새로운 프로미스를 반환

- Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료

```js
Promise.all([
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 1'), 3000))),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 2'), 2000))),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 3'), 1000))),
])
	.then(console.log)
	.catch(console.log); // Error: Error 3
```

**Promise.race**

> 💡 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환

```js
Promise.race([
	new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
	new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
	new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
])
	.then(console.log) // 3
	.catch(console.log);
```

프로미스가 reject 상태가 되면 Promise.all 메서드와 동일하게 처리

```js
Promise.race([
	new Promise((resolve) => setTimeout(() => reject(new Error('Error 1')), 3000)),
	new Promise((resolve) => setTimeout(() => reject(new Error('Error 2', 2000)),
	new Promise((resolve) => setTimeout(() => reject(new Error('Error 3', 1000)),
])
	.then(console.log)
	.catch(console.log); // Error: Error 3
```

**Promise.allSettled**

> 💡 전달받은 프로미스가 모두 settled 상태(fulfilled 또는 rejected 상태)가 되면 처리 결과를 배열로 반환

```js
Promise.allSettled([
	new Promise((resolve) => setTimeout(() => resolve(1), 2000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000)),
]).then(console.log);
/*
  {status: "fulfilled", value: 1}
  {status: "rejected", reason: Error: Error! at <anonymous>:3:54}
 */
```

- 프로미스가 fulfilled 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 가짐
- 프로미스가 rejected 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 가짐

```js
[
	// 프로미스가 fulfilled 상태인 경우
	{ status: 'fulfilled', value: 1 },
	// 프로미스가 rejected 상태인 경우
	{ status: 'rejected', reason: Error: Error! at <anonymous>:3:60 },
];
```

## 45.7 마이크로태스크 큐

```js
setTimeout(() => console.log(1), 0);

Promise.resolve()
	.then(() => console.log(2))
	.then(() => console.log(3));

// 2 3 1 순으로 출력됨
```

> 💡 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되기 때문

`마이크로태스크 큐`

- 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장
- 태스크 큐보다 우선순위가 높음

👉 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행

## 45.8 fetch

> 💡 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API

```js
const promise = fetch(url, [, options]);
```

- HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환

```js
fetch('http://jsonplaceholder.typicode.com/todos/1')
	// response는 HTTP 응답을 나타내는 Response 객체
	// json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화
	.then((response) => response.json())
	// json은 역직렬화된 HTTP 응답 몸체
	.then((json) => console.log(json));
```

- 후속 처리 메서드 then을 통해 프로미스가 resolve한 Response 객체를 전달받을 수 있음
- Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공

**fetch 함수 에러 처리**

- 404 Not Found나 500 Internal Server Error와 같은 HTTP 에러가 발생해도 에러를 reject하지 않고 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 `resolve` 함
- 오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 `reject

```js
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

// 부적적한 URL이 지정되었기 떄문에 404 Not Found 에러가 발생
fetch(wrongUrl)
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todo) => console.log(todo))
	.catch((err) => console.error(err));
```

### axios

- 모든 HTTP 에러를 reject하는 프로미스를 반환
- 모든 에러를 catch에서 처리 가능

```js
const request = {
	get(url) {
		return fetch(url);
	},
	post(url, payload) {
		return fetch(url, {
			method: 'POST',
			headers: { 'content-Type': 'application/json' },
			body: JSON.stringify(payload),
		});
	},
	patch(url, payload) {
		return fetch(url, {
			method: 'PATCH',
			headers: { 'content-Type': 'application/json' },
			body: JSON.stringify(payload),
		});
	},
	delete(url) {
		return fetch(url, { method: 'DELETE' });
	},
};
```

**GET 요청**

```js
request
	.get('https://jsonplaceholder.typicode.com/todos/1')
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

**POST 요청**

```js
request
	.post('https://jsonplaceholder.typicode.com/todos', {
		userId: 1,
		title: 'JavaScript',
		completed: false,
	})
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

**PATCH 요청**

```js
request
	.patch('https://jsonplaceholder.typicode.com/todos/1', {
		completed: true,
	})
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

**DELETE 요청**

```js
request
	.delete('https://jsonplaceholder.typicode.com/todos/1')
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

### 참고

- https://velog.io/@hey880/%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4Promise
