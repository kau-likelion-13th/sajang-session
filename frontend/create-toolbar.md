### 1. ToolBar 컴포넌트 생성

```python
import React from "react";
import "../styles/ToolBar.css";

const ToolBar = () => {
  return (
    <div className="toolbar-container">
      <img
        src={`${process.env.PUBLIC_URL}/icon/icon_login.svg`}
        alt="login"
        className="toolbar-icon"
      ></img>
      <img
        src={`${process.env.PUBLIC_URL}/icon/icon_recent.svg`}
        alt="recent"
        className="toolbar-icon"
      ></img>
      <img
        src={`${process.env.PUBLIC_URL}/icon/icon_up.svg`}
        alt="up"
        className="toolbar-icon"
      ></img>
      <img
        src={`${process.env.PUBLIC_URL}/icon/icon_down.svg`}
        alt="down"
        className="toolbar-icon"
      ></img>
    </div>
  );
};
export default ToolBar;
```

- ❓ `process.env.PUBLIC_URL`
    
    React에서 **환경 변수(Environment Variable)** 를 통해 **`public` 폴더의 경로를 가져오는 값**입니다.
    
    ```python
    <img src="/icon/icon_up.svg" alt="up" />
    ```
    
    위와 같이 작성할 경우
    👉 React 앱이 `https://mywebsite.com/myapp/` 에 배포되면
    👉 브라우저는 `https://mywebsite.com/icon/icon_up.svg`를 찾음 → **잘못된 경로** ❌
    
    따라서 `process.env.PUBLIC_URL` 를 통해 이미지 경로를 자동으로 조정하게 합니다.
    

```python
.toolbar-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    background-color: #fff;
    position: fixed;
    z-index: 99;
    top: 300px;
    right: 0;
    padding: 20px 15px 15px 20px;
    border-radius: 30px 0 0 30px;
}

.toolbar-icon {
    width: 32px;
    height: 32px;
}
```

```python
import ToolBar from "./components/ToolBar";

function App() {
  return (
    <Router>
      <Header />
      <ToolBar />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/mypage" element={<Mypage />} />
        <Route path="/new" element={<New />} />
        <Route path="/perfume" element={<Perfume />} />
        <Route path="/diffuser" element={<Diffuser />} />
      </Routes>
    </Router>
  );
}
export default App;
```

### 2. 페이지 맨 위/아래로 이동하기

```python
  const MoveToTop = () => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  };

  const MoveToBottom = () => {
    window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
  };
```

```python
 			<img
        src={`${process.env.PUBLIC_URL}/icon/icon_up.svg`}
        alt="up"
        className="toolbar-icon"
        onClick={MoveToTop}
      ></img>
      <img
        src={`${process.env.PUBLIC_URL}/icon/icon_down.svg`}
        alt="down"
        className="toolbar-icon"
        onClick={MoveToBottom}
      ></img>
```

- ❓ `window.scrollTo`
    
    웹 페이지에서 스크롤을 위아래로 이동시킬 수 있습니다.
    
    `top: 0` : 페이지의 최상단으로 이동
    
    `top: document.body.scrollHeight` : 페이지의 전체 높이 값을 가져와 최하단으로 이동
    
    `behavior: "smooth"` : 부드럽게 이동
