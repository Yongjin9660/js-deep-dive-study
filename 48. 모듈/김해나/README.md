# 48. 모듈

## 48.1 모듈의 일반적 의미

> 💡 모듈은 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 의미

<br>

- **일반적으로 모듈은 기능을 기준으로 파일 단위로 분리**
  - 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있음.
  - 재사용성이 높아서 개발 효율성과 유지보수성 ↑
    <br>
- 이때 모듈이 성립하려면 **모듈은 자신만의 파일 스코프 (모듈 스코프) 를 가질 수 있어야 함.**
  - `자신만의 파일 스코프를 가지는 모듈의 자산은기본적으로 비공개 상태 (캡슐화 -> 다른 모듈에서 접근 x)`
  - 즉, 모듈은 개별적 존재로서 애플리케이션과 분리되어 존재함.
    <Br>
- 하지만 모듈은 애플리케이션이나 다른 모듈에 의해 재사용되어야 의미가 있음.

  - **공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능 => `export`**
    <br>

- 공개된 모듈의 자산은 다른 모듈에서 재사용 O
  - 이때 공개된 모듈의 자산을 사용하는 모듈을 `모듈 사용자` 라고 부름.
    <br>
- **모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택하여 자신의 스코프 내로 불러들여 재사용 가능 => `import`**

<br>

![img](https://media.vlpt.us/images/junh0328/post/5a6d12da-26da-4964-8102-b9c6376384bf/42_1.png)

<Br>

## 48.2 자바스크립트와 모듈

- **자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 import, export 지원 X**
  - 모든 자바스크립트 파일은 하나의 전역을 공유
  - 여러 개의 파일로 분리하여 script 태그로 로드해도 결국 하나의 자바스크립트 파일 내에 있는 것처럼 동작

<br>

- **자바스크립트에서 모듈 시스템을 해결하기 위해 제안된 것이 `CommonJS` 와 `AMD`**

  - 브라우저 환경에서 모듈을 사용하기 위해서는 CommonJS 또는 AMD 를 구현한 모듈 로더 라이브러리를 사용
    <br>

- **Node.js 는 ECMAScript 표준 사양은 아니지만 모듈 시스템을 지원**
  - CommonJS 사양을 따르고 있으며, 파일별로 독립적인 파일 스코프 (모듈 스코프) 를 가짐.

<Br>

## 48.3 ES6 모듈 (ESM)

- ES6 에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가

<br>

##### \*\* ES6 모듈 (ESM) 의 사용법

```jsx
<script type='module' src='app.mjs'></script>
```

- script 태그에 `type="module"` 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작
- 일반적인 자바스크립트 파일이 아닌 ESM 임을 명확히 하기 위해 ESM 의 파일 확장자는 mjs 사용을 권장
- ESM 에는 클래스와 마찬가지로 기본적으로 strict mode 가 적용됨.

<br>

#### 1) 모듈 스코프

- ESM 이 아닌 일반적인 자바스크립트 파일은 script 태그로 분리해서 로드해도 하나의 자바스크립트 파일 내에 있는 것처럼 동작
  - 분리된 자바스크립트 파일들의 전역 변수가 중복되는 문제 ↑
    <br>

```js
// foo.mjs
// x 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아님
var x = "foo";

console.log(x); // foo
console.log(window.x); // undefined
```

```js
// bar.mjs
// x 변수는 전연 변수가 아니며 window 객체의 프로퍼티도 아님
// foo.mjs 에서 선언한 x 변수와는 스코프가 다른 변수
var x = "bar";

console.log(x); // bar
console.log(window.x); // undefined
```

```html
<!DOCTYPE html>
<html>
	<body>
		<script type="module" src="foo.js"></script>
		<script type="module" src="bar.js"></script>
	</body>
</html>
```

- ESM 은 파일 자체의 독자적인 모듈 스코프를 제공
- 따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아님.

<br>

```js
// foo.mjs
const x = "foo";
console.log(x); // foo
```

```js
// bar.mjs
console.log(x); // ReferenceError
```

```html
<!DOCTYPE html>
<html>
	<body>
		<script type="module" src="foo.js"></script>
		<script type="module" src="bar.js"></script>
	</body>
</html>
```

- 모듈 내에서 선언한 식별자는 모듈 외부에서 참조 X
  - 모듈 스코프가 다르기 때문

<br>

#### 2) export 키워드

- 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 참조 가능
- **모듈 내부에서 선언한 식벽자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 `export` 키워드를 사용해야 함.**

<br>

```js
// lib.mjs
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
	return x * x;
}

// 클래스의 공개
export class Person {
	constructor(name) {
		this.name = name;
	}
}
```

<br>

```jsx
const pi = Math.PI;

function square(x) {
	return x * x;
}

class Person {
	constructor(name) {
		this.name = name;
	}
}
// 변수, 함수, 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

- export 할 대상을 하나의 객체로 구성하여 한 번에 export 하는 것도 가능

<br>

#### 3) impprt 키워드

- **다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 `import` 키워드 사용**
- 다른 모듈이 export 한 식별자 이름으로 import 해야 하며, ESM 의 경우 파일 확장자 생략 X

<br>

```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import
// ESM의 경우 파일 확장자 생략 X
import { pi, square, Person } from "./lib.mjs";

console.log(pi); // 3.14592653589793
console.log(square(10)); // 100
console.log(new Person("Lee")); // Person { name: 'Lee' }
```

```html
<!DOCTYPE html>
<html>
	<body>
		<script type="module" src="app.js"></script>
	</body>
</html>
```

<Br>

```js
// app.mjs
// lib.mjs 모듈이 export 한 모든 식별자를 lib 객체의 프로퍼티로 모아 import
import * as lib from "./lib.mjs";

console.log(pi); // 3.14592653589793
console.log(square(10)); // 100
console.log(new Person("Lee")); // Person { name: 'Lee' }
```

- **모듈이 export 한 식별자 이름을 일일이 지정하지 않고 하나의 이름으로 한 번에 import 하는 것도 가능**
  - 이때 import 되는 식별자는 as 뒤에 지정한 이름의 객체에 프로퍼티로 할당됨.
    <br>

```js
// lib.mjs
export default (x) => x * x;
```

- 모듈에서 하나의 값만 export 한다면 default 키워드 사용 가능
  - default 키워드를 사용하는 경우 기본적으로 이름 없이 하나의 값을 export

<br>

```js
// lib.mjs
export default const foo = () => {};
// SyntaxError: Unexpoected token 'const'
// export default () => {};
```

- default 키워드를 사용하는 경우 var, let, const 키워드 사용 X
  <br>

```js
// app.mjs
import square from "./lib.mjs";

console.log(square(3)); // 9
```

- default 키워드와 함께 export 한 모듈은 {} 없이 임의의 이름으로 import

<br><br>
\*\* 이미지 출처 : https://velog.io/@junh0328/DEEP-DIVE-%ED%95%9C-%EC%9E%A5-%EC%9A%94%EC%95%BD-%EB%AA%A8%EB%93%88
