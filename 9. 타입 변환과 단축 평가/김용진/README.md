## 9.1 타입 변환이란?

- **명시적 타입 변환 (타입 캐스팅)**

  - 개발자가 의도적으로 값의 타입을 변환하는것

  ```jsx
  var x = 10;

  // 명시적 타입 변환
  var str = x.toString();
  console.log(typeof str, str); // string 10
  ```

- **암묵적 타입 변환 (타입 강제 변환)**

  - 자바스크립트 엔진에 의해 암묵적으로 타입 자동 변환

  ```jsx
  var x = 10;

  // 암묵적 타입 변환
  var str = x + '';
  console.log(typeof str, str); // string 10
  ```

## 9.2 암묵적 타입 변환

- **문자열 타입으로 변환**

  ```jsx
  1 +
  	'2' // '12'
  	`1 + 1 - ${1 + 1}`; // '1 + 1 = 2'
  ```

  - 피연산자 중 하나 이상이 문자열 ⇒ 문자열 연결 연산자로 동작
  - 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 **암묵적 타입 변환**

- **숫자 타입으로 변환**

  ```js
  1 - '1'; // 0
  1 * '10'; // 10
  +'1'; // 1
  +'string'; // NaN
  ```

- **불리언 타입으로 변환**
  - 자바스크립트 엔진은 불리언 타입이 아닌 값을 **Truthy 값(참으로 평가되는 값) 또는 Falsy 값(거짓으로 평가되는 값)**으로 구분
    - Truthy 값 ⇒ true
    - Falsy 값 ⇒ false 으로 암묵적 타입 변환
      - false
      - undefined
      - null
      - 0, -0
      - NaN
      - ‘’ (빈 문자열)

## 9.3 명시적 타입 변환

- **문자열 타입으로 변환**

  ```jsx
  // 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
  // 숫자 타입 => 문자열 타입
  String(1); // '1'
  String(NaN); // 'NaN'
  String(Infinity); // 'Infinity'

  String(true); // 'true'
  String(false); // 'false'

  // 2. Object.prototype.toString 메서드를 사용하는 방법
  (1).toString(); // '1'
  NaN.toString(); // 'NaN'

  // 3. 문자열 연결 연산자를 이용하는 방법
  1 + ''; // '1'
  NaN + ''; // NaN
  ```

- **숫자 타입으로 변환**
  - Number 생성자 함수를 new 연산자 없이 호출
  - parseInt, parseFloat 함수를 사용하는 방법
    - 단항 산술 연산자를 이용하는 방법
    - 산술 연산자를 이용하는 방법
- **불리언 타입으로 변환**
  - Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  - ! 부정 논리 연산자를 두 번 사용하는 방법

## 9.4 단축 평가

- **논리 연산자를 사용한 단축 평가**

  - **논리곱(&&) 연산자**

    ```jsx
    'Cat' && 'Dog'; // "Dog"
    ```

    - 두 개의 피연산자가 모두 true로 평가될 때 true를 반환
    - 좌항에서 우항으로 평가
    - 논리 연산의 결괄르 결정하는 두 번째 피연산자 ⇒ 문자열 ‘Dog’을 그대로 반환

  - **논리합(||) 연산자**
    ```jsx
    'Cat' || 'Dog'; // 'Cat'
    ```
    - **두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환**
    - 좌항에서 우항으로 평가
    - 첫 번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가
    - 두 번째 피연산자까지 평가하지 않아도 위 표현식을 평가할 수 있음
    - 즉, 문자열 ‘Cat’을 그대로 반환
      `단축 평가` : 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략
      | 단축 평가 표현식 | 평가 결과 |
      | :-----------------: | :---------: |
      | true \| \| anything | true |
      | false \| \| anything | anything |
      | true && anything | anything |
      | false && anything | false |

- **옵셔널 체이닝 연산자**

  - 좌항의 피연산자가 null 또는 undefined 인 경우 undefined 를 반환하고
  - 그렇지 않으면 우항의 프로퍼티 참조를 이어감

  ```jsx
  var elem = null;

  var value = elem?.value;
  console.log(value); // undefined
  ```

  ```jsx
  // 옵셔널 체이닝 연산자 도입되기 전
  var elem = null;

  var value = elem && elem.value;
  console.log(value); // null;
  ```

- **null 병합 연산자**

  - 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환
  - 그렇지 않다면 좌항의 피연산자를 반환
  - **변수에 기본값을 설정할 때 유용**

  ```jsx
  var foo = null ?? 'default string';
  console.log(foo); // "default string"
  ```
