## 2.1 자바스크립트의 탄생

<aside>
💡 웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는 경량 프로그래밍 언어를 도입하면서 탄생
</aside>

- _넷스케이프 커뮤니케이션즈_ 측에서 브렌던 아이크가 개발
  | 시기 | 특징 |
  | ----------- | --------------------------------------------------------- |
  | 1995년 | 넷스케이프 커뮤니케이션즈가 자바스크립트를 도입 |
  | 1996년 3월 | 넷스케이프 내비게이터2에 탑재되었고, 모카 Mocha 로 명명됨 |
  | 1996년 9월 | LiveScript 로 이름 변경 |
  | 1996년 12월 | JavaScript로 최종 명명 |

## 2.2 자바스크립트의 표준화

→ 1996년 8월, 마이크로소프트는 자바스크립트의 파생 버전인 `JScript` 를 인터넷 익스플로러 3.0에 탑재

→ JScript와 자바스크립트가 표준화되지 못하고 적당히 호환 ⇒ `크로스 브라우징 이슈`

→ 모든 브라우저에서 정상적으로 동작하는 표준화된 자바스크립트 필요성이 대두 ⇒ `ECMAScript` 탄생

| 버전                                                                                                                           | 출시 연도 | 특징                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ES1                                                                                                                            | 1997      | 초판                                                                                                                                                                                 |
| ES2                                                                                                                            | 1998      | ISO/IEC 16262 국제 표준과 동일한 규격을 적용                                                                                                                                         |
| ES3                                                                                                                            | 1999      | 정규 표현식, try ... catch                                                                                                                                                           |
| ES5                                                                                                                            | 2009      | HTML5와 함께 출현한 표준안.                                                                                                                                                          |
| JSON, strict mode, 접근자 프로퍼티, 프로퍼티 어트리뷰트 제어, 향상된 배열 조작 기능(forEach, map, filter, reduce, some, every) |
| ES6(ECMAScript 2015)                                                                                                           | 2015      | let/const, 클래스, 화살표 함수, 템플릿 리터럴, 디스트럭처링 할당, 스프레드 문법, rest 파라미터, 심벌, 프로미스, Map/Set, 이터러블, for ... of, 제너레이터, Proxy, 모듈 import/export |
| ES7(ECMAScript 2016)                                                                                                           | 2016      | 지수(\*\*) 연산자, Array.prototype.includes, String.prototype.includes                                                                                                               |
| ES8(ECMAScript 2017)                                                                                                           | 2017      | async/await, Object 정적 메서드(Object.values, Object.entries, Object.getOwnPropertyDescriptors)                                                                                     |
| ES9(ECMAScript 2018)                                                                                                           | 2018      | Object rest/spread 프로퍼티, Promise.prototype.finally, async generator, for await ... of                                                                                            |
| ES10(ECMAScript 2019)                                                                                                          | 2019      | Object.fromEntries, Array.prototype.flat, Array.prototype.flatMap, optional catch binding                                                                                            |
| ES11(ECMAScript 2020)                                                                                                          | 2020      | String.prototype.matchAll, BigInt, globalThis, Promise.allSettled, null 병합 연산자, 옵셔널 체이닝 연산자, for ... in enumeration order                                              |

## 2.3 자바스크립트 성장의 역사

- **초창기 자바스크립트**
  → 웹페이지의 보조적인 기능을 수행하기 위한 한정적인 용도로만 사용
  → 대부분의 로직은 주로 웹 서버에서 실행되었고, 브라우저는 서버로부터 전달받은 HTML과 CSS를 단순히 렌더링하는 수준
  - 렌더링 : HTML, CSS, 자바스크립트로 작성된 문서를 해석해서 브라우저에 시각적으로 출력하는 것.
- **Ajax [XMLHttpRequest]**
  → 자바스크립트를 이용해 서버와 브라우저가 **비동기 방식**으로 데이터 교환할 수 있는 통신 기능

  > **_Ajax 등장 이전_**

      → 완전한 HTML 코드를 서버로부터 전송받아 **웹페이지 전체를 렌더링**하는 방식

      → 단점 : 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터

             다시 렌더링해야됨 → 화면 전환 시 순간적인 화면 깜빡임 현상 및 불필요한 데이터 통신 발생

  > **_Ajax 등장 이후_**
  > → 웹페이지에서 변경할 필요가 없는 부분은 다시 렌더링 X
  > → 서버로부터 **필요한 데이터만 전송받아서 변경해야 할 부분만 한정적으로 렌더링**하는 방식

- **JQuery**
  → DOM을 쉽게 제어
  → 크로스 브라우징 이슈도 어느정도 해결
- **V8 자바스크립트 엔진**
  → 구글 맵스를 통해 웹 애플리케이션 프로그래밍 언어로서의 가능성이 확인된 자바스크립트로
  웹 애플리케이션을 구축하려는 시도가 늘면서 더욱 빠르게 동작하는 JS 엔진의 필요성 대두
  → 이 엔진의 등장으로 JS는 데스크톱 애플리케이션과 유사한 사용자 경험 (UX ; user experience)
  제공할수 있는 웹 애플리케이션 프로그래밍 언어로 정착하게 됨
- **Node.js**

  → 구글 V8 자바스크립트 엔진으로 빌드된 JS 런타임 환경

  → 브라우저의 자바스크립트 엔진에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서도

  동작할 수 있도록 자바스크립트 엔진을 브라우저에서 독립시킨 JS 실행 환경

  → 비동기 I/O를 지원하며, 단일 스레드 이벤트 루프 기반으로 동작 (요청 처리 성능 ↑)

  → 주로 서버 사이드 애플리케이션 개발에 주로 사용됨

- **SPA 프레임워크**
  → CBD 방법론을 기반으로 하는 SPA (Single Page Application) 가 대중화됨
  → 대표적인 프레임워크 : Angular, React, Vue.js, Svelte 등

## 2.4 자바스크립트와 ECMAScript

- **ECMAScript**
  → 자바스크립트으 표준 사양인 ECMA-262 를 말하며, 핵심 문법을 규정
- **자바스크립트**
  → 기본 뼈대를 이루는 ECMAScript와 브라우저가 별도 지원하는 클라이언트 사이드 Web API를 아우르는 개념

## 2.5 자바스크립트의 특징

→ 웹 브라우저에서 동작하는 유일한 프로그래밍 언어

→ 별도의 컴파일 작업을 수행하지 않는 인터프리터 언어

→ 명령형, 함수형, **프로토타입 기반 객체지향 프로그래밍**을 지원하는

멀티 패러다임 프로그래밍 언어

**\*컴파일러** : 컴파일 타임에 **소스코드 전체를 한번에** 머신코드로 변환한 후 실행

⇒ 컴파일과 실행 단계가 분리되어 있으므로 코드 실행 속도가 빠름

**\*인터프리터 언어** : 코드가 실행되는 단계인 런타임에 **문 단위로 한 줄씩** 중간 코드인

바이트코드로 변환한 후 실행

     ⇒ 인터프리트 단계와 실행 단계가 분리되어 있지 않고 반복 수행

     ⇒ 코드 실행 속도가 비교적 느림

## 2.6 ES6 브라우저 지원 현황

![2-1](https://user-images.githubusercontent.com/55246584/147937160-fb1a55ef-2c55-42ab-aaac-c9a66c8139af.png)

→ 대부분의 모던 브라우저의 ES6를 지원 비율은 96~99%

→ 인터넷 익스플로러나 구형 브라우저는 ES6를 대부분 지원 X

[Can I use... Support tables for HTML5, CSS3, etc](https://caniuse.com/)
