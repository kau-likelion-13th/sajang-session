## 1. HTML**(HyperText Markup Language)**이란?

> **웹페이지의 구조를 정의하는 마크업 언어**
> 
- 웹사이트의 모든 요소(텍스트, 이미지, 링크, 버튼 등)를 배치하는 데 사용
- **HTML은 프로그래밍 언어가 아니라 마크업 언어**
- 태그(tag)라는 특별한 기호를 사용하여 문서의 구조를 정의

### 1.1 HTML의 특징

- **정적인 웹페이지의 기본 구조 제공**
- **태그(tag) 기반의 마크업 언어**
    - 예를 들어, `<h1>` 태그는 제목을, `<p>` 태그는 문단을 나타내는 태그
- **CSS, JavaScript와 결합하여 웹사이트 완성**
    - 보통 CSS(디자인 및 스타일링) 및 JavaScript(동적 기능 추가)와 함께 사용
- **모든 웹 브라우저에서 지원됨**
    - 크롬, 파이어폭스, 사파리, 엣지 등 모든 웹 브라우저가 HTML을 해석하여 웹페이지를 표시

---

## 2. HTML 문서의 기본 구조

HTML 문서는 `<html>`, `<head>`, `<body>` 3가지 주요 부분으로 구성

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="멋쟁이사자처럼13기 HTML">
    <title>HTML 세션</title>
</head>
<body>
    <h1>안녕하세요!</h1>
    <p>이것은 기본적인 HTML 문서입니다.</p>
</body>
</html>
```

**!! 모든 구성을 외울 필요는 없어요 !!**

- `<html>` : **HTML의 최상위 태그**~!! 이걸로 다른 태그들을 감싸줘요
- `<head>` : **스타일시트( .css 파일 등)** 과 **스크립트(.js 파일 등)** 과 같은 문서의 메타 정보를 포함
- `<body>` : **실제 화면에서 표시되는 콘텐츠 포함**
    - 다른 태그들을 조합하여 **눈에 보이는 내용을 작성하는 곳**
- *각 태그의 상세 역할(참고)*
    
    
    | 태그 | 설명 |
    | --- | --- |
    | `<!DOCTYPE html>` | HTML5 문서임을 선언하며, 웹 브라우저가 올바르게 해석하도록 도움 |
    | `<html lang="ko">` | HTML 문서의 최상위 요소이며, `lang="ko"` 속성을 추가하여 한국어 문서임을 지정 |
    | `<head>` | 문서의 메타 정보 포함 (문서 제목, 인코딩, 설명, 스타일시트, 스크립트 등) |
    | `<meta charset="UTF-8">` | 문자 인코딩을 UTF-8로 설정하여 다양한 언어 지원 |
    | `<meta name="viewport" content="width=device-width, initial-scale=1.0">` | 반응형 디자인을 위해 뷰포트 설정 |
    | `<title>` | 브라우저 탭에 표시되는 제목 |
    | `<body>` | 웹페이지에서 실제 화면에 표시되는 콘텐츠 포함 |
- *🚩VSCode 꿀팁*
    - 빈 html 파일에 **`html:5`** 을 입력하고 클릭하면 기본 html 틀이 완성됩니다~!
    
    ![image.png](attachment:cd47f307-a822-40b4-9669-f0c945f102b9:image.png)
    
    - 그럼 굳이 외울 필요 없겠죠?? **`<body>`** 태그 사이에 편하게 작업 시작하면 됩니다.
    
    ![image.png](attachment:fe5117ed-2528-4ab7-93ab-2a3da8123e4c:image.png)
    

### 2.1 테스트 코드 출력

위에 작성된 코드를 직접 실행시켜볼 수 있어요!

 **“Run Pen”**  버튼을 누르고,  **HTML**  에서 코드 확인,   **Result**  에서 출력 화면 확인    ****

https://codepen.io/izkrhann-the-animator/pen/GgRrOzz

- `<h1>` : html 제목을 만드는 태그
- `<p>` : 문단을 만드는 태그

## 3. HTML 기본 태그

<aside>
💡

HTML 태그는 매우 많아서 굳이 다 외울 필요는 없어요😎

자주 사용되는 태그들만 알아두고, https://www.w3schools.com/tags/ref_byfunc.asp에서 다른 태그들을 구경해보세요!

밑에 예시들은 https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default안에서 **`<body>` 태그** 안에 넣어서 확인해봅시다!

</aside>

### 3.1 제목 태그 (Heading)

> `<h1>` ~ `<h6>` 까지 있어요.
> 

```html
<h1>제목 1</h1>
<h2>제목 2</h2>
<h3>제목 3</h3>
<h4>제목 4</h4>
<h5>제목 5</h5>
<h6>제목 6</h6>
```

- 결과
    
    ![image.png](attachment:7cad59cd-0105-4110-b3ba-787cccc1904b:image.png)
    

### 3.2 단락 태그 (Paragraph)

```html
<p>이것은 단락입니다.</p>
```

- 결과
    
    ![image.png](attachment:b7c8842c-b819-4cbb-b431-2f4a0ab4ef0e:image.png)
    

### 3.3 <div>태그(기본적인 태그이며, 그룹화할 때 사용)

> `<p>`태그는 문단(단락)을 만들기 때문에 기본 태그인 `<div>`와 달리 위아래로 공간이 생긴 것을 볼 수 있어요.
> 

```html
<div>
    <div>HTML</div>
    <div>안녕하세요!</div>
    <p>이것은 섹션의 내용입니다.</p>
    <div>이것은 기본태그입니다. 아무거나 넣을 수 있어요</div>
</div>
```

- 결과
    
    ![image.png](attachment:ab3d6bf1-88da-45fd-be33-3e43a98608b2:image.png)
    

### 3.4 줄바꿈 및 수평선

> `<br>` 태그는 줄바꿈을 만들고, `<hr>` 태그는 수평선을 만들어요.
> 

```html
<p>첫 번째 줄<br>두 번째 줄</p>
<hr>
```

- 결과
    
    ![image.png](attachment:204dea4e-d373-45b8-bf43-88f14ae6c7dc:image.png)
    

### 3.5 목록 태그 (List)

> `ul` 태그는 **unorderd list**이기 때문에 순서 없는 리스트를 만들고,
`ol` 태그는 **orderd list**이기 때문에 순서 있는 리스트를 만들어요.
`ul` 과 `ol` 태그 안에는 **list item**을 만드는 `li` 로 내용을 적을 수 있어요.
> 

```html
<ul>
    <li>항목 1</li>
    <li>항목 2</li>
</ul>

<ol>
    <li>첫 번째</li>
    <li>두 번째</li>
</ol>
```

- 결과
    
    ![image.png](attachment:9a051430-a994-4239-b91a-af3355bdb24c:image.png)
    

### 3.6 링크 태그 (Anchor)

> `a` 태그는 `href` 에 적은 주소로 `target` 에 따라 같은 창에서 페이지를 이동하거나 새로운 창을 열어 보여줄 수도 있어요.
> 

```html
<a href="https://www.google.com" target="_blank" rel="noopener noreferrer">Google로 이동</a>
```

- 결과
    
    ![image.png](attachment:77b313c6-57af-469b-9161-1a7025a08e8d:image.png)
    

### 3.7 테이블 태그 (Table)

> `table` 태그: 테이블(table)을 정의
`tr` 태그: 테이블 행(table row)를 정의
`th` 태그: 테이블 헤더(table header)를 정의
`td` 태그: 하나의 데이터 셀(table data cell)을 정의
> 

```html
<table border="1">
    <tr>
        <th>이름</th>
        <th>나이</th>
        <th>성별</th>
    </tr>
    <tr>
        <td>홍길동</td>
        <td>25</td>
        <td>남자</td>
    </tr>
</table>
```

- 결과
    
    ![image.png](attachment:f96d0400-3d5f-4e12-96fd-d8d61b773967:image.png)
    

---

**🚩아래 내용은 심화 내용이며, 선택적으로 살펴보시면 됩니다!**

## 5. 시맨틱 태그 (Semantic Tags)

> **웹페이지의 의미를 명확하게 전달하는 태그**
검색 엔진이 콘텐츠를 쉽게 분석할 수 있도록 하고, 접근성을 높이는 역할
> 

[시맨틱 태그란? - 태그 요소의 종류와 이점](https://seo.tbwakorea.com/blog/what-is-semantic-tag/)

![image.png](attachment:b28c6154-c371-41aa-8d76-9e8bc200d386:image.png)

| 태그 | 설명 |
| --- | --- |
| `<header>` | 웹페이지의 머리말 부분을 정의 (로고, 메뉴 등 포함) |
| `<nav>` | 내비게이션 링크를 포함하는 영역 |
| `<section>` | 논리적으로 그룹화된 콘텐츠 영역 |
| `<article>` | 독립적인 콘텐츠 영역 (블로그 글, 기사 등) |
| `<aside>` | 보조 콘텐츠 영역 (사이드바 등) |
| `<footer>` | 웹페이지의 바닥글 영역 |

시맨틱 태그를 활용하면 코드의 가독성이 높아지고, SEO(검색 엔진 최적화)에 유리하며, 웹 접근성을 개선할 수 있어요!

### 시맨틱 태그 예제

```html
<!-- 웹페이지의 머리말 부분을 정의 -->
<header>
    <h1>내 웹사이트</h1>
</header>
<!-- 내비게이션 링크(다른 페이지로 이동하는 버튼 등)을 포함하는 영역 -->
<nav>
    <ul>
        <li><a href="#">홈</a></li>
        <li><a href="#">소개</a></li>
    </ul>
</nav>
<!-- 논리적으로 그룹화된 영역 -->
<section>
		<!-- 독립적인 콘텐츠 영역 -->
    <article>
        <h2>블로그 제목</h2>
        <p>이것은 블로그 글입니다.</p>
    </article>
</section>
<!-- 보조 콘텐츠 영역 -->
<aside>
    <p>사이드바 콘텐츠</p>
</aside>
<!-- 웹페이지 바닥글 영역 -->
<footer>
    <p>&copy; 2025 내 웹사이트</p>
</footer>
```

굳이 시멘틱 태그를 지키면서 작성하지 않아도 괜찮아요. 
시멘틱 태그 대신에 **`<div>`** 를 사용해도 똑같은 결과가 나옵니다!
만약에 신경써서 개발한다면, 나중에 코드를 다시 찾아볼 때, 협업할 때 더 좋을 수 있겠죠? 

- 결과
    
    ![image.png](attachment:41cf667c-e8d9-4e20-9d89-e09976becd34:image.png)
    

## 6. <form> 태그

> **`<form>` 태그는 사용자가 데이터를 입력하고 서버에 전송할 수 있는 폼을 정의하는 데 사용**
> 

[🏷️ 폼(Form) 태그 양식 & 종류 - 한방 정리](https://inpa.tistory.com/entry/HTML-%F0%9F%93%9A-%ED%8F%BCForm-%ED%83%9C%EA%B7%B8-%EC%A0%95%EB%A6%AC)

### `<form>` 태그 기본 구조

```html
<form action="서버로보낼URL" method="POST">
  <input type="text" name="username" placeholder="Username">
  <input type="password" name="password" placeholder="Password">
  <button type="submit">Submit</button>
</form>
```

- **action**: 폼이 제출될 때 데이터를 전송할 URL을 지정합니다. 서버에서 데이터를 처리할 스크립트나 페이지의 URL을 입력합니다.
- **method**: 폼 데이터를 서버로 전송하는 방법을 지정합니다. 두 가지 주요 방법이 있습니다.
    - **GET**: 폼 데이터를 URL 쿼리 문자열로 전송 (URL에 데이터가 노출됨).
    - **POST**: 폼 데이터를 HTTP 메시지 본문에 포함시켜 전송 (데이터가 URL에 노출되지 않음).

### **👇 form 태그에서 사용되는 태그들이면서도, 평소에도 자주🚩 쓰이는 태그**

### 1. `<input>` 요소

- `<input>` 태그는 사용자가 데이터를 입력할 수 있는 필드를 제공
- 여러 유형의 입력 필드가 있으며, `type` 속성으로 종류를 지정 가능!

```html
<input type="text"> // 텍스트 입력 필드
<input type="password"> // 비밀번호 입력 필드
<input type="checkbox"> // 체크박스
<input type="radio"> // 라디오 버튼 (하나의 그룹에서 하나만 선택)
<input type="file"> // 파일 선택 필드
<input type="date"> // 날짜 선택 필드
```

### 2. `<textarea>` 요소

멀티 라인 텍스트를 입력할 수 있는 필드

```html
<textarea name="message" rows="4" cols="50" placeholder="Enter your message"></textarea>
```

- **rows**: 표시할 줄 수.
- **cols**: 한 줄에 표시할 문자 수.

### 3. `<button>` 요소

버튼을 생성하며, `type` 속성으로 버튼의 동작을 정의 가능

```html
/* 폼을 제출하는 버튼 */
<button type="submit">Submit</button>

/* 폼의 모든 필드를 초기화하는 버튼 */
<button type="reset">Reset</button>

/* 클릭 시 특정 동작을 할 수 있는 버튼 (JavaScript와 함께 가장~!~! 많이 사용) */
<button type="button" onclick="alert('Hello')">Click Me</button>
```

### 4. `<select>` 및 `<option>` 요소

드롭다운 목록을 생성하는데 사용

```html
<select name="country">
  <option value="usa">USA</option>
  <option value="canada">Canada</option>
  <option value="uk">UK</option>
</select>
```
