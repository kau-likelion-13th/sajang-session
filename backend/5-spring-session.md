## 0️⃣ Spring 이란? 🍃

: 복잡한 설정과 코드 의존성을 줄이고, 객체 지향 프로그래밍(OOP) 원칙을 쉽게 적용하도록 도와주는 자바 언어 기반의 프레임워크 

- 또한 데이터 연동, 트랜잭션 관리, 보안 등을 쉽게 만드는데에 도움을 줍니다.
- 과거에는 개발자가 직접 HttpServletRequest/Response 메서드를 다루며 요청-응답 처리를 해야 했지만, Spring MVC 덕분에 간단한 @Controller와 @RequestMapping으로 요청을 처리할 수 있게 되었습니다!

---

## 1️⃣ 객체 지향 프로그래밍(OOP)이란?

→ 클래스 간의 **의존성을 최소화**하여 코드를 **유지 보수하기 쉽고, 확장 가능하게** 만드는 원칙입니다.

**객체 지향 설계에서 가장 중요한 개념은**

- **단일 책임 원칙**(SRP, Single Responsibility Principle)
- **의존성 주입**(DI, Dependency Injection)

---

### 📌 단일 책임 원칙

> "하나의 클래스는 하나의 책임만 가져야 한다!"
> 

- **`Controller`**: 클라이언트의 요청을 받고, 응답을 반환하는 역할
- **`Service`**: 비즈니스 로직을 처리하는 역할
- **`Repository`**: DB 접근 및 데이터 처리 (예: 회원 저장, 조회)

**이렇게 분리하면 어떤 장점이 있을까요?**

- 집중하는 기능이 분명해져 코드가 깔끔하고 유지 보수하기 쉬워집니다!

---

### 📌 의존성 주입

> "클래스 간의 결합도를 낮추고, 객체를 외부에서 주입받아 사용하자!"  ****
> 
- **의존성**이란?
: 한 클래스가 다른 클래스(객체)를 필요로 하는 관계
- 그렇다면 **의존성 주입**이란?
: 직접 객체를 생성하지 않고 필요한 걸 Spring이 대신 생성하여 주입합니다.

### **의존성 주입 방법**

의존성 주입하는 방법으로는 총 **필드 주입, 세터 주입, 생성자 주입** 세 가지가 있습니다.

그 전에 먼저! 

**📌 어노테이션** 이란?

- 쉽게 말해 프로그램에게 정보를 전달하는 주석

**📌 `final` 변수란?**

- 한번 초기화 하면 바꿀 수 없는 변수
- 객체 생성 시점에 반드시 초기화돼야 하고, 이후 값을 변경할 수 없음
- **객체의 불변성을 보장**해줌
- 의존성이 변경되지 않도록 보호해줌

- 필드 주입
    
    ```java
    public class SomeClass {
        @Autowired // 빈을 자동으로 주입(연결)해주는 역할
        private SomeService someService;
    }
    ```
    
    - `@Autowired`를 사용하여 직접 필드에 의존성을 주입합니다.
    - 가장 간단하지만, 테스트가 어려워지거나, 또 `final` 변수에는 주입할 수 없다는 단점이 있습니다. 
    
    **🙄왜 주입할 수 없을까요?**
        - 필드 주입은 객체 (SomeClass)를 생성한 후, @Autowired 를 보고 의존성 주입을 해주기 때문입니다. 
        즉, **객체 생성 시점에는 someService가 null 상태**이며, Spring이 객체를 만든 후 의존성을 주입하기 때문에 final 변수에는 값을 주입할 수 없습니다.
- 세터 주입
    
    ```java
    public class SomeClass {
        private SomeService someService;
    
        @Autowired
        public void setSomeService(SomeService someService) {
            this.someService = someService;
        }
    }
    ```
    
    - `@Autowired`를 세터 메서드에 사용하여 의존성을 주입하는 방식
    - 이 방식 또한 `final` 변수에는 주입할 수 없습니다.
- 생성자 주입 : 의존성을 생성자를 통해 주입
    - 필드 주입과 세터 주입의 단점을 해결해주는 주입 방식 입니다.
    - 가장 추천되는 방식으로, `final` 키워드로 불변성을 보장할 수 있고, 의존성 주입을 명확히 알 수 있어 코드의 가독성이 높아집니다.
    
    ```java
    public class SomeClass {
        private final SomeService someService;
    
        @Autowired
        public SomeClass(SomeService someService) {
            this.someService = someService;
        } // 이렇게 생긴걸 생성자라 합니다.
    }
    // 자바에서는 생성자 이름 = 클래스 이름 이어야 함
    ```
    
    `final` 은 해당 필드를 생성자에서 반드시 초기화하도록 강제합니다! 
    
    - 생성자 주입은 Spring이 객체 생성 시점에 필요한 의존성을 생성자 파라미터로 넘겨줘요.
    - 그래서 객체가 생성될 때부터 someService는 주입된 상태인 것!
    
    ---
    
    - 이때 `@RequiredArgsConstructor` 를 사용하면 final 또는 @NonNull 필드를 가진 생성자를 자동으로 만들어줍니다! (예습..?..)
    
    ```java
    @RequiredArgsConstructor
    public class SomeClass {
    	private final SomeService someService;
    	
    	//그냥 사용하면 됨..!
    }
    ```
    

**만약 의존성 주입을 안 받는다면..?**

```java
public class OrderService {
    private UserRepository userRepository = new UserRepository();
    
    public void createOrder() {
        User user = userRepository.findById(1L);
        System.out.println("주문 생성 완료: " + user.getName());
    }
}
```

🙄 내부에서 직접 new로 생성하면.. → 즉 빈이 아닌 **일반 자바 객체로 생성**하면

- Spring의 기능을 전혀 적용할 수 없을 뿐더러 단순히 OrderService 내부의 지역 변수 수준으로 존재합니다.
- → 재사용성, 관리성, 유연성이 모두 떨어져요.
- 지역 변수 수준인데 어떻게 userRepository.findById()가 가능? (●'◡'●)
    
    `UserRepository userRepository = new UserRepository();` 이렇게 하면:
    1️⃣ 자바에서 `new` 키워드를 사용해 **UserRepository 객체를 메모리에 생성하고**
    2️⃣ 생성된 객체의 참조를 `userRepository` 변수에 저장합니다.
    3️⃣ 이후에 `userRepository.findById()`처럼 **해당 인스턴스의 메서드를 호출**할 수 있어요.
    

---

# 2️⃣ Spring의 빈

복습복습

> Spring이 객체를 대신 만들어 두고, 필요할 때 이름만 부르면 꺼내 쓸 수 있도록 컨테이너(Bean Container)에 보관한다 는 뜻입니다.
> 
- **Bean**이란 **Spring이 관리하는 객체**입니다. (수명·의존성 모두 Spring 책임, 객체 생성과 관리 제어 Spring에게 넘김)
→ 개발자는 비즈니스 로직에만 집중 가능!
- Spring은 아래와 같은 어노테이션을 통해 클래스를 스캔하고 빈으로 등록해 관리합니다.

```java
@Component   // 공통적으로 사용되는 빈
@Service     // 비즈니스 로직을 담당하는 서비스 빈
@Repository  // 데이터 액세스 계층 (DAO) 빈
@Controller  // 웹 요청을 처리하는 컨트롤러 빈
@RestController // @Controller + @ResponseBody , 반환값을 JSON로 처리 (API 응답 처리)
```

### 다시 의존성 주입 안 받는 애를 데리고 와보자면..

**ex) 의존성 주입 없이 UserRepository를 사용**

```java
public class OrderService {
    private UserRepository userRepository = new UserRepository();
    
    public void createOrder() {
        User user = userRepository.findById(1L);
        System.out.println("주문 생성 완료: " + user.getName());
    }
}
```

이렇게 직접 생성하면 = Spring 기능 사용할 수 X =

- **UserRepository 내부에서 DB 연결, 트랜잭션 관리 등이 전혀 설정되지 않은 상태**로 메모리에 생성됩니다.
- 즉, DB 연결, 트랜잭션 관리, 예외 처리 모두 **직접 구현**해야 합니다.
- Spring 없이 직접 DB 연결 및 조회 하려면..
    
    → Spring을 사용하면 이런 복잡한 작업들은 **build.gradle**과 **application.yml**에 DB 설정만 추가해주면 됩니다!
    
    ```java
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.PreparedStatement;
    import java.sql.ResultSet;
    
    public class UserRepository {
    // 메서드 하나에도 이마아안큼
        public User findById(Long id) {
            User user = null;
            try {
                // 1. DB 연결 정보 설정
                String url = "jdbc:mysql://localhost:3306/your_db";
                String username = "root";
                String password = "your_pw";
    
                // 2. MySQL 드라이버 로드 (필요 시)
                Class.forName("com.mysql.cj.jdbc.Driver");
    
                // 3. DB 연결 생성
                Connection connection = DriverManager.getConnection(url, username, password);
    
                // 4. SQL 쿼리 작성 및 실행
                String sql = "SELECT * FROM users WHERE id = ?";
                PreparedStatement pstmt = connection.prepareStatement(sql);
                pstmt.setLong(1, id);
                ResultSet rs = pstmt.executeQuery();
    
                // 5. 결과 처리
                if (rs.next()) {
                    user = new User(rs.getLong("id"), rs.getString("name"));
                }
    
                // 6. 연결 종료
                rs.close();
                pstmt.close();
                connection.close();
    
            } catch (Exception e) {
                e.printStackTrace();
            }
            return user;
        }
    }
    ```
    

### 반면에 @Repository를 붙이면..?!

- Spring이 관리하는 Bean으로 등록
- → 해당 클래스가 DB와 연동될 수 있도록 필요한 설정을 자동으로 해줍니다!
- 즉, findById()같은 메서드가 실제 DB에 접속해 데이터를 읽어와요.

---

# 3️⃣ 그렇다면 Spring  Boot 란?

**: Spring Boot = Spring 을 더 쉽게 시작하도록 만든 프레임워크**

- Spring은 여러가지 서브 프로젝트들로 이루어져 있어요.

![[다양한 스프링 생태계] - 출처:인프런](attachment:c70002e5-fe9a-4aa5-bebb-207732d87f45:image.png)

[다양한 스프링 생태계] - 출처:인프런

- 이 중 Spring Boot는 톰캣 같은 웹 컨테이너 등을 내장하고 있어서 복잡한 설정 없이 바로 실행할 수 있고, 시작 단계를 줄여 보다 간편하게 Spring 프로젝트를 시작할 수 있도록 해줍니다!
- 톰캣이 뭐에요?
    
    먼저.. 클라이언트의 요청을 처리하는 서버는 **웹서버(Web Server)** 와 **WAS(Web Application Server)**로 나뉩니다.
    
    ### 🌐 **웹 서버와 WAS**
    
    - **웹 서버**는 **정적인 요청(HTML, CSS, JS, 이미지 등)** 을 처리합니다.
    - **WAS**는 **동적인 요청(비즈니스 로직, DB 연동, API 등)** 을 처리합니다.
    
    클라이언트가 요청을 보내면,
    
    - **정적인 파일(HTML, CSS 등)** 은 **웹 서버**가 직접 응답하고,
    - **동적인 요청(로그인, 데이터 처리 등)** 은 **WAS**로 넘겨서 처리한 후 응답합니다.
    
    📌 **즉, 웹 서버와 WAS는 서로를 보완하는 역할을 합니다!**
    
    ### 🐈톰캣 이란?
    
    톰캣은 **WAS의 한 종류**로, **Java 기반의 웹 애플리케이션을 실행하는 서버**
    
    입니다. 톰캣은 기본적으로 WAS 역할(동적 요청 처리)을 하지만, 간단한 웹 서버 역할(정적 파일 처리)도 할 수 있음. Spring Boot는 톰캣을 내장해서 둘 다 자동 처리 가능하게 만듦. 
    
    Spring Boot에는 **톰캣이 기본적으로 내장**되어 있어, 따로 설치하지 않아도 사용할 수 있어요.

