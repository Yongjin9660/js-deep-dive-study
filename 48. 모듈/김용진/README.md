# 48. 모듈

## 48.1 모듈의 일반적 의미

> 💡 모듈이란 애플리케이션을 구헝하는 개별적 요소로서 재사용 가능한 코드 조각을 말함

- 일반적으로 모듈은 기능을 기준으로 파일 단위로 분리
  👉 모듈이 성립하려면 모듈은 자신만의 파일 스코프를 가져야 함

모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능 👉 export
모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 저신의 스코프 내로 불러들여 재사용할 수 있음 👉 import

> 👉 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있고, 재소용성이 좋아 개발 효율성과 유지보수성을 높일 수 있음

## 48.2 자바스크립트와 모듈

`클라이언트 사이드 자바스크립트`

- script 태그를 사용하여 외부의 자바스크립트 파일을 로드
- 파일마다 독립적인 파일 스코프를 가지지 않음

> 💡 CommonJS, AMD가 제안됨

## 48.3 ES6 모듈(ESM)

> 💡 ES6에서는 클라이언트 사이드 자바스크립트에서도 동작하는 모듈 기능을 추가

- script 태그에 `type="modele"` 어트리뷰트를 추가

```js
<script type='module' src='app.mjs'></script>
```

### 모듈 스코프

> 💡 ESM은 독자적은 모듈 스코프를 가짐

```js
// foo.js
// x 변수는 전역 변수
var x = 'foo';
console.log(window.x); // foo
```

```js
// foo.js
// x 변수는 전역 변수이다. foo.js에서 선언한 전역 변수 x 와 중복된 선언
var x = 'bar';
console.log(window.x); // bar
```

```html
<!DOCTYPE html>
<html>
	<body>
		<script src="foo.js"></script>
		<script src="bar.js"></script>
	</body>
</html>
```

- script 태그로 분리해서 로드된 2개의 자바스크립트 파일은 하나의 자바스크립트 파일 내에 있는 것처럼 동작
- 하나의 전역을 공유
- 따라서 foo.js 에서 선언한 x 변수와 bar.js에서 선언한 x 변수는 중복 선언되며 의도치 않게 x 변수의 값에 덮어써짐

```js
// foo.mjs
// x 변수는 전역 변수가 아니며 window 객체의 프로퍼티도 아님
var x = 'foo';
console.log(x); // foo
console.log(window.x); // undefined
```

```js
// bar.mjs
// x 변수는 전연 변수가 아니며 window 객체의 프로퍼티도 아님
// foo.mjs 에서 선언한 x 변수와 스코프가 다른 변수
var x = 'bar';
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

- 모듈 내에서 선언한 식별자는 모듈 외부에서 참조할 수 없음
  - 모듈 스코프가 다름

```js
// foo.mjs
const x = 'foo';
console.log(x); // foo
```

```js
// bar.mjs
console.log(x); // ReferenceError: x is not defined
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

### export 스코프

> 💡 모듈 내부에서 선언한 식벽자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드를 사용

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

- export할 대상을 하나의 객체로 구성하여 한 번에 export할 수도 있음

```js
// lib.mjs
// 변수의 공개
const pi = Math.PI;

// 함수의 공개
function square(x) {
	return x * x;
}

// 클래스의 공개
class Person {
	constructor(name) {
		this.name = name;
	}
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

### import 스코프

> 💡 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용

```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import
// ESM의 경우 파일 확장자를 생략할 수 없음
import { pi, square, Person } from './lib.mjs';

console.log(pi); // 3.14592...
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

```html
<!DOCTYPE html>
<html>
	<body>
		<script type="module" src="app.js"></script>
	</body>
</html>
```

- app.mjs는 애플리케이션의 진입점이므로 반드시 script 태그로 로드를 해야함
- 하지만 lib.mjs는 app.mjs의 import 문에 의해 로드되는 의존성임
  👉 lib.mjs는 script 태그로 로드하지 않아도 됨

<br />

- 한번에 import

```js
// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import 함
import * as lib from './lib.mjs';

console.log(pi); // 3.14592...
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

- 모듈이 export한 식별자 이름을 변경하여 import

```js
// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import
import { pi as PI, square as sq, Person as P } from './lib.mjs';

console.log(PI); // 3.14592...
console.log(sq(10)); // 100
console.log(new P('Lee')); // Person { name: 'Lee' }
```

- 모듈에서 하나의 값만 export한다면 default 키워드를 사용 가능

```js
// lib.mjs
export default (x) => x * x;
```

- default 키워드를 사용하는 경우 var, let, const 키워드를 사용할 수 없음

```js
// lib.mjs
export default const foo = () => {};
// SyntaxError: Unexpoected token 'const'
```

- default 키워드와 함께 export한 모듈은 {} 없이 임의의 이름으로 import

```js
// app.mjs
import square from './lib.mjs';
console.log(square(3)); // 9
```
