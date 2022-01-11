# 16. 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

> 💡 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다

- ECMAScript 사양에 등장하는 이중 대괄호(\[[...]])로 감싼 이름들이 내부 슬롯과 내부 메서드이다.

![image](https://user-images.githubusercontent.com/55246584/148978680-0cb2635a-7d44-42d9-8ca6-b61504629562.png)

- 내부 슬롯과 내부 메서드는 자바스크립트 엔진에서 실제로 동작
- 개발자가 직접 접근할 수는 없음
- 일부 내부 슬롯과 내부 메서드에 한하여 간접적 접근 수단 제공

```js
const obj = {};

// 내부 슬롯은 접근할 수 없음
obj.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['
// 일부 내부 슬롯과 내부 메서드에 한해 접근할 수 있는 수단 제공
obj.__proto__ // Object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

> 💡 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 어트리뷰트를 기본값으로 자동 정의

- `프로퍼티 어트리뷰트` : 자바스크립트 엔진이 관리하는 내부 상태 값
  - \[[Value]], \[[Writable]], \[[Enumerable]], \[[Configurable]]

```js
const person = { name: 'Kim' };

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환
Object.getOwnPropertyDescriptor(person, 'name');
// { value: "Kim", writable: true, enumerable: true, configurable: true }
```

- ES8 에서 도입된 `Object.getOwnPropertyDescriptors` 메서드는 모든 프로퍼티의 프로퍼티 디스크립터 객체를 반환

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

## 16.4 프로퍼티 정의

## 16.5 객체 변경 방지
