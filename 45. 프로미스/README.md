# 45. 프로미스

- 자바스크립트는 비동기 처리를 위한 하나의 패턴으로 **콜백 함수**를 사용

  - 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며, 여러 개의 비동기 처리를 한 번에 처리하는 데도 한계가 있음.
    <br>

- ES6 에서는 비동기 처리를 위한 또 다른 패턴으로 **프로미스** 를 도입
  - 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있게 됨.

<br>

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

#### 1) 콜백 헬

```js
// GET 요청을 위한 비동기 함수
const get = (url) => {
	const xhr = new XMLHttpRequest();
	xhr.open("GET", url);
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

get("https://jsonplaceholder.typicode.com/posts/1");
```

- `get 함수는 비동기 함수`

  - 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다해도 기다리지 않고 즉시 종료됨
  - 즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료됨
    <br>

- **따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않음.**

<br>

#### 👉 **비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 함**

<br>

```jsx
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
	const xhr = new XMLHttpRequest();
	xhr.open("GET", url);
	xhr.send();

	xhr.onload = () => {
		if (xhr.status === 200) {
			// 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 해야 한다.
			successCallback(JSON.parse(xhr.response));
		} else {
			// 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 한다.
			failureCallback(xhr.status);
		}
	};
};

// id가 1인 post 를 취득
// 서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get 에 전달해야 한다.
get("https://jsonplaceholder.typicode.com/posts/1", console.log, console.error);

/*
{
  "userId": 1, 
  "id": 1,
  "title": "sunt aut facere ...", 
  "body": "quia et suscipit ..."
}
*/
```

- 콜백 함수를 통해 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생 => **`콜백 헬`**

<br>

##### ※ 콜백 헬

```js
get("/step1", (a) => {
	get(`/step2/${a}`, (b) => {
		get(`/step2/${b}`, (c) => {
			get(`/step2/${c}`, (d) => {
				console.log(d);
			});
		});
	});
});
```

<br>

#### 2) 에러 처리의 한계

```jsx
try {
	setTimeout(() => {
		throw new Error("Error!");
	}, 1000);
} catch (e) {
	// 에러를 캐치하지 못한다.
	console.error("캐치한 에러", e);
}
```

- try 코드 블록 내에서 호출한 setTimeout 함수는 1초 후에 콜백 함수가 실행되도록 타이머를 설정하고, 이후 콜백 함수는 에러를 발생시킴.
  - 하지만 이 에러는 catch 코드 블록에서 캐치되지 않음.
    <br>

**[catch 코드 블록 내에서 에러가 캐치되지 않은 이유]**

- 비동기 함수인 setTimeout 함수는 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거됨.
- 이후 타이머가 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행됨.
- setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태
  - setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니라는 의미
- **`에러는 호출자 방향으로 전파`**
  - 즉, 콜 스택의 아래 방향으로 전파되기 때문에 setTimeout 함수의 콜백 함수가 발생시킨 에러가 catch 블록에서 캐치되지 않는 것.
    <br>

#### 👉 **콜백 헬이나 에러 처리를 극복하기 위해 ES6에서 프로미스 도입**

<br>

## 45.2 프로미스의 생성

- 프로미스 (Promise) 객체 생성 방법
  - **Promise 생성자 함수를 new 연산자와 함께 호출**
  - ES6에서 도입된 Promise 는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체
  - `Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달`받는데, 이 콜백 함수는 **`resolve`** 와 **`reject`** 함수를 인수로 전달받음.

<br>

```jsx
// 프로미스 생성
const promise = new Promise((resolve, reject) => {

  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행
  if (/* 비동기 처리 성공 */) {
    resolve('result');
  } else { /* 비동기 처리 실패 */) {
    reject('failure reason');
  }
})
```

- Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리 수행
  - 비동기 처리 성공 => resolve 함수 호출
  - 비동기 처리 실패 => reject 함수 호출

<br>

```jsx
// GET 요청을 위한 비동기 함수
const promiseGet = (url) => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest();
		xhr.open("GET", url);
		xhr.send();

		xhr.onload = () => {
			if (xhr.status === 200) {
				// 성공적으로 응답을 전달받으면 resolve 함수 호출
				resolve(JSON.parse(xhr.response));
			} else {
				// 에러 처리를 위해 reject 함수 호출
				reject(new Error(xhr.status));
			}
		};
	});
};

// promiseGet 함수는 프로미스를 반환
promiseGet("https://jsonplaceholder.typicode.com/posts/1");
```

- 비동기 처리 함수인 promiseGet 은 함수 내부에서 프로미스를 생성하고 반환
  - `비동기 처리는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 수행`

<br>

- 프로미스는 다음과 같이 **현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보**를 가짐.

| 프로미스의 상태 정보 | 의미                                  | 상태 변경 조건                  |
| -------------------- | ------------------------------------- | ------------------------------- |
| pending              | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본상태 |
| fulfilled            | 비동기 처리가 수행된 상태 (성공)      | resolve 함수 호출               |
| rejected             | 비동기 처리가 수행된 상태 (실패)      | reject 함수 호출                |

<br>

![img](https://uzilog-upload.s3.ap-northeast-2.amazonaws.com/private/ap-northeast-2%3Ab6c10628-1f45-492c-a9eb-f54020bc8014/1582255268755-image.png)

<br>

- 생성된 직후의 프로미스는 기본적으로 pending 상태

  - `비동기 처리 성공` : resolve 함수를 호출하여 프로미스를 **fulfilled** 상태로 변경
  - `비동기 처리 실패` : reject 함수를 호출하여 프로미스를 **rejected** 상태로 변경
    <br>

- **settled 상태** : pending 이 아닌 상태로 비동기 처리가 수행된 상태
  - 일단 settled 상태가 되면 더 이상 다른 상태로 변화할 수 없음.

<br>

#### 1) 비동기 처리 성공 시

![image](https://user-images.githubusercontent.com/77706631/156986639-5699cc16-9aef-4099-87ae-c5ff6074473d.png)

- **비동기 처리가 성공**하면 프로미스는 pending 상태에서 **fulfilled 상태로 변화**
  - `비동기 처리 결과인 1을 값으로 가짐.`

<br>

#### 2) 비동기 처리 실패 시

![image](https://user-images.githubusercontent.com/77706631/156986793-707a3270-d2c2-4107-acc8-be95f02b6769.png)

- **비동기 처리가 실패**하면 프로미스는 pending 상태에서 **rejected 상태로 변화**
  - `비동기 처리 결과인 Error 객체를 값으로 가짐.`

<br>

#### 👉 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체

<Br>

## 45.3 프로미스의 후속 처리 메서드

- 프로미스는 비동기 처리 상태가 변화하면 이에 따른 **후속 처리** 를 해야 함.
  - 이를 위해 프로미스는 후속 메서드 `then`, `catch`, `finally` 제공
    <br>
- **프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출됨.**
  - 이때 후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달됨.
  - `모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작함.`

<br>

#### 1) Promise.prototype.then

> 💡 then 메서드는 2개의 콜백 함수를 인수로 전달받음.

- **성공 처리 콜백 함수**
  - 프로미스가 fulfilled 상태 (resolve 함수가 호출된 상태) 가 되면 호출된다.
  - 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
    <br>
- **실패 처리 콜백 함수**
  - 프로미스가 rejected 상태 (reject 함수가 호출된 상태) 가 되면 호출된다.
  - 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.

<br>

```jsx
// fulfilled
new Promise((resolve) => resolve("fulfilled")).then(
	(v) => console.log(v),
	(e) => console.error(e)
); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error("rejected"))).then(
	(v) => console.log(v),
	(e) => console.error(e)
); // Error : rejected
```

- **then 메서드는 언제나 프로미스를 반환**
  - then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환
  - then 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성하여 반환
    <br>

#### 2) Promise.prototype.catch

> 💡 catch 메서드는 1개의 콜백 함수를 인수로 전달받음.

- **catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우에만 호출됨.**
  - then 메서드와 동일하게 동작하며 catch 메서드 또한 언제나 프로미스를 반환

<br>

```jsx
// rejected
new Promise((_, reject) => reject(new Error("rejected"))).catch((e) => console.log(e)); // Error : rejected
```

<br>

#### 3) Promise.prototype.finally

> 💡 finally 메서드는 1개의 콜백 함수를 인수로 전달받음.

- **finally 메서드의 콜백 함수는 프로미스의 성공 여부와 상관없이 무조건 한 번 호출됨.**
  - 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용
  - then/catch 메서드와 동일하게 동작하며 finally 메서드 또한 언제나 프로미스를 반환

<br>

```jsx
new Promise(() => {}).finally(() => console.log("finally")); // finally
```

<br>

##### \*\* 프로미스로 구현한 비동기 함수 get 을 사용하여 후속 처리 구현

```jsx
const promiseGet = (url) => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest();
		xhr.open("GET", url);
		xhr.send();

		xhr.onload = () => {
			if (xhr.status === 200) {
				// 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
				resolve(JSON.parse(xhr.response));
			} else {
				// 에러 처리를 위해 reject 함수를 호출한다.
				reject(new Error(xhr.status));
			}
		};
	});
};

// promiseGet 함수는 프로미스를 반환한다.
promiseGet("https://jsonplaceholder.typicode.com/posts/1")
	.then((res) => console.log(res))
	.catch((err) => conosle.error(err))
	.finally(() => console.log("Bye!"));
```

<br>

## 45.4 프로미스의 에러 처리

```jsx
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL 이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl).then(
	(res) => console.log(res),
	(err) => console.log(err)
); // Error : 404
```

```jsx
promiseGet("https://jsonplaceholder.typicode.com/XXX/1").then(
	(res) => console.xxx(res),
	(err) => console.error(err)
); // 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못한다.
```

- **비동기 처리에서 발생한 에러는 then 메서드의 2번째 콜백 함수로 처리 가능**
  - 단, 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해져서 가독성이 좋지 않음.
    <br>

```jsx
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL 이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl)
	.then((res) => console.log(res))
	.catch((err) => console.error(err)); // Error : 404
```

```jsx
promiseGet("https://jsonplaceholder.typicode.com/XXX/1")
	.then((res) => console.xxx(res))
	.catch((err) => console.error(err)); // TypeError : console.xxx is not a function
```

- **비동기 처리에서 발생한 에러는 프로미스의 후속 처리 메서드 catch 를 사용해서도 처리 가능**

  - catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치 가능
  - 따라서 에러 처리는 then 메서드보다는 catch 메서드에서 하는 것을 권장

## 45.5 프로미스 체이닝

- 비동기 처리를 위한 콜백 패턴은 콜백 헬이 발생하는 문제가 존재함.
  - `프로미스는 then, catch, finally 후속 처리 메서드를 통해 콜백 헬을 해결`
    <br>

```jsx
const url = "https://jsonplaceholder.typicode.com/XXX/1";

// id 가 1인 post 의 userId 를 취득
promiseGet(`${url}/posts/1`)
	.then(({ userId }) => promiseGet(`${url}/users/${userId}`))
	.then((userInfo) => console.log(userInfo))
	.catch((err) => console.error(err));
```

- 위 예제에서 then → then → catch 순서로 후속 처리 메서드를 호출 => `프로미스 체이닝`
  - 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적인 호출이 가능하기 때문

<br>

| 후속 처리 메서드 | 콜백 함수의 인수                                                                        | 후속 처리 메서드의 반환값                             |
| ---------------- | --------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| then             | promiseGet 함수가 반환한 프로미스가 resolve 한 값 (id가 1인 post)                       | 콜백 함수가 반환한 프로미스                           |
| then             | 첫 번째 then 메서드가 반환한 프로미스가 resolve한 값 (post의 userId로 취득한 user 정보) | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |
| catch            | promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 reject한 값              | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |

- 후속 처리 메서드는 콜백 함수가 반환한 프로미스를 반환
- 만약 후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성하여 반환
- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 콜백 헬 X

<br>

## 45.6 프로미스의 정적 메서드

- Promise 는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있음.
  - Promise 는 5가지 메서드를 제공
    <Br>

#### 1) Promise.resolve / Promise.reject

> 💡 Promise.resolve 와 Promise.reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용

<br>

```jsx
// 배열을 resolve 하는 프로미스 생성
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]
```

- Promise.resolve 메서드는 인수로 전달받은 값을 resolve 하는 프로미스를 생성
  <br>

```jsx
// 에러 객체를 reject 하는 프로미스 생성
const rejectedPromise = Promise.reject(new Error("Error!"));
rejectddPromise.catch(console.log); // Error : Error!
```

- Promise.reject 메서드는 인수로 전달받은 값을 reject 하는 프로미스를 생성
  <br>

#### 2) Promise.all

> 💡 Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용

<br>

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

- 위 예제는 세 개의 비동기 처리를 순차적으로 처리 => `총 6초 소요`
- 하지만 세 개의 비동기 처리는 서로 의존하지 않고 개별적으로 수행됨
  - 즉, 순차적으로 처리할 필요 X

<br>

```jsx
const requestData1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const requestData3 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));

// 3개의 비동기 처리를 병렬로 처리
Promise.all([requestData1(), requestData2(), requestData3()])
	.the(console.log) // [ 1, 2, 3 ] -> 약 3초 소요
	.catch(console.error);
```

- 위 예제의 경우 Promise.all 메서드는 3개의 프로미스를 요소로 갖는 배열을 전달받음

  - 첫 번째 프로미스는 3초 후에 1을 resolve
  - 두 번째 프로미스는 2초 후에 2를 resolve
  - 세 번째 프로미스는 1초 후에 3을 resolve
    <br>

- 모든 처리에 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 첫 번째 프로미스의 처리 시간인 3초보다 조금 더 소요됨.
  <br>

- **Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음.**

- **전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장하여 새로운 프로미스 반환**
  - 처리 순서 보장 (첫 번째 프로미스가 resolve 한 처리 결과부터 차례대로 배열에 저장

<br>

```js
Promise.all([
	new Promise((_, reject) => setTimeout(() => reject(new Error("Error 1"), 3000))),
	new Promise((_, reject) => setTimeout(() => reject(new Error("Error 2"), 2000))),
	new Promise((_, reject) => setTimeout(() => reject(new Error("Error 3"), 1000))),
])
	.then(console.log)
	.catch(console.log); // Error: Error 3
```

- 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료
  <br>

#### 3) Promise.race

> 💡 Promise.race 메서드는 가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve 하는 새로운 프로미스를 반환

<br>

```jsx
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000)), // 3
)]
  .then(console.log) // 3
  .catch(console.log(;))
```

- **Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음.**

  - 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료
    <br>

#### 4) Promise.allSettled

> 💡 Promise.allSettled 메서드는 전달받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환

<br>

```jsx
Promise.allSettled([
	new Promise((resolve) => setTimeout(() => resolve(1), 2000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error("Error!")), 1000)),
]).then(console.log);

/*
[
  {status: "fulfiiled", value: 1}.
  {status: "rejected", reason: Error: Error! at <anonymous>:3:54}
]
*/
```

- **Promise.allSettled 메서드가 반환한 배열에는 fulfilled 또는 rejected 상태와는 상관없이 Promise.allSettled 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 모두 담겨 있음.**
  <br>
- Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받음.
  <br>
- 프로미스의 처리 결과를 나타내는 객체
  - 프로미스가 fulfilled 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 갖는다.
  - 프로미스가 rejected 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 갖는다.

<br>

## 45.7 마이크로태스크 큐

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
	.then(() => console.log(2))
	.then(() => console.log(3));
```

- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 → 2 → 3 순으로 출력될 것처럼 보이지만 2 → 3 → 1 순으로 출력됨.
  - 그 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 **마이크로태스크 큐**에 저장되기 때문
    <br>

#### \*\* `마이크로태스크 큐`

- **태스크 큐와는 별도의 큐로, 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장됨.**

  - 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장됨.
    <br>

- **마이크로태스크 큐는 태스크 큐보다 우선순위 ↑**
  - 이벤트 루프는 콜 스택이 비면 먼저 마이크로테스트 큐에서 대기하고 있는 함수를 가져와 실행
  - 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행
    <br>

## 45.8 fetch

> fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API

<br>

```jsx
const promise = fetch(url [, options])
```

- Http 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달

  - 첫 번째 인수로 HTTP 요청을 전송할 URL 만 전달하면 GET 요청을 전송
    <br>

![image](https://user-images.githubusercontent.com/77706631/157028693-4236c681-3626-4180-b0d2-3ca82ef4cf05.png)

- fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환
  - 후속처리 then 을 통해 프로미스가 resolve 한 Response 객체를 전달받을 수 있음.
  - Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티 제공
    <br>

```jsx
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL 이 지정되었기 때문에 404 Not Found 에러가 발생한다.
fetch(wrongUrl)
	.then(() => console.log("ok"))
	.catch(() => console.log("error"));

// 출력 결과 : 'ok'
// 부적절한 url 이 지정되었기 때문에 404 Not Found 에러가 발생하고 cath 후속 처리 메서드에 의해 'error' 가 출력되어야 하지만 'ok' 가 출력됨.
```

- fetch 함수가 반환하는 프로미스는 기본적으로 404 Not Found나 500 Internal Server Error 와 같은 HTTP 에러가 발생해도 에러를 reject 하지 않고 불리언 타입의 ok 상태를 false 로 설정한 Response 객체를 `resolve` 함.
  <Br>
- **오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject**

<br>

```jsx
constwrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL 이 지정되었기 때문에 404 Not Found 에러가 발생한다.
fetch(wrongUrl)
	// response 에는 HTTP 응답을 나타내는 Response 객체
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todo) => console.log(todo))
	.catch((err) => console.error(err));
```

- fetch 함수를 사용할 때는 fetch 함수가 반환한 프로미스가 resolve 한 불리언 타입의 ok 타입의 상태를 확인하여 명시적으로 에러를 처리해야 함.

<Br>

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
			method: "POST",
			headers: { "content-Type": "application/json" },
			body: JSON.stringify(payload),
		});
	},
	patch(url, payload) {
		return fetch(url, {
			method: "PATCH",
			headers: { "content-Type": "application/json" },
			body: JSON.stringify(payload),
		});
	},
	delete(url) {
		return fetch(url, { method: "DELETE" });
	},
};
```

**GET 요청**

```js
request
	.get("https://jsonplaceholder.typicode.com/todos/1")
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
	.post("https://jsonplaceholder.typicode.com/todos", {
		userId: 1,
		title: "JavaScript",
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
	.patch("https://jsonplaceholder.typicode.com/todos/1", {
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
	.delete("https://jsonplaceholder.typicode.com/todos/1")
	.then((response) => {
		if (!response.ok) throw new Error(response.statusText);
		return response.json();
	})
	.then((todos) => console.log(todos))
	.catch((err) => console.error(err));
```

<br> <br>
\*\* 이미지 출처
https://uzihoon.com/post/5e15d7e0-538f-11ea-95fe-0317987f9fc8
