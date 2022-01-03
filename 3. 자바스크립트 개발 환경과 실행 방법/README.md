## 3.1 자바스크립트 실행 환경

모든 브라우저뿐 아니라 Node.js 도 자바스크립트 엔진을 내장하고 있음

- **브라우저** : HTML, CSS, JS 를 실행하여 **웹페이지를 브라우저 화면에 렌더링**하는 것이 주된 목적
- **Node.js** : **브라우저 외부에서 JS 실행 환경을 제공**하는 것이 주된 목적

⇒ 브라우저와 Node.js 모두 ECMAScript를 실행할 수 있지만 브라우저와 Node.js에서 ECMAScript 이외에 추가로 제공하는 기능은 호환되지 않음

![3-1](https://user-images.githubusercontent.com/55246584/147937613-94036832-1961-498b-9b2d-a5b87568e869.png)

⇒ 브라우저는 클라이언트 사이드 Web API 제공

⇒ Node.js : Node.js 고유의 API 제공

## 3.2 웹 브라우저

\*구글 크롬 브라우저 기준

![3-2](https://user-images.githubusercontent.com/55246584/147937624-1af4d57a-7196-4a77-948e-028f36fb566d.png)

⇒ 개발자 도구는 브라우저에 기본 내장되어 있으므로 별도로 설치할 필요 X (단축키 F12)

- Elements : 로딩된 웹페이지의 DOM과 CSS를 편집해서 렌더링된 뷰 확인 가능
  (단, 편집 내용이 저장되지는 않음.)
- Console : 로딩된 웹페이지의 에러 또는 console.log 메서드의 실행 결과 확인
- Sources : 로딩된 웹페이지의 JS 코드 디버깅 확인
- Network : 로딩된 웹페이지에 관련된 네트워크 요청 정보와 성능 확인
- Application : 웹 스토리지, 세션, 쿠키 확인 및 관리

![3-3](https://user-images.githubusercontent.com/55246584/147937639-7323be8a-9c37-4b68-860b-ec1799cd0265.png)

→ Dock side : 개발자 도구 분리

![3-4](https://user-images.githubusercontent.com/55246584/147937648-3f9fd4ad-45c6-4780-b6e9-6261a0e85ad6.png)

→ Console 창에서 줄바꿈은 Shift 키 + Enter 키

- **HTML 코드**

  ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<meta charset="UTF-8" />
  		<title>Counter</title>
  	</head>
  	<body>
  		<div id="counter">0</div>
  		<button id="increase">+</button>
  		<button id="decrease">-</button>
  		<script>
  			// 에러를 발생시키는 코드: 선택자는 'counter-x'가 아니라 'counter'를 지정해야 한다.
  			const $counter = document.getElementById('counter-x');
  			const $increase = document.getElementById('increase');
  			const $decrease = document.getElementById('decrease');

  			let num = 0;
  			const render = function () {
  				$counter.innerHTML = num;
  			};

  			$increase.onclick = function () {
  				num++;
  				console.log('increase 버튼 클릭', num);
  				render();
  			};

  			$decrease.onclick = function () {
  				num--;
  				console.log('decrease 버튼 클릭', num);
  				render();
  			};
  		</script>
  	</body>
  </html>
  ```

![3-5](https://user-images.githubusercontent.com/55246584/147937663-634ff620-1eff-4900-a73c-488f1134d124.png)

→ 콘솔창을 열어두지 않는 이상 버튼 클릭할 때마다 발생하는 오류를 확인할 수 없음

![3-6](https://user-images.githubusercontent.com/55246584/147937670-4759885e-8482-4ae9-9718-84250e8bc0cc.png)

→ Source 창을 열어서 **x** 표시 지점의 라인 번호를 클릭하여 **브레이크포인트 (중단점)** 를 검

![3-7](https://user-images.githubusercontent.com/55246584/147937687-8d21e3e0-0a7a-4025-b500-3364b77c40b5.png)

→ 중단점을 걸고 다시 버튼을 클릭하면 위와 같이 **디버깅 모드**로 들어가게 됨.

\*디버깅 : 에러 메세지를 먼저 확인하고 에러 발생 원인을 제거하는 것.

→ $counter에 마우스 커서를 올려 보면 변수값 확인 가능 (여기서는 null 이었기 때문에 오류 발생)

→ 13번째 줄의 ‘counter-x’ 를 ‘counter’ 로 수정 (요소 아이디 잘못 지정했기 때문)

## 3.3 Node.js

- Node.js : 브라우저 이외의 환경에서도 동작시킬 수 있는 JS 실행 환경
- npm : 자바스크립트 패키지 매니저

→ Node.js에서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과

패키지 설치 및 관리를 위한 CLI 제공

\*Code Runner : (윈도우 기준) 단축키 ‘Ctrl + Alt + N’ 를 통해 JS 파일 실행

\*Live Server : 가상 서버가 실행되어 브라우저에 HTML 파일이 자동 로딩
