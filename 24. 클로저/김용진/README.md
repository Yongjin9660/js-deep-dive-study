# 24. 클로저

## 24.1 렉시컬 스코프

> 💡 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

> 💡 함수는 자신의 내부 슬롯 [[Environment]] 에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

## 24.3 클로저와 렉시컬 환경

> 💡 외부 함수보다 중첩 함수가 더 오래 유지되는 경우
> 중첩 함수는 이미 생명 주기가 종료된 외부 함수의 변수를 참조할 수 있음
> 이러한 중첩 함수를 **클로저**라고 부름

```jsx
const x = 1;

function outer() {
	const x = 10;
	const inner = function () {
		console.log(x);
	};
	return inner;
}

const fun = outer();
fun(); // 10
```

## 24.4 클로저의 활용

> 💡 상태를 안전하게 변경하고 유지하기 위해 사용

```jsx
const counter = (function () {
	let num = 0;
	return {
		plus() {
			return ++num;
		},
		minus() {
			return --num;
		},
	};
})();

console.log(counter.plus()); // 1;
console.log(counter.plus()); // 2;
console.log(counter.minus()); // 1;
```

## 24.5 캡슐화와 정보 은닉

> 💡 객체의 특정 프로퍼티나 메서드를 감추기 위해 사용
