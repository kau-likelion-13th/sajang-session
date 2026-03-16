**JavaScript는 기본적인 문법만 익혀두시고, 이후 세션을 진행하면서 실습을 통해서 직접 적용해보는 것이 좋습니다!
코딩이 처음이시거나, 아래 내용이 이해가 안 되더라도 괜찮아요😎
기본적인 문법만 가볍게 짚어보고, 더 알고 싶은 분들은 아래 참고 블로그를 이용해보세요!**

## 1. JavaScript란?

> **JavaScript(자바스크립트)는 웹 페이지를 동적으로 만들어 주는 프로그래밍 언어**
> 
- HTML과 CSS가 웹 페이지의 구조와 디자인을 담당
- **JavaScript는 사용자와 상호작용하는 기능을 추가하는 역할**
- 참고할 블로그 (모두 이해하지 않아도 괜찮습니다.)
    
    [자바스크립트 문법 총정리판](https://inpa.tistory.com/entry/%EA%B9%9C%EC%A7%80-%F0%9F%93%9DJavaScript-%F0%9F%92%AD-%EC%9A%94%EC%95%BD-%EC%B4%9D%EC%A0%95%EB%A6%AC)
    

## 2. JavaScript 코드 작성 방법

CSS와 마찬가지로 인라인, 내부, 외부 방법이 있어요!

### 2.1 인라인 스크립트 (Inline Script)

HTML 태그 안에서 `onclick` 같은 속성을 사용하여 JavaScript를 직접 작성

```html
<button onclick="alert('버튼이 클릭되었습니다!')">클릭하세요</button>
```

### 2.2 내부 스크립트 (Internal Script)

HTML 문서 안의 `<script>` 태그 안에 JavaScript 코드를 작성하는 방법

```html
<script>
  function sayHello() {
    alert("안녕하세요!");
  }
</script>
<button onclick="sayHello()">클릭하세요</button>
```

### 2.3 외부 스크립트 (External Script)

JavaScript 코드를 별도의 파일로 작성하고 HTML에서 불러오는 방법

```html
<script src="script.js"></script>
```

`script.js` 파일에 작성된 코드 예시:

```jsx
function sayHello() {
  alert("안녕하세요!");
}
```

## 3. JavaScript 기초 문법

### 3.1 변수 선언

변수는 데이터를 저장하는 공간

```jsx
var name = "철수"; // 옛날 방식 (지양)
let age = 20; // 변경 가능한 변수
const PI = 3.14; // 변경 불가능한 변수
```

### 3.2 데이터 타입

JavaScript에는 여러 가지 데이터 타입

```jsx
let number = 10; // 숫자
let text = "Hello"; // 문자열
let isTrue = true; // 불리언 (참/거짓)
let nothing = null; // 값이 없음
let notDefined; // undefined (정의되지 않음)
```

- 팁
    
    **`null` vs `undefined` 차이**
    
    둘 다 "값이 없음"을 나타내지만, 의미와 사용 방식이 달라요 😅
    
    |  | `null` | `undefined` |
    | --- | --- | --- |
    | **의미** | "의도적으로 비어 있음" | "값이 정의되지 않음" |
    | **사용 방식** | 프로그래머가 명시적으로 설정 | JavaScript가 자동으로 설정 |
    | **예시 코드** | `let value = null;` | `let value;` (선언만 하고 값 할당 안 함) |

### 3.3 연산자

```jsx
let a = 10;
let b = 5;
console.log(a + b); // 더하기 (15)
console.log(a - b); // 빼기 (5)
console.log(a * b); // 곱하기 (50)
console.log(a / b); // 나누기 (2)
console.log(a % b); // 나머지 (0)
```

### 3.4 조건문

조건문은 특정 조건에 따라 다른 동작을 수행

```jsx
let age = 18;
if (age >= 18) {
  console.log("성인입니다.");
} else {
  console.log("미성년자입니다.");
}
```

### 3.5 반복문

반복문을 사용하면 같은 동작을 여러 번 실행.

```jsx
for (let i = 1; i <= 5; i++) {
  console.log("반복문 실행: " + i);
}
```

### 3.6 함수

함수는 특정 기능을 수행하는 코드 블록

```jsx
function greet(name) {
  console.log("안녕하세요, " + name + "!");
}
greet("철수"); // "안녕하세요, 철수!" 출력
```

## 4. 이벤트 처리

JavaScript를 사용하면 버튼 클릭, 입력 등과 같은 이벤트에 반응 가능

```html
<button id="btn">클릭하세요</button>

<script>
// 위 <button> 태그로 만들어진 화면 속 요소를 클릭했을 때의 이벤트
document.getElementById("btn").addEventListener("click", function() {
  // 경고창 띄우기
  alert("버튼이 클릭되었습니다!");
});
</script>
```

## 5. DOM 조작

DOM(Document Object Model)은 웹 페이지의 요소를 JavaScript로 조작할 수 있도록 해주는 인터페이스

```html
<p id="text">이 문장을 변경해보세요!</p>
<button onclick="changeText()">변경</button>

<script>

// <button> 태그에 onclick 함수로 설정되어 있기 때문에, button을 누르면 함수가 실행!!
function changeText() {
	// 위 text라는 id를 가진 요소의 텍스트를 변경
  document.getElementById("text").innerText = "변경된 문장!";
}

</script>
```

---

**🚩아래 내용은 심화 내용이며, 선택적으로 살펴보시면 됩니다!**

## 6. 비동기 프로그래밍

[🌐 자바스크립트의 핵심 '비동기' 완벽 이해 ❗](https://inpa.tistory.com/entry/%F0%9F%8C%90-js-async)

JavaScript에서는 `setTimeout`, `setInterval`, `Promise`, `async/await` 등을 이용하여 비동기 처리를 할 수 있습니다.

### `setTimeout`, `setInterval`

- setTimeout: **일정한 시간 이후에 실행**
    
    ```jsx
    setTimeout(() => {
      console.log("3초 후 실행");
    }, 3000);
    ```
    
- setInterval: **일정한 주기로 계속 실행**
    
    ```jsx
    setInterval(() => {
      console.log("1초마다 실행!");
    }, 1000);
    ```
    

### `Promise`, `async/awiat`(비동기 흐름 제어)

**비동기 흐름 제어란 API 호출처럼 서버에 요청을 보낸 후, 응답을 기다리는 동안 다른 작업을 먼저 수행하는 것을 말해요!**

**반대로 동기는 요청을 보내면 응답이 올 때까지 기다린 후, 다음 작업을 진행하는 방식이에요.**

**그럼 무조건 비동기가 BEST인가? 그것은 아닙니다!**

**일반적으론 성능 상 그렇지만, 예외적으로 동기처리가 꼭 필요한 경우도 있어요.**

## 7. 로컬 스토리지 활용

브라우저의 `localStorage`를 이용해 데이터를 저장하고 불러올 수 있습니다.

```
localStorage.setItem("username", "철수");
console.log(localStorage.getItem("username")); // "철수" 출력
```

**localStorage와 sessionStorage의 차이점**

- `localStorage`는 데이터를 영구적으로 저장하며, 브라우저 세션이 종료되어도 데이터가 유지
- `sessionStorage`는 데이터가 브라우저 탭이나 세션이 종료되면 사라지며, 페이지를 새로 고침해도 데이터를 유지

**데이터 크기 제한**

- `localStorage`는 대체로 5MB의 저장 용량 제한이 있으며, 이 한도를 넘기면 저장이 불가능

**주의사항!!**

- `localStorage`는 클라이언트 측에 저장되므로 보안에 민감한 정보를 저장하는 것은 권장되지 않습니다. 예를 들어, 사용자 인증 토큰이나 비밀번호 등을 저장할 경우, 해당 데이터가 쉽게 노출될 수 있습니다.
- 중요한 정보는 서버 측에 안전하게 보관하고, `localStorage`에는 최소한의 정보만 저장하는 것이 좋습니다.
