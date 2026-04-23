### 1. Axios를 이용한 API 요청 및 데이터 가져오기

```json
import axios from "axios";
import { useCookies } from "react-cookie";
import { useEffect, useState } from "react";
```

- `import axios from "axios";` → HTTP 요청을 보내기 위해 Axios 라이브러리를 가져옵니다.
- `import { useCookies } from "react-cookie";` → 사용자의 로그인 토큰(`accessToken`)을 쿠키에서 가져오기 위해 `useCookies` 훅을 사용합니다.
- `import { useEffect, useState } from "react";` → React의 상태 관리(`useState`)와 API 요청 후 데이터를 업데이트하기 위한 사이드 이펙트(`useEffect`)를 사용합니다.

```jsx
const [cookies] = useCookies(["accessToken"]);
const [products, setProducts] = useState([]);

useEffect(() => {
    axios
      .get("/categories/1/items")
      .then((response) => {
        setProducts(response.data.result);
      })
      .catch((err) => {
        console.log("API 요청 실패:", err);
      });
  }, []);
```

- `useCookies(["accessToken"])`: 로그인된 사용자의 `accessToken`을 쿠키에서 가져옵니다.
- `useEffect(() => {...}, [cookies.accessToken])`:
    - `accessToken` 값이 변경될 때마다 실행됩니다.
    - `axios.get()`을 사용하여 API 요청을 보냅니다.
    - 요청 시 `Authorization` 헤더에 `Bearer` 토큰을 포함합니다.
    - API 응답 데이터를 받아 `setProducts(response.data.result)`로 상태 업데이트합니다.
    - 요청 실패 시 `console.log("API 요청 실패:", err)`를 통해 오류를 출력합니다.

---

### 2. 데이터 활용

```jsx
product={{
  id: product.id,
  name: product.name,
  brand: product.brand,
  price: product.price,
  imagePath: product.imagePath,
  isNew: product.isNew,
}}
```

- `product` 객체를 생성하여 필요한 정보를 포함시킵니다.
- 이 데이터를 자식 컴포넌트로 전달하여 렌더링할 수 있습니다.
- `id`, `name`, `brand`, `price`, `imagePath`, `isNew` 등의 정보를 사용하여 상품 리스트를 표시합니다.

---

Perfume, Diffuser 페이지에서도 동일한 방식으로 API를 연결하면 됩니다.

**→ 과제!**

---

### 3. 로그인을 하지 않은 상태에서 PayModal을 띄우지 않도록..

- `cookies.accessToken`이 아예 설정되지 않은 경우 콘솔로 값을 출력해보면 `undefined`가 아닌 `{}` 형태의 객체가 출력 되는 것을 확인할 수 있습니다.
- 따라서 `handleCardClick` 함수에서 `cookies.accessToken` 값이 string 형태가 아니면, `alert("로그인이 필요합니다.")`을 띄우고, 모달을 열지 않도록 처리해야합니다.

```jsx
if (typeof cookies.accessToken !== "string") {
  alert("로그인이 필요합니다.");
  return;
}
```
