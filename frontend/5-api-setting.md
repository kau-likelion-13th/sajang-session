### 1. Axios 설치하기

❓ Axios: HTTP 요청을 쉽게 보낼 수 있게 해주는 JavaScript 라이브러리

```bash
npm i axios
```

설치 시 에러가 발생한다면, axios 버전을 다운그레이드 하여 재설치합니다.

```bash
npm uninstall axios 
npm install axios@1.3.5
```

재설치하였는데, crypto-browserify 관련 에러가 발생한다면, 다음을 실행합니다.

```bash
npm i crypto-browserify
```

일부 환경에서는 crypto 모듈 관련 에러가 발생할 수 있어, 이를 해결하기 위해 설치하는 것입니다.

---

### 2. proxy 설정 추가

package.json 파일 “private” 아래에 proxy 설정을 추가합니다.

```json
"proxy": "[http://sajang-dev-env-2.eba-3ycixkjh.ap-northeast-2.elasticbeanstalk.com](http://sajang-dev-env-2.eba-3ycixkjh.ap-northeast-2.elasticbeanstalk.com/)", 
```

**프록시(proxy)란?**

사전적 의미: 대리인

![image.png](attachment:cebbb7f2-c341-4c4d-ac0f-2a9c4b609a9f:image.png)

프록시는 브라우저가 직접 API 서버와 통신하는 대신, 중간 서버를 거쳐 요청을 전달하는 방식입니다.

👽 **프록시, 왜 쓰나요?**

- **1. 보안:** 사용자의 IP 주소를 숨기고 익명성을 제공 → 사용자의 개인 정보를 보호
- **2. 캐싱:** 자주 요청되는 데이터를 저장해 응답 속도 향상
- **3. 콘텐츠 필터링**: 특정 웹사이트나 콘텐츠에 대한 접근을 제한
- **4. 접근 제어**: 네트워크 접근을 제어하고 모니터링할 수 있음
- **5.** **로깅 및 모니터링**: ****모든 요청과 응답을 기록하여 네트워크 사용 현황을 분석할 수 있음

### 🧐 프록시(proxy) 설정 그래서 ***우리는?***  왜 하나요?

`proxy` 설정을 통해 React 개발 서버가 API 요청을 대신 보내도록 하면 CORS 문제를 해결할 수 있습니다.

❓ **CORS 문제란?**

CORS(Cross-Origin Resource Sharing)는 **웹 브라우저가** 보안 정책(Same-Origin Policy)에 의해 다른 도메인에서 요청하는 리소스를 차단하는 기능입니다.
예를 들어, `http://localhost:3000`에서 실행 중인 React 애플리케이션이 `http://api.example.com`의 데이터를 가져오려고 하면 CORS 정책 위반으로 인해 요청이 차단될 수 있습니다.

CORS 에러는 **브라우저가 다른 도메인으로의 직접 요청을 차단**해서 발생

CORS는 **브라우저 보안 정책**이라 서버 간 통신(프록시→API)에는 적용되지 않기 때문

브라우저 입장에서는 같은 도메인 통신이라 CORS 체크 안 함*(오예!)*

1. **CORS 문제 해결**: 브라우저에서 차단되는 API 요청을 개발 서버를 통해 우회할 수 있습니다.
2. **보안성 증가**: 클라이언트 측에서 직접 API URL을 노출하지 않고, 개발 서버를 통해 관리할 수 있습니다.
3. **간편한 API 요청**: API 요청 시 `http://sajang-dev-env-2.elasticbeanstalk.com`과 같은 전체 URL을 입력하지 않아도 됩니다. 예를 들어, `axios.get("/categories/1/items")`로 요청할 수 있습니다.
