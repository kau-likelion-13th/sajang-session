### 🌐 HTTP 통신이란?

HTTP는 **클라이언트(요청 보내는 쪽)** 🧑‍💻 과 **서버(요청 받는 쪽)** 💻 이 데이터를 주고받는 약속된 방법입니다.

👉 웹사이트, 앱, 서버가 모두 이 방식을 사용해 대화합니다.

### 📬 통신 구조

![image.png](attachment:398faf28-ca28-4e83-ab48-1ce6d43df17a:image.png)

**요청(Request)** ✉️ → 클라이언트가 서버로 “이거 해줘!”

**응답(Response)** 📦 → 서버가 “결과는 이거야!”

### ⚙️ 주요 HTTP 메소드(Method)

| 메소드 | 의미 | 예시 |
| --- | --- | --- |
| **GET** | 데이터 가져오기 📥 | 게시글 목록 보기 |
| **POST** | 새 데이터 보내기 📨 | 회원가입, 로그인 |
| **PUT** | 기존 데이터 전체 수정 🔄 | 프로필 전체 수정 |
| **PATCH** | 일부 데이터 수정 ✏️ | 닉네임만 수정 |
| **DELETE** | 데이터 삭제 🗑️ | 게시글 삭제 |

### 🧾 헤더(Header): “저는 이사장입니다. 디퓨저 페이지 내놔!”

요청에 **추가 정보**를 담는 부분입니다.

데이터 형식, 인증 정보 등이 여기에 들어갑니다.

![naver.com에서 개발자도구(F12) > Network](attachment:8662e100-47b4-4bd5-a719-c69fc87b9c16:image.png)

naver.com에서 개발자도구(F12) > Network

- 헤더를 통해 알아낼 수 있는 사용자 정보
    - **브라우저 정보(User-Agent)**
        - `Mozilla/5.0 (Windows NT 10.0; Win64; x64)`
        - `Chrome/142.0.0.0`
            
            → Windows 10 **운영체제**와 Chrome **브라우저**(버전 142) 를 사용 중
            
    - **요청 출처(Origin)**
        - `https://shopsquare.naver.com/shopping`
            
            → 사용자는 **네이버 쇼핑(ShopSquare)** 관련 페이지를 사용 중
            
    - **요청 시도한 동작**
        - Request URL: `https://shopsquare.naver.com/api/auth`
        - Method: `POST`
    - **IP 정보 (서버 쪽)**
        - Remote Address: `110.93.151.161:443`
            
            → 이건 **서버 IP**이며, 사용자의 개인 IP는 아님
            
    - **Accept-Language / Encoding**
        - `ko-KR, q=0.9` → 한국어 환경
            
            → 시스템 또는 브라우저 기본 언어가 **한국어**로 설정되어 있음
            
    - **보안 연결 여부**
        - Scheme: `https` → **SSL/TLS 암호화 통신** 사용 중

### 🔍 Swagger 보는 법

![image.png](attachment:6ac918b4-3d61-45f8-b091-2945b23402e9:image.png)

- 📍**스웨거 기본 진입 주소**
    
    ```json
    .../swagger-ui/index.html
    ```
    
- 🔐 **Authorize 버튼**
    - 오른쪽 상단의 **Authorize** 버튼은 **인증용 토큰 입력 창**을 여는 기능입니다.
    - 로그인된 사용자만 사용할 수 있는 API인 경우, 여기에서 토큰을 입력해야 합니다.
    - 일반적으로 `Bearer <토큰값>` 형태로 넣습니다.
- 🟢 **POST /users/address**
    - 이 부분이 **API의 정보**입니다.
    - **POST**: 서버에 새 데이터를 보내는 메소드
    - **/users/address**: “회원 주소 저장”을 위한 API 경로입니다.
- **🧾 Parameters / Request body**
    - **Parameters**: URL에 붙는 값
    - **Request body**: 서버로 보낼 실제 데이터(JSON 형식)
- ▶️ **Try it out 버튼**
    - “Try it out”을 누르면 직접 데이터를 입력하고 **API 요청을 테스트**할 수 있습니다.
