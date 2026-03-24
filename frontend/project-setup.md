## 1. 🎯 Node.js와 npm 설치하기(필수)

> **아래 명령어들은 Window → 명령 프롬프트 / macOs → 터미널에서 진행해주세요!**
> 

아래 링크에서 Node.js(LTS)를 다운로드 해주세요 😎

[Node.js — 어디서든 JavaScript를 실행하세요](https://nodejs.org/ko)

**`Node.js`를 설치하면 자동으로 `npm`도 함께 설치됩니다!**

설치 후, 아래 명령어로 버전을 확인해주세요 → 버전이 올바르게 나오면 설치가 완료되었다는 뜻 👌

```bash
node -v
npm -v

# npx는 npm 5.2.0 이상에서 기본 제공되지만 별도로 확인 한번 해주세요
npx -v

# 바탕화면으로 이동
cd Desktop
```

![image.png](attachment:c4efb14e-af72-46fd-96d9-8a7785eda940:image.png)

## 2. 🔎 React 프로젝트 생성하기

> **이미 Git 레포지토리를 생성한 상태에서 React 프로젝트를 만든 후, 업로드하는 방법**
> 

Git 세션에서 이미 Frontend 레포지토리를 만들어놨기 때문에 해당 폴더에서 React 프로젝트를 생성해보겠습니다~!

![image.png](attachment:c187ccf0-11fa-470a-891d-4b33fd0f9a8d:image.png)

![image.png](attachment:bb66887f-2864-4bd4-9c35-29777a31c2bc:image.png)

- Git 세션 과제에서 만들었던 폴더로 들어와주세요!
- 저는 `Jerry-Frontend` 폴더에 들어왔고, `jerry.txt` 파일 1개 있습니다.

![image.png](attachment:15e86f9d-8c8b-48c9-bd73-c3e6645b2bf2:image.png)

- Git 세션에서는 Git Bash(윈도우), Terminal(맥)에서 명령어를 실행했었죠?
- **VSCode에서도 Terminal>Git Bash를 사용할 수 있습니다!**
- 이번 사전 세션 예제에서는 VSCode에서 제공하는 Git Bash를 사용해보겠습니다.

![image.png](attachment:872d89d0-2a3c-4af2-9601-0f71f2f5f98b:image.png)

```bash
# jerry-frontend 폴더 내에 폴더/파일 확인하기
ls -al

# jerry.txt 삭제하기 (꼭 삭제해주세요!)
rm jerry.txt

# 잘 삭제되었는지 확인하기
ls -al

# react 프로젝트 생성 명령어 (마지막에 "."까지 꼭 입력해주세요!)
npx create-react-app .

# 위 사진처럼 "Happy hacking!" 문구가 보이면 성공입니다 ㅎ

# 개발 서버 실행
npm start
```

- (참고) React 프로젝트 생성하기(로컬PC에서 생성 후에 Git에 올리는 방법)
    
    ```bash
    # 원하는 위치로 이동
    cd Desktop
    
    # 프로젝트 생성
    npx create-react-app my-react-project
    # my-react-project는 폴더 이름이므로, 원하는 이름으로 진행하셔도 됩니다.
    
    # 프로젝트 폴더 이동
    cd my-react-project
    
    # 개발 서버 실행
    npm start
    ```
    
    - **`npx`**:
        - `npx`는 **Node.js**와 함께 제공되는 도구
        - 이 도구는 명령어를 실행할 때, 로컬(본인의 PC라고 생각)에 패키지가 없다면 자동으로 인터넷에서 최신 버전의 패키지를 다운로드해서 실행해 줍니다..
    - **`create-react-app`**:
        - `create-react-app`은 React 애플리케이션을 쉽게 생성할 수 있도록 도와주는 공식 도구
    - **`my-react-project`**:
        - `my-react-project`는 새로 생성될 프로젝트의 폴더 이름입니다.
        - 즉, 자신이 원하는 이름으로 변경 가능합니다!
    
    ![image.png](attachment:5d2f2a9e-268e-4025-b33b-a9eb2fc89a43:image.png)
    
    - `npx create-react-app my-react-project` 명령어로 프로젝트를 생성할 수 있습니다.
    - 위 예제는 Desktop(바탕화면)위치에서 프로젝트를 생성하였습니다.
    
    ![image.png](attachment:be34c4e1-9d97-405a-8da7-9809266e1f61:image.png)
    
    - Desktop위치에서 `dir` 명령어(윈도우) 또는 `ls -al` 명령어(맥)을 입력하면 my-react-project 폴더가 생성된 것을 확인할 수 있습니다.
    
    ![image.png](attachment:c187ccf0-11fa-470a-891d-4b33fd0f9a8d:image.png)
    
    - **Visual Studio Code**에 들어와서 **Open Folder**로 자신의 Desktop(바탕화면)에 생성된 `my-react-project` 를 선택합니다.
    
    ![image.png](attachment:7f0ffb38-3955-44b5-b85d-d81e45c08edb:image.png)
    
    - 프로젝트 생성이 완료되었습니다.
    
- (참고) `npx create-react-app .` vs `npx create-react-app name`
    
    **npx create-react-app my-app**
    
    👉 `my-app`이라는 **새 폴더**를 만들고 그 안에 React 프로젝트를 생성
    
    ```
    npx create-react-app my-app
    ```
    
    - 현재 디렉터리(`jerry-frontend` 등) 안에 `my-app`이라는 폴더가 생김
    - `cd my-app`으로 이동해서 개발 진행
    - 기존 폴더에 영향을 주지 않음
    
    📌 **언제 사용?**
    
    - **새로운 프로젝트**를 만들 때 (빈 폴더에서 시작하는 게 안전함)
    
    ---
    
    **npx create-react-app** 
    
    👉 **현재 폴더**에 React 프로젝트를 바로 생성
    
    ```
    npx create-react-app .
    ```
    
    - **현재 폴더가 비어 있어야** 실행 가능 (파일이 있으면 충돌 에러 발생)
    - `.git`, `README.md`, `.txt` 같은 파일이 있으면 삭제해야 함
    - 기존 폴더를 유지하면서 React 프로젝트를 추가할 때 사용
    
    📌 **언제 사용?**
    
    - **이미 생성한 빈 레포에 React 프로젝트를 추가할 때**
    - 기존 폴더에서 프로젝트를 초기화할 때

## 3. 🚀 프로젝트 실행하기

![image.png](attachment:04f0383e-5794-4b4b-b2b5-f7d86a222ba2:image.png)

- `localhost:3000` 에서 다음과 같은 페이지가 나오면 생성한 프로젝트가 성공적으로 실행된 것입니다.
- `npm start` 후에, 자동으로 크롬 새 창이 생기면서 보이게 됩니다.
- 만약 생기지 않았다면, 인터넷 브라우저에서 `localhost:3000` 으로 url을 입력해주셔도 됩니다!

## 4. 📒 디렉토리 세팅하기

```markdown
📦 my-react-project
 ┣ 📂 public                # **정적 파일 (HTML, 이미지, 폰트 등)**
 ┃ ┣ 📜 index.html          ## React 진입점
 ┃ ┣ 📜 favicon.ico         
 ┃ ┗ 📜 manifest.json      
 ┣ 📂 src                  # **소스 코드**
 ┃ ┣ 📂 components         ## 재사용 가능한 UI 컴포넌트
 ┃ ┣ 📂 pages              ## 각 페이지 컴포넌트
 ┃ ┣ 📂 styles             ## CSS 파일
 ┃ ┣ 📜 App.js             ## 메인 컴포넌트
 ┃ ┗ 📜 index.js           
 ┣ 📜 package.json       
 ┣ 📜 .gitignore           ## Git에서 제외할 파일
 ┣ 📜 README.md            ## 프로젝트 설명
 ┗ 📜 yarn.lock / package-lock.json 
```

- 기본적인 react 폴더 구조는 이렇게 구성되어 있습니다.
- `public` 폴더에는 **정적 파일**이 들어가고, `src` 폴더에는 **소스코드**가 들어갑니다.

![image.png](attachment:7f69058a-4a83-4db8-8084-60f520461c28:image.png)

- 필요없는 파일은 삭제해줍니다.

![image.png](attachment:f556c016-fb24-47f6-ad68-d47195decd57:image.png)

- `/src` 폴더 밑에 `/component` , `/pages` , `/styles` 폴더를 생성해줍니다.
- 위 사진처럼 필요없는 파일은 삭제 및 폴더 생성을 해주세요!
    - **/component**
        - **역할:**
            - 재사용 가능한 UI 요소(Component)들을 저장하는 폴더.
            - 여러 페이지에서 공통적으로 사용되는 버튼, 네비게이션 바, 카드, 모달 등과 같은 컴포넌트들이 포함됩니다.
        - **예제:**
            - `Button.js`: 재사용 가능한 버튼 컴포넌트
            - `Navbar.js`: 상단 네비게이션 바
            - `Card.js`: 카드 UI 컴포넌트
    - **/pages**
        - **역할:**
            - 각각의 **페이지 컴포넌트**(예: 홈, 로그인, 대시보드 등)를 저장하는 폴더.
            - `React Router`를 사용할 경우, 라우팅되는 주요 페이지들이 위치함.
        - **예제:**
            - `Home.js`: 메인 페이지
            - `Login.js`: 로그인 페이지
            - `Dashboard.js`: 대시보드 페이지
    - **/styles**
        - **역할:**
            - CSS 파일과 같은 스타일 관련 파일을 관리하는 폴더.
            - `TailwindCSS`, `styled-components` 같은 CSS-in-JS 라이브러리를 사용하면 이 폴더가 필요 없을 수도 있음.
        - **예제:**
            - `global.css`: 프로젝트 전역 스타일 정의
            - `Button.module.css`: 모듈화된 버튼 스타일
            - `Navbar.scss`: 네비게이션 바 스타일
