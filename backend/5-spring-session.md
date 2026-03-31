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
    
## Spring Boot 프로젝트 생성
<aside>
💡

맥북은 인텔리제이 프로가 아니면 데이터베이스가 안된다고 합니다.

</aside>

## 1️⃣프로젝트 생성

[**start.spring.io](http://start.spring.io)   👈**

![image.png](attachment:d1a1dc92-2f81-4ab7-abac-2ed82e505289:image.png)

![image.png](attachment:466fdf4d-e893-48a0-a177-9f8ccfb0cc73:1ed05782-8948-4ebf-bb30-11aa21a10198.png)

**Project Metadata**

- Group : 기업 도메인명 
→ likelion13th 으로 설정해주겠습니다!
- Artifact : 결과물
→ 원하는 쇼핑몰 이름으로 해주세요~
- Name : 프로젝트명 
(일반적으로 Artifact와 동일)
- Description : 설명
- Package name : 패키지 이름
(자동으로 설정해줌)

![image.png](attachment:6a88d06c-0e12-431b-90db-4195589cfb9e:image.png)

**Dependencies** 

- Spring Web

<aside>
💡

이때 자바 설정은 **Java17** 로 진행합니다!

→ 윈도우와 맥북 자바 설치 방법이 다릅니다. 이 부분은 인터넷을 참고해주시길 바랍니다!

</aside>

### 📌 방금 한 Dependencies 추가란..?

→ 어떤 기능을 사용할지 선택하는 과정입니다!

→ 하나하나 라이브러리를 찾아 추가할 필요 없이 자동으로 추가 해줌

- Spring Web을 추가해줌으로써 웹을 개발할 때 필요한 라이브러리를 자동으로 추가해 줌
- 만약 intelliJ 한 프로젝트에서 프론트엔드까지 다룬다면..?
→ **Thymeleaf**를 추가하면 HTML을 동적으로 렌더링 할 수 있는 템플릿 엔진이 자동으로 설정됨 
→ Spring Boot와 함께 사용하면, HTML 코드에 데이터를 동적으로 넣을 수 있음
- (DI는 객체끼리 연결해주는 것, build.gradle의 dependencies 추가는 프로젝트에 필요한 라이브러리를 추가하는 것!!!)

- +  동적으로 렌더링..?
    
    ![스크린샷 2025-02-10 124647.png](attachment:1fa8aed8-cad2-4f1d-934b-a07510da56af:스크린샷_2025-02-10_124647.png)
    
    먼저 **정적 렌더링**은 단어 의미 그대로 ‘변화가 없는’ 방식입니다. 미리 만들어진 HTML을 그대로 전달하는 방식이라, 항상 같은 내용이 표시돼요.
    
    하지만 사용자에 따라 다른 데이터가 보여야 할 페이지들이 있잖아요?! 예를 들어, 마이페이지처럼 사용자별로 맞춤형 정보를 제공해야 하는 경우  HTML로 미리 만들어두기엔 한계가 있습니다.
    
    이럴 때 필요한 것이 바로 **동적 렌더링**을 해주어야 합니다! 이 방식은 사용자의 요청에 따라 서버에서 데이터를 가공해 HTML을 동적으로 생성하는 방식입니다.
    

---

### **📂 이제  GENERATE 눌러서 파일을 다운로드해주세요!**

- 로컬디스크 (C:)→ 사용자 → (사용자 이름) → Documents 에서 압축을 풀어줍시다!
    
    D 드라이브, 네트워크 드라이브, 특정 사용자 폴더에 있으면 IntelliJ가 정상적으로 접근하지 못할 수 있어요.
    

---

![스크린샷 2025-02-02 221902.png](attachment:804583d1-f38a-4b09-a337-30a185bb3aee:스크린샷_2025-02-02_221902.png)

**IntelliJ 에 들어가서 → 파일 →  열기**

→ 압축 푼 경로로 들어가서 열어줍니다!

파일 이름 안에 파일이름이 있으면 걜!!! 열어줘야해요!!!!!

또는 **압축을 푼 파일에서 우클릭 후**

**Open Folder as IntelliJ~~ 클릭**

프로젝트 신뢰

---

⚠️ Dependencies 추가 시에 **맥북**과 같은 경우는 …

위 사이트에서 체크를 한 후, 굳이 다운을 받을 필요 없이 build.gradle 파일에 들어가면 의존성이 추가가 된 경우가 있습니다. 이런 경우는 그대로 진행하시면 됩니다!

---

로딩이 조금 걸립니다!

필요한 라이브러리와 아까 우리가 넣어준 의존성을 다운로드하고 설정하는 중이에요!

---

### ⚙️ 잘 설정되었는지 확인해볼게요!

1. 프로젝트명(이하 shop..) 한 번 눌러서 아래에 코끼리 모양 **build.gradle**에서 아까 넣었던 **dependencies** 애들 있는지 확인

![스크린샷 2025-02-10 115643.png](attachment:79297415-b248-4f9e-8c54-6a7f879138c9:스크린샷_2025-02-10_115643.png)

→ 그럴 리는 거의 없지만 호옥시라도 없다면 직접 추가해 줘도 됩니다.

1. **src→main→java→likelion13th.shop→ShopApplication 실행**

![스크린샷 2025-02-10 120532.png](attachment:bc1b3d74-7777-4c2a-8399-8d82dbd74b3c:스크린샷_2025-02-10_120532.png)

- 실행하면 밑에 쭈욱 뜰거에요.

![화면 캡처 2025-02-10 120803.png](attachment:c43ea9d0-58e8-41fc-a5e5-e406feba5527:화면_캡처_2025-02-10_120803.png)

→ 웹 서버가 8080 포트에서 실행되었다는 뜻

- (실행한 상태에서!!) 주소창에 [localhost:8080](http://localhost:8080) 을 쳐줍니다.

![스크린샷 2025-02-10 121829.png](attachment:87720ebd-f131-42dc-a770-12c4d8b61d45:ce626414-3da4-497e-84c8-65b49227849f.png)

아무것도 안 적어주었기 때문에 이런 에러가 뜨는 게 정상입니다!

1. 설정 → 프로젝트 구조 → 프로젝트 → **SDK 설정에 17**로 되어있는지 확인!
* JDK는 Java Development Kit를 의미합니다.

---

이제 GitHub와 이 프로젝트를 연결하러 가봅시당

## 2️⃣ GitHub 연결

- Window는 파일 우클릭 후  Git Bash 열기
- Mac은 터미널에서…

![스크린샷 2025-02-10 130126.png](attachment:7a9743ed-f6e2-4b13-a998-5921ecd36731:스크린샷_2025-02-10_130126.png)

1. 초기 설정을 해줄게요.

```bash
git config --global user.name "GitHub 계정 아이디"
git config --global user.email "GitHub 계정 이메일"
```

1. 현재 폴더를 Git 저장소로 만들기

```bash
 git init
```

1. 과제로 만들었던 레포지토리를 열어주세요!

![README 추가를 안하면 주소가 안보일 수 있다고 하네요](attachment:fdc3039e-1236-4b1d-bcef-87b86fa4088d:스크린샷_2025-02-10_130837.png)

README 추가를 안하면 주소가 안보일 수 있다고 하네요

- 주소 복사! → 다시 Git Bash로
1. 로컬 저장소와 GitHub 원격 저장소 연결

```bash
git remote add origin <주소>
```

> 이때 ‘로컬 저장소’는 내 컴퓨터에서 작업하고 있는 Git 저장소를 의미해요. ‘origin’은 원격 저장소를 뜻한다고 보면 돼요~
> 

(git branch -M main) // 현재 브랜치 이름을 main으로 변경

(git pull origin main)

1. 모든 파일 Git에 추가하기

```bash
git add .
```

1. 커밋하여 변경 사항을 저장하기

```bash
 git commit -m “message” 
 📌 커밋 메시지는 현재 변경 내용을 설명하는 메시지!
```

1. 이제 원격 저장소 = GitHub에 올리기

```bash
//첫 푸시는 
 git push -u origin main
 //main은 브랜치 이름
```

→ 다음 푸시부터는 git push 만 입력해도 자동으로 같은 브랜치에 푸시돼요

🫏🫏🫏깃허브 레포지토리로 가서 확인해보아요~!!

📌 **기억해놓으면 좋을 푸시 순서**

1️⃣ git add .

2️⃣ git commit -m “message’

3️⃣ git push origin <branch> / git push
