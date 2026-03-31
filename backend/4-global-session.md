네? 뉴진스 하잎보이요?

아뇨아뇨 글로벌이요

네? 세계요?

그쵸 세계의 규칙입니다.

# Spring Boot Global 패키지 완전 정복

## 실제 프로젝트 구조 기반 완벽 가이드

## 📚 학습 목표

- 실제 프로젝트의 Global 패키지 구조를 완벽히 이해한다
- 각 파일의 실제 역할과 협력 관계를 파악한다
- API 표준화, 보안 설정, 예외 처리의 실제 구현을 학습한다
- 실무에서 바로 활용할 수 있는 Global 패키지 설계 방법을 익힌다

---

## 🗂️ 우리 프로젝트의 실제 Global 구조

```
📦 global
 ┣ 📂 api                    ← 📡 API 응답 표준화
 ┃ ┣ 📄 ApiResponse.java     ← 통일된 응답 형식
 ┃ ┣ 📄 BaseCode.java        ← 응답 코드 인터페이스
 ┃ ┣ 📄 ErrorCode.java       ← 실패 응답 코드들
 ┃ ┣ 📄 ReasonDto.java       ← 응답 상세 정보
 ┃ ┗ 📄 SuccessCode.java     ← 성공 응답 코드들
 ┣ 📂 config                 ← 🔧 전역 설정
 ┃ ┣ 📄 PasswordEncoderConfig.java  ← 비밀번호 암호화
 ┃ ┣ 📄 SecurityConfig.java          ← 보안 설정
 ┃ ┗ 📄 SwaggerConfig.java           ← API 문서화
 ┣ 📂 constant               ← 📋 상수 정의
 ┃ ┗ 📄 OrderStatus.java     ← 주문 상태 enum
 ┣ 📂 controller             ← 🌐 전역 컨트롤러
 ┃ ┗ 📄 RootController.java  ← 헬스체크 등
 ┗ 📂 exception             ← ⚠️ 예외 처리
   ┣ 📄 CustomException.java         ← 커스텀 예외
   ┣ 📄 GeneralException.java        ← 일반 예외
   ┗ 📄 GlobalExceptionHandler.java  ← 전역 예외 처리기
```

---

## 🤔 왜 이런 구조로 만들었을까?

### Global 없이 개발한다면... 😱

**각 Controller마다 다른 응답 형식:**

```java
// ItemController
public String getItem() {
    return "success";  // 단순 문자열
}

// UserController
public Map getUser() {
    Map<String, Object> result = new HashMap<>();
    result.put("status", "ok");
    result.put("user", userData);
    return result;  // Map 형태
}

// OrderController
public ResponseEntity getOrder() {
    return ResponseEntity.ok().body(orderData);  // ResponseEntity
}
```

**예외 처리가 중복되고 일관성 없음:**

```java
// 모든 Controller에서 반복되는 try-catch
@GetMapping("/items/{id}")
public ResponseEntity getItem(@PathVariable Long id) {
    try {
        Item item = itemService.getItem(id);
        return ResponseEntity.ok(item);
    } catch (ItemNotFoundException e) {
        return ResponseEntity.status(404).body("상품 없음");
    } catch (Exception e) {
        return ResponseEntity.status(500).body("서버 오류");
    }
}
```

### Global 패키지로 해결! ✨

- **일관된 응답 형식**: 모든 API가 동일한 구조
- **중앙화된 예외 처리**: 한 곳에서 모든 예외 관리
- **표준화된 설정**: 보안, 문서화 등 통일된 관리
- **재사용 가능한 코드**: 중복 제거와 유지보수 용이

---

## 📡 API 패키지 - 응답 표준화의 핵심

### 📄 ApiResponse.java - 모든 응답의 기본 틀

**편의점 영수증처럼 항상 같은 형식! 🧾**

```java
@Getter
@AllArgsConstructor(access = AccessLevel.PROTECTED)
@JsonPropertyOrder({"isSuccess", "code", "message", "result"})
public class ApiResponse<T> {
    private final Boolean isSuccess;  // 🎯 성공/실패 여부
    private final String code;        // 🔢 응답 코드 (ITEM_2003 등)
    private final String message;     // 💬 사용자에게 보여줄 메시지
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private final T result;           // 📦 실제 데이터 (null이면 제외)

    // ✅ 성공 응답 생성
    public static <T> ApiResponse<T> onSuccess(BaseCode code, T result) {
        return new ApiResponse<>(
            true,
            code.getReason().getCode(),
            code.getReason().getMessage(),
            result
        );
    }

    // ❌ 실패 응답 생성
    public static <T> ApiResponse<T> onFailure(BaseCode code, T data) {
        return new ApiResponse<>(
            false,
            code.getReason().getCode(),
            code.getReason().getMessage(),
            data
        );
    }
}
```

**실제 사용 예시:**

```java
// ItemController에서 사용
@GetMapping("/{itemId}")
public ApiResponse<?> getItem(@PathVariable Long itemId) {
    ItemResponseDto item = itemService.getItemById(itemId);

    // 성공 응답 생성
    return ApiResponse.onSuccess(SuccessCode.ITEM_GET_SUCCESS, item);
}
```

**생성되는 실제 JSON:**

```json
{
  "isSuccess": true,
  "code": "ITEM_2003",
  "message": "상품 조회에 성공했습니다.",
  "result": {
    "id": 1,
    "itemName": "맥북 프로",
    "price": 2000000,
    "brand": "Apple"
  }
}
```

### 📄 BaseCode.java - 응답 코드의 설계 원칙

```java
// 📐 모든 응답 코드가 구현해야 하는 규칙
public interface BaseCode {
    ReasonDto getReason();  // 상세 정보를 반환하는 메서드
}
```

**이 인터페이스가 있는 이유:**

- 모든 응답 코드가 동일한 방식으로 정보 제공
- 새로운 응답 코드 추가 시 일관성 보장
- 컴파일 타임에 오류 방지

### 📄 SuccessCode.java - 성공의 모든 경우들

```java
@Getter
@AllArgsConstructor
public enum SuccessCode implements BaseCode {
    // 🟢 일반적인 성공 응답
    OK(HttpStatus.OK, "COMMON_200", "Success"),
    CREATED(HttpStatus.CREATED, "COMMON_201", "Created"),

    // 👤 사용자 관련 성공
    USER_LOGIN_SUCCESS(HttpStatus.CREATED, "USER_201", "회원가입& 로그인이 완료되었습니다."),
    USER_LOGOUT_SUCCESS(HttpStatus.OK, "USER_200", "로그아웃 되었습니다."),
    USER_INFO_GET_SUCCESS(HttpStatus.OK, "USER_203", "사용자 정보 조회에 성공했습니다."),
    USER_MILEAGE_GET_SUCCESS(HttpStatus.OK, "USER_204", "사용자 마일리지 조회에 성공했습니다."),

    // 🏷️ 카테고리 관련 성공
    CATEGORY_ITEMS_GET_SUCCESS(HttpStatus.OK, "CATEGORY_2001", "카테고리 상품 조회 성공"),
    CATEGORY_ITEMS_EMPTY(HttpStatus.OK, "CATEGORY_204", "해당 카테고리에 등록된 상품이 없습니다."),

    // 📦 상품 관련 성공
    ITEM_GET_SUCCESS(HttpStatus.OK, "ITEM_2003", "상품 조회에 성공했습니다."),

    // 🛒 주문 관련 성공
    ORDER_CREATE_SUCCESS(HttpStatus.CREATED, "ORDER_201", "주문이 성공적으로 생성되었습니다."),
    ORDER_CANCEL_SUCCESS(HttpStatus.OK, "ORDER_2003", "주문이 성공적으로 취소되었습니다.");

    private final HttpStatus httpStatus;  // HTTP 상태 코드
    private final String code;            // 우리만의 코드
    private final String message;         // 메시지

    @Override
    public ReasonDto getReason() {
        return ReasonDto.builder()
                .httpStatus(this.httpStatus)
                .code(this.code)
                .message(this.message)
                .build();
    }
}
```

### 📄 ErrorCode.java - 실패의 모든 경우들

```java
@Getter
@AllArgsConstructor
public enum ErrorCode implements BaseCode {
    // 🔴 일반적인 오류
    BAD_REQUEST(HttpStatus.BAD_REQUEST, "COMMON_400", "잘못된 요청입니다."),
    INTERNAL_SERVER_ERROR(HttpStatus.INTERNAL_SERVER_ERROR, "COMMON_500", "서버 에러, 서버 개발자에게 문의하세요."),

    // 👤 사용자 관련 오류
    USER_NOT_FOUND(HttpStatus.NOT_FOUND, "USER_4041", "존재하지 않는 회원입니다."),
    ALREADY_USED_NICKNAME(HttpStatus.FORBIDDEN, "USER_4031", "이미 사용중인 닉네임입니다."),
    USER_NOT_AUTHENTICATED(HttpStatus.UNAUTHORIZED, "AUTH_0001", "카카오 로그인 정보가 없습니다."),

    // 🔐 JWT 토큰 관련 오류
    WRONG_REFRESH_TOKEN(HttpStatus.NOT_FOUND, "JWT_4041", "일치하는 리프레시 토큰이 없습니다."),
    TOKEN_EXPIRED(HttpStatus.UNAUTHORIZED, "JWT_4011", "토큰 유효기간이 만료되었습니다."),
    TOKEN_INVALID(HttpStatus.FORBIDDEN, "JWT_4032", "유효하지 않은 토큰입니다."),

    // 🏷️ 카테고리 관련 오류
    CATEGORY_NOT_FOUND(HttpStatus.NOT_FOUND, "CATEGORY_4041", "해당 카테고리를 찾을 수 없습니다."),

    // 📦 상품 관련 오류
    ITEM_NOT_FOUND(HttpStatus.NOT_FOUND, "ITEM_4041", "해당 상품을 찾을 수 없습니다."),

    // 🛒 주문 관련 오류
    ORDER_NOT_FOUND(HttpStatus.NOT_FOUND, "ORDER_4041", "해당 주문을 찾을 수 없습니다."),
    ORDER_CANCEL_FAILED(HttpStatus.BAD_REQUEST, "ORDER_4001", "주문 취소에 실패했습니다.");

    private final HttpStatus httpStatus;
    private final String code;
    private final String message;

    @Override
    public ReasonDto getReason() {
        return ReasonDto.builder()
                .httpStatus(this.httpStatus)
                .code(this.code)
                .message(this.message)
                .build();
    }
}
```

**실제 사용 예시:**

```java
// Service에서 예외 발생
@Service
public class ItemService {
    public ItemResponseDto getItemById(Long itemId) {
        Item item = itemRepository.findById(itemId)
            .orElseThrow(() -> GeneralException.of(ErrorCode.ITEM_NOT_FOUND));

        return ItemResponseDto.from(item);
    }
}
```

### 📄 ReasonDto.java - 응답 상세 정보의 담당자

```java
@Getter
@Builder
public class ReasonDto implements BaseCode {
    private HttpStatus httpStatus;  // HTTP 상태 코드 (200, 404, 500 등)
    private String code;            // 우리만의 응답 코드 (ITEM_4041 등)
    private String message;         // 사용자에게 보여줄 메시지

    @Override
    public ReasonDto getReason() {
        return this;  // 자기 자신을 반환
    }
}
```

---

## 🔧 Config 패키지 - 전역 설정의 사령부

### 📄 SecurityConfig.java - 보안의 최전선 🛡️

**우리 쇼핑몰의 보안 담당자! 누가 어디에 들어올 수 있는지 결정**

```java
@RequiredArgsConstructor
@Configuration
public class SecurityConfig {
    private final AuthCreationFilter authCreationFilter;        // JWT 생성 필터
    private final JwtValidationFilter jwtValidationFilter;      // JWT 검증 필터
    private final OAuth2UserServiceImpl oAuth2UserService;      // 카카오 로그인 서비스
    private final OAuth2SuccessHandler oAuth2SuccessHandler;    // 로그인 성공 처리

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            // 🔹 CSRF 비활성화 (API 서버이므로)
            .csrf(AbstractHttpConfigurer::disable)

            // 🔹 CORS 설정 (프론트엔드와 통신 허용)
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))

            // 🔹 접근 권한 설정 - 핵심!
            .authorizeHttpRequests(auth -> auth
                .requestMatchers(
                    "/health",                    // 🟢 헬스체크 - 누구나 접근 가능
                    "/swagger-ui/**",             // 🟢 API 문서 - 개발용
                    "/v3/api-docs/**",
                    "/users/reissue",             // 🟢 토큰 재발급
                    "/users/logout",              // 🟢 로그아웃
                    "/oauth2/**",                 // 🟢 카카오 OAuth
                    "/login/oauth2/**",
                    "/categories/**",             // 🟢 카테고리 조회 - 로그인 없이 가능
                    "/items/**"                   // 🟢 상품 조회 - 로그인 없이 가능
                ).permitAll()                     // ← 위 경로들은 인증 없이 접근 가능
                .anyRequest().authenticated()     // ← 나머지는 모두 JWT 토큰 필요
            )

            // 🔹 세션 사용 안함 (JWT 기반)
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS))

            // 🔹 카카오 OAuth2 로그인 설정
            .oauth2Login(oauth2 -> oauth2
                .successHandler(oAuth2SuccessHandler)     // 로그인 성공 시 처리
                .userInfoEndpoint(userInfo -> userInfo
                    .userService(oAuth2UserService))      // 사용자 정보 처리
            )

            // 🔹 JWT 필터 체인 설정
            .addFilterBefore(authCreationFilter, AnonymousAuthenticationFilter.class)
            .addFilterBefore(jwtValidationFilter, AuthCreationFilter.class);

        return http.build();
    }
}
```

**실제 동작 시나리오:**

**1. 상품 조회 요청 (로그인 불필요)**

```
GET /items/1
↓
SecurityConfig 확인: "/items/**"가 permitAll()에 포함
↓
✅ 인증 없이 통과
↓
ItemController 실행
```

**2. 주문 생성 요청 (로그인 필요)**

```
POST /orders (JWT 토큰 없음)
↓
SecurityConfig 확인: "/orders"가 permitAll()에 없음
↓
anyRequest().authenticated() 적용
↓
❌ 401 Unauthorized 응답
```

**3. 주문 생성 요청 (JWT 토큰 있음)**

```
POST /orders
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
↓
JwtValidationFilter에서 토큰 검증
↓
토큰이 유효하면 인증 정보 설정
↓
✅ 요청 통과
↓
OrderController 실행
```

**CORS 설정 상세:**

```java
@Bean
public CorsConfigurationSource corsConfigurationSource() {
    CorsConfiguration configuration = new CorsConfiguration();

    // 허용할 프론트엔드 도메인들
    configuration.setAllowedOrigins(Arrays.asList(
        "http://localhost:3000",                              // 로컬 개발
        "http://sajang-dev.ap-northeast-2.elasticbeanstalk.com",  // 개발 서버
        "https://likelionshop.netlify.app"                    // 운영 서버
    ));

    // 허용할 HTTP 메서드들
    configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));

    // 허용할 헤더들 (JWT 토큰 포함)
    configuration.setAllowedHeaders(Arrays.asList("Authorization", "Content-Type"));

    // 쿠키 등 자격 증명 포함 여부
    configuration.setAllowCredentials(true);

    // 프리플라이트 요청 캐시 시간
    configuration.setMaxAge(3600L);

    UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    source.registerCorsConfiguration("/**", configuration);  // 모든 경로에 적용
    return source;
}
```

### 📄 PasswordEncoderConfig.java - 비밀번호 암호화 🔐

```java
@Configuration
public class PasswordEncoderConfig {
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();  // 최고 수준의 단방향 암호화
    }
}
```

**실제 동작:**

```java
// 회원가입 시
String rawPassword = "mypassword123";
String encodedPassword = passwordEncoder.encode(rawPassword);
// 결과: "$2a$10$N.zmdr9k7NpE4gYurhfYaOdz2VvDLk7/nYLGsDbkbCtVzj7dBLo1e"

// 로그인 시 검증
boolean isValid = passwordEncoder.matches("mypassword123", encodedPassword);
// true - 비밀번호 일치!
```

### 📄 SwaggerConfig.java - API 문서화 📚

**개발자들의 소통 도구! API 사용법을 자동으로 문서화**

```java
@Configuration
public class SwaggerConfig {

    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Shop API")                    // API 제목
                .version("1.0.0")                     // 버전
                .description("Shop API 명세서"))      // 설명

            // JWT 인증 방식 설정
            .addSecurityItem(new SecurityRequirement().addList("Authorization"))
            
            .schemaRequirement("Authorization", new SecurityScheme()
                .name("Authorization")
                .type(SecurityScheme.Type.APIKEY)     // API Key 방식
                .in(SecurityScheme.In.HEADER)         // 헤더에 포함
                .description("Access Token을 입력하세요."));
    }

    @Bean
    public GroupedOpenApi allGroup() {
        return GroupedOpenApi.builder()
            .group("All")                    // 그룹명
            .pathsToMatch("/**")             // 모든 경로 포함
            .build();
    }
}
```

**Swagger UI에서 보이는 내용:**

```
📋 Shop API 1.0.0

🔐 Authorization: [Bearer Token 입력란]

📁 item-controller
  🟢 GET /items/{itemId} - 상품 개별 조회
     Parameters:
     - itemId (path, required): 상품 ID

     Responses:
     - 200: 조회 성공
       {
         "isSuccess": true,
         "code": "ITEM_2003",
         "message": "상품 조회에 성공했습니다.",
         "result": { ... }
       }
     - 404: 상품을 찾을 수 없음

📁 user-controller
  🔵 POST /users/login - 로그인
  🟡 PUT /users/profile - 프로필 수정 (🔒 인증 필요)
```

---

## 📋 Constant 패키지 - 상수의 보관소

### 📄 OrderStatus.java - 주문 상태 관리

```java
public enum OrderStatus {
    PROCESSING,  // 🔄 처리 중
    COMPLETE,    // ✅ 완료
    CANCEL       // ❌ 취소
}
```

**실제 사용 예시:**

```java
@Entity
@Table(name = "orders")
public class Order extends BaseEntity {
    @Enumerated(EnumType.STRING)  // DB에 문자열로 저장
    @Column(nullable = false)
    private OrderStatus status = OrderStatus.PROCESSING;  // 기본값

    // 주문 완료 처리
    public void completeOrder() {
        this.status = OrderStatus.COMPLETE;
    }

    // 주문 취소 처리
    public void cancelOrder() {
        if (this.status == OrderStatus.COMPLETE) {
            throw new IllegalStateException("완료된 주문은 취소할 수 없습니다");
        }
        this.status = OrderStatus.CANCEL;
    }
}
```

**데이터베이스에 실제 저장되는 값:**

```sql
-- orders 테이블
| order_id | status      | created_at |
|----------|-------------|------------|
| 1        | PROCESSING  | 2025-01-15 |
| 2        | COMPLETE    | 2025-01-14 |
| 3        | CANCEL      | 2025-01-13 |
```

---

## 🌐 Controller 패키지 - 전역 컨트롤러

### 📄 RootController.java - 시스템 상태 확인

```java
@Tag(name = "health", description = "health check 관련 api 입니다.")
@RestController
public class RootController {

    @Operation(summary = "cicd 확인", description = "cicd가 정상적으로 작동하고 있는지 확인하는 메서드입니다.")
    @ApiResponses({
        @io.swagger.v3.oas.annotations.responses.ApiResponse(responseCode = "COMMON_200", description = "Success"),
    })
    @GetMapping("/health")
    public String healthCheck() {
        return "OK";  // 간단한 응답으로 서버 상태 확인
    }
}
```

**이 컨트롤러의 역할:**

- **CI/CD 파이프라인**: 배포 후 서버가 정상 동작하는지 확인
- **로드밸런서**: 여러 서버 중 정상 서버만 트래픽 전달
- **모니터링 시스템**: 주기적으로 서버 상태 체크

**실제 사용 시나리오:**

```bash
# 배포 스크립트에서 확인
curl http://localhost:8080/health
# 응답: "OK" (HTTP 200)

# AWS ALB Health Check
Target: /health
Success Code: 200
```

---

## ⚠️ Exception 패키지 - 예외 처리의 달인

### 📄 GlobalExceptionHandler.java - 예외 처리의 총사령관

**응급실의 의료진처럼 모든 예외 상황을 적절히 처리! 🏥**

```java
@RestControllerAdvice(annotations = {RestController.class})  // 모든 RestController의 예외 처리
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    // 🎯 우리가 직접 만든 비즈니스 예외 처리
    @ExceptionHandler(value = GeneralException.class)
    public ResponseEntity<Object> onThrowException(GeneralException generalException,
                                                   HttpServletRequest request) {
        System.out.println("🚨 비즈니스 예외 발생: " + generalException.getReason().getMessage());

        ReasonDto reason = generalException.getReason();
        ApiResponse<Object> body = ApiResponse.onFailure(reason, null);

        return ResponseEntity
            .status(reason.getHttpStatus())
            .body(body);
    }

    // ✏️ 입력값 검증 실패 처리 (@Valid 어노테이션 실패 시)
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(
            MethodArgumentNotValidException e, HttpHeaders headers,
            HttpStatusCode status, WebRequest request) {

        System.out.println("📝 입력값 검증 실패!");

        // 어떤 필드에서 어떤 오류가 발생했는지 정리
        Map<String, String> errors = new LinkedHashMap<>();
        e.getBindingResult().getFieldErrors().forEach(fieldError -> {
            String fieldName = fieldError.getField();           // 예: "itemName"
            String errorMessage = fieldError.getDefaultMessage(); // 예: "상품명은 필수입니다"
            errors.put(fieldName, errorMessage);
        });

        ApiResponse<Object> body = ApiResponse.onFailure(ErrorCode.BAD_REQUEST, errors);
        return ResponseEntity.status(400).body(body);
    }

    // 📊 제약조건 위반 예외 처리 (@Min, @Max 등)
    @ExceptionHandler
    public ResponseEntity<Object> validation(ConstraintViolationException e, WebRequest request) {
        System.out.println("🔢 제약조건 위반!");

        String errorMessage = e.getConstraintViolations().stream()
                .map(ConstraintViolation::getMessage)
                .findFirst()
                .orElse("제약조건 위반");

        return handleExceptionInternalConstraint(e, ErrorCode.BAD_REQUEST, HttpHeaders.EMPTY, request);
    }

    // 🆘 모든 예상치 못한 예외 처리 (최후의 방어선)
    @ExceptionHandler
    public ResponseEntity<Object> exception(Exception e, WebRequest request) {
        System.out.println("💥 예상치 못한 예외: " + e.getClass().getSimpleName());
        System.out.println("📝 메시지: " + e.getMessage());
        e.printStackTrace();  // 로그에 상세 스택 트레이스 출력

        ApiResponse<Object> body = ApiResponse.onFailure(
            ErrorCode.INTERNAL_SERVER_ERROR,
            e.getMessage()
        );
        return ResponseEntity.status(500).body(body);
    }
}
```

### 📄 GeneralException.java - 비즈니스 예외의 대표

```java
@Getter
@AllArgsConstructor
public class GeneralException extends RuntimeException {
    private final BaseCode code;

    // 🏭 예외 생성 팩토리 메서드 (사용하기 편리)
    public static GeneralException of(BaseCode code) {
        return new GeneralException(code);
    }

    // 📋 예외 상세 정보 반환
    public ReasonDto getReason() {
        return this.code.getReason();
    }
}
```

### 📄 CustomException.java - 특별한 상황용 예외

```java
@Getter
public class CustomException extends RuntimeException {
    private final BaseCode errorCode;

    public CustomException(BaseCode errorCode) {
        super(errorCode.getReason().getMessage());
        this.errorCode = errorCode;
    }
}
```

---

## 🔄 실제 프로젝트에서 Global 패키지들이 협력하는 완전한 시나리오

### 🛒 "존재하지 않는 상품 조회" 전체 과정

**실제 우리 프로젝트 코드 기반:**

**1단계: 클라이언트 요청**

```bash
GET /items/999
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
```

**2단계: SecurityConfig 보안 검증**

```java
// SecurityConfig.java에서 확인
.requestMatchers("/items/**").permitAll()  // ✅ 인증 없이 접근 가능
```

**3단계: ItemController에서 요청 받기**

```java
@RestController
@RequestMapping("/items")
@RequiredArgsConstructor
public class ItemController {
    private final ItemService itemService;

    @GetMapping("/{itemId}")
    @Operation(summary = "상품 개별 조회", description = "상품을 개별 조회합니다.")
    public ApiResponse<?> getItemById(@PathVariable Long itemId) {
        System.out.println("🌐 상품 조회 요청: itemId=" + itemId);

        // Service 호출
        ItemResponseDto item = itemService.getItemById(itemId);

        // Global/API 패키지의 표준 응답 생성
        return ApiResponse.onSuccess(SuccessCode.ITEM_GET_SUCCESS, item);
    }
}
```

**4단계: ItemService에서 비즈니스 로직 처리**

```java
@Service
@RequiredArgsConstructor
public class ItemService {
    private final ItemRepository itemRepository;

    @Transactional(readOnly = true)
    public ItemResponseDto getItemById(Long itemId) {
        System.out.println("🧠 상품 조회 처리 시작: itemId=" + itemId);

        // Repository에서 상품 조회 시도
        Item item = itemRepository.findById(itemId)
            .orElseThrow(() -> {
                System.out.println("❌ 상품을 찾을 수 없음! itemId: " + itemId);
                // Global/Exception 패키지의 예외 사용
                return GeneralException.of(ErrorCode.ITEM_NOT_FOUND);
            });

        System.out.println("✅ 상품 찾음: " + item.getItem_name());
        return ItemResponseDto.from(item);
    }
}
```

**5단계: GlobalExceptionHandler가 예외 자동 처리**

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = GeneralException.class)
    public ResponseEntity<Object> onThrowException(GeneralException e, HttpServletRequest request) {
        System.out.println("🚨 상품 조회 예외 처리!");
        System.out.println("📋 예외 코드: " + e.getReason().getCode());      // ITEM_4041
        System.out.println("💬 예외 메시지: " + e.getReason().getMessage());  // 해당 상품을 찾을 수 없습니다.

        // Global/API 패키지를 사용해서 표준 응답 생성
        ReasonDto reason = e.getReason();
        ApiResponse<Object> body = ApiResponse.onFailure(reason, null);

        return ResponseEntity
            .status(reason.getHttpStatus())  // 404 상태 코드
            .body(body);
    }
}
```

**6단계: 클라이언트가 받는 최종 응답**

```json
{
  "isSuccess": false,
  "code": "ITEM_4041",
  "message": "해당 상품을 찾을 수 없습니다.",
  "result": null
}
```

---

### 👤 "닉네임 변경" 입력값 검증 실패 시나리오

**우리 프로젝트의 실제 User 관련 처리:**

**1단계: 잘못된 입력으로 요청**

```bash
PUT /users/nickname
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
Content-Type: application/json

{
  "nickname": "a"  // 너무 짧음 (2자 미만)
}
```

**2단계: DTO 검증 어노테이션**

```java
@Getter
public class UpdateNicknameDto {
    @NotBlank(message = "닉네임은 필수입니다")
    @Size(min = 2, max = 10, message = "닉네임은 2-10자 사이여야 합니다")
    @Pattern(regexp = "^[가-힣a-zA-Z0-9]+$", message = "닉네임은 한글, 영문, 숫자만 가능합니다")
    private String nickname;
}
```

**3단계: Controller에서 @Valid 검증**

```java
@PutMapping("/nickname")
@Operation(summary = "닉네임 변경", description = "사용자의 닉네임을 변경합니다.")
public ApiResponse<?> updateNickname(@Valid @RequestBody UpdateNicknameDto request,
                                   HttpServletRequest httpRequest) {
    // @Valid에서 검증 실패 → MethodArgumentNotValidException 발생
    // GlobalExceptionHandler가 자동으로 처리함
}
```

**4단계: GlobalExceptionHandler가 입력값 검증 오류 처리**

```java
@Override
protected ResponseEntity<Object> handleMethodArgumentNotValid(
        MethodArgumentNotValidException e, HttpHeaders headers,
        HttpStatusCode status, WebRequest request) {

    System.out.println("📝 닉네임 입력값 검증 실패!");

    // 어떤 필드에서 어떤 오류가 발생했는지 수집
    Map<String, String> errors = new LinkedHashMap<>();
    e.getBindingResult().getFieldErrors().forEach(fieldError -> {
        String fieldName = fieldError.getField();           // "nickname"
        String errorMessage = fieldError.getDefaultMessage(); // "닉네임은 2-10자 사이여야 합니다"
        errors.put(fieldName, errorMessage);
    });

    // Global/API 패키지의 표준 오류 응답 생성
    ApiResponse<Object> body = ApiResponse.onFailure(ErrorCode.BAD_REQUEST, errors);
    return ResponseEntity.status(400).body(body);
}
```

**5단계: 클라이언트가 받는 검증 오류 응답**

```json
{
  "isSuccess": false,
  "code": "COMMON_400",
  "message": "잘못된 요청입니다.",
  "result": {
    "nickname": "닉네임은 2-10자 사이여야 합니다"
  }
}
```

---

### 🔐 "JWT 토큰 만료" 인증 실패 시나리오

**보안이 필요한 API 호출 시:**

**1단계: 만료된 토큰으로 주문 생성 요청**

```bash
POST /orders
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6... (만료된 토큰)
Content-Type: application/json

{
  "itemId": 1,
  "quantity": 2
}
```

**2단계: SecurityConfig의 필터 체인 동작**

```java
// SecurityConfig에서 설정한 JwtValidationFilter가 먼저 동작
.addFilterBefore(jwtValidationFilter, AuthCreationFilter.class);
```

**3단계: JwtValidationFilter에서 토큰 검증**

```java
@Component
public class JwtValidationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                   HttpServletResponse response,
                                   FilterChain filterChain) throws ServletException, IOException {

        String token = extractTokenFromHeader(request);  // Bearer 토큰 추출

        try {
            if (token != null && jwtTokenProvider.validateToken(token)) {
                // 유효한 토큰이면 인증 정보 설정
                Authentication authentication = jwtTokenProvider.getAuthentication(token);
                SecurityContextHolder.getContext().setAuthentication(authentication);
                System.out.println("✅ 토큰 검증 성공");
            }
        } catch (TokenExpiredException e) {
            System.out.println("🚨 토큰 만료됨!");
            // Global/Exception 패키지의 예외 발생
            throw new GeneralException(ErrorCode.TOKEN_EXPIRED);
        } catch (InvalidTokenException e) {
            System.out.println("🚨 유효하지 않은 토큰!");
            throw new GeneralException(ErrorCode.TOKEN_INVALID);
        }

        filterChain.doFilter(request, response);
    }
}
```

**4단계: GlobalExceptionHandler가 JWT 예외 처리**

```java
@ExceptionHandler(value = GeneralException.class)
public ResponseEntity<Object> onThrowException(GeneralException e, HttpServletRequest request) {

    // JWT 관련 예외인지 확인
    String errorCode = e.getReason().getCode();
    if (errorCode.startsWith("JWT_")) {
        System.out.println("🔐 JWT 관련 예외 처리: " + e.getReason().getMessage());
    }

    ReasonDto reason = e.getReason();
    ApiResponse<Object> body = ApiResponse.onFailure(reason, null);

    return ResponseEntity
        .status(reason.getHttpStatus())  // 401 Unauthorized
        .body(body);
}
```

**5단계: 클라이언트가 받는 인증 오류 응답**

```json
{
  "isSuccess": false,
  "code": "JWT_4011",
  "message": "토큰 유효기간이 만료되었습니다.",
  "result": null
}
```

**클라이언트의 후속 처리:**

```jsx
// 프론트엔드에서 응답 확인
if (response.code === "JWT_4011") {
    // 토큰 만료 시 재발급 API 호출
    await refreshToken();
    // 원래 요청 재시도
    await retryOriginalRequest();
}
```

---

## 📊 Global 패키지의 실제 효과

### Before vs After 비교

| 영역 | Global 적용 전 | Global 적용 후 |
| --- | --- | --- |
| **API 응답** | 각 Controller마다 다른 형식 | `ApiResponse`로 모든 API 통일 |
| **예외 처리** | 각 메서드마다 try-catch 중복 | `GlobalExceptionHandler`가 자동 처리 |
| **보안 설정** | 여러 파일에 흩어져 있음 | `SecurityConfig` 한 곳에서 관리 |
| **상수 관리** | 하드코딩된 문자열/숫자 | `OrderStatus` 등 의미 있는 enum |
| **에러 코드** | "404", "상품없음" 등 비일관적 | `ErrorCode.ITEM_NOT_FOUND` 체계적 |
| **API 문서** | 수동으로 작성하거나 없음 | `SwaggerConfig`로 자동 생성 |

### 실제 개발 시 장점들

**1. 일관성 있는 API 응답**

```java
// 모든 Controller에서 동일한 패턴
return ApiResponse.onSuccess(SuccessCode.USER_LOGIN_SUCCESS, userDto);
return ApiResponse.onSuccess(SuccessCode.ITEM_GET_SUCCESS, itemDto);
return ApiResponse.onSuccess(SuccessCode.ORDER_CREATE_SUCCESS, orderDto);
```

**2. 예외 처리 코드 중복 제거**

```java
// Before: 각 Controller마다 반복
try {
    // 비즈니스 로직
} catch (ItemNotFoundException e) {
    return ResponseEntity.status(404).body("상품 없음");
} catch (Exception e) {
    return ResponseEntity.status(500).body("서버 오류");
}

// After: Service에서 간단히 예외만 발생
throw GeneralException.of(ErrorCode.ITEM_NOT_FOUND);
// GlobalExceptionHandler가 알아서 처리!
```

**3. 새로운 기능 추가 시 편리함**

```java
// 새로운 ErrorCode만 추가하면 끝!
COUPON_EXPIRED(HttpStatus.BAD_REQUEST, "COUPON_4001", "쿠폰이 만료되었습니다."),

// Service에서 사용
throw GeneralException.of(ErrorCode.COUPON_EXPIRED);
// 자동으로 표준 응답 형식으로 변환됨
```

---

## 🎯 실습 과제

### 1. 새로운 응답 코드 추가하기

**사용자 관련 코드 추가:**

```java
// SuccessCode.java에 추가
USER_PASSWORD_CHANGE_SUCCESS(HttpStatus.OK, "USER_208", "비밀번호가 성공적으로 변경되었습니다."),
USER_PROFILE_IMAGE_UPLOAD_SUCCESS(HttpStatus.OK, "USER_209", "프로필 이미지가 업로드되었습니다."),

// ErrorCode.java에 추가
USER_PASSWORD_MISMATCH(HttpStatus.BAD_REQUEST, "USER_4005", "현재 비밀번호가 일치하지 않습니다."),
USER_PROFILE_IMAGE_TOO_LARGE(HttpStatus.BAD_REQUEST, "USER_4006", "프로필 이미지 크기가 너무 큽니다."),
```

### 2. 새로운 Constant 추가하기

**배송 상태 enum 추가:**

```java
// DeliveryStatus.java
public enum DeliveryStatus {
    PREPARING,    // 배송 준비중
    SHIPPED,      // 배송 중
    DELIVERED,    // 배송 완료
    RETURNED      // 반송
}

// 사용자 역할 enum 추가
public enum UserRole {
    ADMIN,        // 관리자
    SELLER,       // 판매자
    CUSTOMER      // 고객
}
```

### 3. 커스텀 예외 처리 추가하기

**파일 업로드 관련 예외:**

```java
// FileUploadException.java
public class FileUploadException extends CustomException {
    public FileUploadException(BaseCode errorCode) {
        super(errorCode);
    }
}

// GlobalExceptionHandler.java에 추가
@ExceptionHandler(FileUploadException.class)
public ResponseEntity<Object> handleFileUploadException(FileUploadException e) {
    System.out.println("📁 파일 업로드 예외: " + e.getMessage());

    ApiResponse<Object> body = ApiResponse.onFailure(e.getErrorCode(), null);
    return ResponseEntity.status(e.getErrorCode().getReason().getHttpStatus()).body(body);
}
```

### 4. 새로운 보안 규칙 추가하기

**관리자 전용 API 보안 설정:**

```java
// SecurityConfig.java에 추가
.requestMatchers("/admin/**").hasRole("ADMIN")  // 관리자만 접근 가능
.requestMatchers("/seller/**").hasAnyRole("ADMIN", "SELLER")  // 관리자, 판매자 접근 가능
```

---

## ✨ 핵심 포인트 정리

### 🎯 Global 패키지의 핵심 원칙

**1. 일관성 (Consistency)**

- 모든 API 응답이 `ApiResponse` 형식
- 모든 예외가 `GlobalExceptionHandler`에서 처리
- 모든 상수가 enum으로 관리

**2. 중앙화 (Centralization)**

- 보안 설정은 `SecurityConfig`에만
- API 문서화는 `SwaggerConfig`에만
- 예외 처리는 `GlobalExceptionHandler`에만

**3. 재사용성 (Reusability)**

- `BaseCode` 인터페이스로 확장 가능한 구조
- `ApiResponse`는 제네릭으로 모든 타입 지원
- 새로운 기능 추가 시 기존 구조 재활용

**4. 유지보수성 (Maintainability)**

- 변경사항이 한 곳에만 영향
- 새로운 개발자도 쉽게 이해 가능한 구조
- 명확한 책임 분리
