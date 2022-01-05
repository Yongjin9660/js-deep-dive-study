## 8.1 블록문

💡 0개 이상의 문을 중괄호로 묶은 것

```js
// 블록문
{
	var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
	x++;
}

// 함수 선언문
function sum(a, b) {
	return a + b;
}
```

## 8.2 조건문

- if ... else 문
  ```js
  if (조건문) {
  	// 조건식이 참이면 실행
  } else {
  	// 조건식이 거짓이면 실행
  }
  ```
- switch 문
  ```js
  switch (표현식) {
  	case 표현식1:
  		// 표현식과 표현식1이 일치하면 실행
  		break;
  	case 표현식2:
      // 표현식과 표현식2가 일치하면 실행
  		break;
  	default:
      표현식과 일치하는 문이 없을 때 실행
  		break;
  }
  ```

## 8.3 반복문

- for 문
- while 문
- do ... while 문

## 8.4 break 문

💡코드 블록을 탈출

## 8.5 continue 문

💡 반복문의 코드 블록 실행을 현 시점에서 중단 => 반복문의 증감식으로 실행 흐름 이동
