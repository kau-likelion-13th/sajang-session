## 1. CSS란?

> **CSS(Cascading Style Sheets)는 웹 페이지의 디자인을 담당하는 언어**
> 
- HTML이 웹 페이지의 구조를 정의
- **CSS는 색상, 글꼴, 레이아웃 등을 설정하여 더 아름답고 사용하기 편하도록 함**

참고할 블로그

[🎨 CSS 기본 사용법 & 문법 정리](https://inpa.tistory.com/entry/CSS-%F0%9F%93%9A-css-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC)

## 2. CSS 적용 방법

### 2.1 인라인 스타일

HTML 태그에 직접 `style` 속성을 사용하여 CSS를 적용하는 방법

```html
<p>이 문장은 기본입니다.</p>
<p style="color: red; font-size: 20px;">이 문장은 빨간색입니다.</p>
```

### 2.2 내부 스타일

HTML 문서 내부에 `<style>` 태그를 사용하여 CSS를 작성하는 방법

```html
<style>
p {
    color: blue;
    font-size: 18px;
}
</style>
<p>이 문장은 파란색입니다.</p>
```

### 2.3 🚩외부 스타일

CSS를 별도의 `.css` 파일로 작성하고 HTML에서 불러오는 방법

```css
/* styles.css */
p {
    color: green;
    font-size: 16px;
}
```

```html
<link rel="stylesheet" href="styles.css">
<p>이 문장은 초록색입니다.</p>
```

## 3. CSS 기본 문법

### 3.1 선택자

> **CSS에서는 특정 HTML 요소를 선택하여 스타일을 적용할 수 있다~!**
> 
- **태그 선택자**: 특정 태그에 스타일 적용

```css
h1 {
    color: blue;
}
```

- **클래스 선택자**: 여러 요소에 동일한 스타일 적용

```css
.my-class {
    font-weight: bold;
}
```

```html
<p class="my-class">이 문장은 굵게 표시됩니다.</p>
```

- **아이디 선택자**: 특정 요소에만 스타일 적용

```css
#my-id {
    text-align: center;
}
```

```html
<p id="my-id">이 문장은 가운데 정렬됩니다.</p>
```

- **😎 응용: 특정 요소 안에 속한 태그나 요소에 스타일 적용**

```html
<div class="box">
	<p>여기도 class나 id를 줘도 되지만, 안 써도 가능해요! 어떻게 할까요?</p>
</div>
```

```css
/* box라는 class를 가진 요소 아래의 p태그에 스타일 적용 */
.box p {
		color: "red";
}
```

### 3.2 색상과 배경

```css
body {
    background-color: lightgray;
}

h1 {
    color: navy;
}
```

### 3.3 글꼴과 텍스트 스타일링

```css
p {
    font-family: Arial, sans-serif;
    font-size: 20px;
    text-align: right;
}
```

### 3.4 여백과 패딩

```css
div {
	/* 바깥 여백 */
	margin: 20px;
	/* 안쪽 여백 */ 
	*padding: 10px; 
	border: 1px solid black;* 
}
```

## 5. 주로 많이 쓰이는 스타일 속성

```css
/* 글꼴 및 텍스트 관련 */
font-size: 16px; /* 글자 크기 */
font-weight: bold; /* 글자 굵기 */
text-align: center; /* 텍스트 정렬 */
color: #333; /* 글자 색상 */

/* 배경 관련 */
background-color: lightblue; /* 배경 색상 */
background-image: url('image.jpg'); /* 배경 이미지 */

/* 여백 및 패딩 */
margin: 10px; /* 바깥쪽 여백 */
padding: 20px; /* 안쪽 여백 */

/* 테두리 및 크기 */
border: 1px solid black; /* 테두리 */
border-radius: 10px; /* 테두리 굴곡..? */
width: 100px; /* 가로 크기 */
height: 50px; /* 세로 크기 */

/* 박스 정렬 */
display: flex; /* Flexbox 레이아웃 */
justify-content: center; /* 가로 정렬 */
justify-content: space-between; /* 간격이 일정하게 정렬 */
align-items: center; /* 세로 정렬 */
```

## 😎연습문제(세션에서 진행 예정)

https://www.w3schools.com/css/tryit.asp?filename=trycss_default

위 링크로 들어가시면 HTML editor를 사용할 수 있습니다.

### 🎯 연습문제: 예시 코드를 바탕으로 아래 화면을 만들어보세요👌

먼저, **좌측 HTML 코드를 아래 예시 코드로 변경해주세요.**

```html
<!DOCTYPE html>
<html>
<head>
<style>
body { 

}
h1 {

}
.top {

}
.boldText {

}
.bottom {

}
.bottom p {

}
</style>
</head>
<body>

<!-- 제목 -->
<h1>[2025-03-20] 멋쟁이사자처럼 13기</h1>

<!-- 1번 Area -->
<div class="top">
  <p>1교시는 HTML/CSS/JS 세션입니다.</p>
  <p class="boldText">HTML/CSS 공부할 땐, 여기 W3Schools를 자주 이용하시면 좋아요!</p>
</div>

<!-- 2번 Area -->
<div class="bottom">
  <p>먼저 완료하신 분들은 말씀해주세요!</p>
  <p>세션 자료는 앞으로 틈틈이 살펴봐주세요~</p>
</div>

</body>
</html>
```

![image.png](attachment:1530ef01-1a12-48db-b07a-33c22c7feba8:image.png)

### **💡스타일 적용 가이드(힌트!)**

**배경 색상 변경하기**

👉 `body` 태그에 배경 색상 `orange`을 추가해 보세요!

👉 `body` 태그에 안쪽 여백 `20px`을 추가해 보세요!

**제목 스타일링 하기**

👉 `h1` 태그의 글자 색상 `white`을 변경해 보세요!

**상단 박스 꾸미기**

👉 `.top`  클래스를 가진 요소에 안쪽 여백 `20px` 추가해 보세요!

👉 `.top`  클래스를 가진 요소에 빨간색 둥근 테두리를 추가해보세요!

👉 `.top`  클래스를 가진 요소에 배경색을 `white` 로 변경해보세요!

**강조된 텍스트 스타일링**

👉 `.boldText` 클래스를 가진 요소의 글자를 굵게 `bold` 만들어 보세요!

👉 `.boldText` 클래스를 가진 요소의 글자를 크기를 `20px` 로 변경해 보세요!

**박스를 가로로 정렬하기**

👉 `.bottom` ****클래스를 가진 요소에 Flexbox를 설정해 가로 정렬하고, 두 박스 사이에 일정한 간격을 만들어 보세요!

**박스 안 텍스트 스타일링**

👉 너비를 `45%` 로 변경해 보세요!

👉 `.bottom` 안의 `<p>` 태그에 테두리 `5px solid white` 를 추가해 보세요!

👉 테두리를 둥글게 `10px` 로 변경해 보세요!

👉 안쪽 여백을 `10px` 로 변경해 보세요!

### 정답

```html
<!DOCTYPE html>
<html>
<head>
<style>
body {
background-color: orange;
padding: 20px;
}
h1 {
color: white;
}
.top {
padding: 20px;
border: 5px solid red;
border-radius: 10px;
background-color: white;
}
.boldText {
font-size: 20px;
font-weight: bold;
}
.bottom {
display: flex;
justify-content: space-between;
}
.bottom p {
width: 45%;
border: 5px solid white;
border-radius: 10px;
padding: 10px;
text-align: center;
}
</style>
</head>
<body>

<h1>[2025-03-20] 멋쟁이사자처럼 13기</h1>
<div class="top">
<p>1교시는 HTML/CSS/JS 세션입니다.</p>
<p class="boldText">HTML/CSS 공부할 땐, 여기 W3Schools를 자주 이용하시면 좋아요!</p>
</div>
<div class="bottom">
<p>먼저 완료하신 분들은 말씀해주세요!</p>
<p>세션 자료는 앞으로 틈틈이 살펴봐주세요~</p>
</div>

</body>
</html>
```

### 👽 더 자세히 알아보고 싶다?! (선택.. 이지만 추천.. 아니 필수..)

## ⭐️display: block;과 display:flex;

[이번에야말로 CSS Flex를 익혀보자](https://studiomeal.com/archives/197)

### `display: flex;`

- **가로정렬임을 기억하면 쉽다**

![Untitled](attachment:8026bfd8-0183-4f54-9777-b741ffd677c6:Untitled.png)

→ 보통 이렇게 `flex` 속성을 사용해서 컨테이너로 묶고, 각각의 item에 개별로 속성을 적용할 수 있다.

### `flex-direction`

- 아이템들이 배치되는 축의 방향을 설정합니다
- 기본 설정은 row
- 저는 column방향으로 정렬할 때 자주 사용합니다

![Untitled](attachment:4675d415-85ca-4f76-b1d2-23ebef7a61f3:Untitled.png)

### justify 정렬과 align 정렬

![Untitled](attachment:c20c0351-fc5c-46a3-863e-77209d6d1006:Untitled.png)

### **justify-content : 메인 축 방향 정렬**

```css
.container{
	justify-content:flex-end; /*기본설정, 왼쪽부터 정렬*/
	justify-content:center; /*가운데정렬*/
	justify-content:space-between; /*일정한 간격을 두고 가운데정렬*/
}
```

![Untitled](attachment:a0bde6ba-c9a4-449e-be5b-f6d6f3ba7dae:Untitled.png)

![Untitled](attachment:113b5151-0ab2-4a4f-8253-399e0ada4d65:Untitled.png)

![Untitled](attachment:9fd2eaa4-4c92-40b0-b0e0-8f287b11a718:Untitled.png)

😳참고: space-어쩌구의 배열방식

![Untitled](attachment:ca7de409-36c5-4e37-be9d-70f090918d35:Untitled.png)

### align-content: 메인 축에 수직 방향으로 정렬

```css
.container{
align-items: stretch; /*기본값, 아이템들이 세로 아래까지 쭉늘어남*/
align-items: flex-start; /*stretch와 시작은 같은데 아이템 세로 크기에 맞춰짐*/
align-items: flex-end; /*flex-start의 시작과 반대*/
align-items: center; /*화면상 가운데정렬*/
align-items: baseline; /*텍스트의 베이스라인 기준으로 정렬*/}
```

![stretch](attachment:28e27d5c-d589-41f5-8e4d-487b06a16a27:Untitled.png)

stretch

![flex-start](attachment:b05f4f6e-2504-45a3-b1d9-60d9d6270ab2:Untitled.png)

flex-start

![center](attachment:f759e731-6b73-4c33-993c-f75fdff0ef92:Untitled.png)

center

![baseline](attachment:785a70c8-7c07-467a-94f3-50fb6b11e2c6:Untitled.png)

baseline

### 여백 : padding과 margin

![Untitled](attachment:9115ff8f-7a1e-41c0-9ea1-0551040028e6:Untitled.png)

border를 경계로 나뉘며 `margin`은 바깥쪽 여백, `padding`은 안쪽여백을 의미합니다

❗️`<a>` 태그는 block이 아니라 `inline 요소`기때문에 margin값을 가질수 없습니다❗️
