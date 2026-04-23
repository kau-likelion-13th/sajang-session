  
## 🔢 어노테이션은 얼마나 있을까?

### 📊 Spring/JPA 주요 어노테이션 개수

- **Spring Framework 핵심**: 약 50-60개
- **Spring Boot**: 약 30-40개
- **JPA/Hibernate**: 약 40-50개
- **Spring Security**: 약 15-20개
- **Spring Web**: 약 25-30개

**총 대략 200개 이상!이라서제가세션을하면서모든걸다알려드릴수없습니다.**

하지만 실무에서 자주 쓰는 건 **30-40개 정도**예요.

### 🎯 Order 코드 기준 실무 필수 어노테이션 (약 25개)

### Entity 관련 (8개)

```java
@Entity, @Table, @Id, @GeneratedValue, @Column,
@Enumerated, @ManyToOne, @JoinColumn
```

### Lombok (6개)

```java
@Getter, @Setter, @NoArgsConstructor, @AllArgsConstructor,
@RequiredArgsConstructor, @Builder
```

### Spring Core (5개)

```java
@Service, @Repository, @Controller, @RestController, @Component
```

### HTTP 매핑 (4개)

```java
@GetMapping, @PostMapping, @PutMapping, @DeleteMapping
```

### 기타 중요한 것들 (7개)

```java
@Transactional, @RequestBody, @PathVariable,
@AuthenticationPrincipal, @Scheduled, @Valid, @Autowired
```

---

## 🗄️ Repository 메서드는 언제 선언해야 할까?

### 1. **기본 JpaRepository로 충분한 경우 (선언 불필요)**

```java
// 이런 상황에서는 추가 메서드 선언 안 해도 됨
orderRepository.save(order);           // 저장
orderRepository.findById(id);          // ID로 조회
orderRepository.findAll();             // 전체 조회
orderRepository.delete(order);         // 삭제
orderRepository.count();               // 개수 세기
```

### 2. **메서드 선언이 필요한 상황들**

### 🔍 특정 조건으로 조회할 때

```java
// Order 코드에서 실제 사용된 예시
List<Order> findByStatusAndCreatedAtBefore(OrderStatus status, LocalDateTime dateTime);

// 이런 상황들에서 필요
List<Order> findByUser(User user);                    // 특정 사용자 주문
List<Order> findByStatus(OrderStatus status);         // 특정 상태 주문
List<Order> findByFinalPriceGreaterThan(int price);   // 특정 금액 이상
```

### 📈 통계나 집계 데이터가 필요할 때

```java
@Query("SELECT COUNT(o) FROM Order o WHERE o.status = 'COMPLETE'")
Long countCompleteOrders();

@Query("SELECT SUM(o.finalPrice) FROM Order o WHERE o.user = :user")
Long getTotalSpentByUser(@Param("user") User user);
```

### 🔄 복잡한 조건이나 정렬이 필요할 때

```java
List<Order> findByUserOrderByCreatedAtDesc(User user);  // 최신순 정렬
Page<Order> findByStatus(OrderStatus status, Pageable pageable);  // 페이징
```

### ⚡ 성능 최적화가 필요할 때

```java
@Query("SELECT o FROM Order o JOIN FETCH o.user JOIN FETCH o.item")
List<Order> findAllWithUserAndItem();  // N+1 문제 해결
```

### 3. **실무 판단 기준**

| 상황 | JpaRepository 기본 메서드 | 커스텀 메서드 선언 |
| --- | --- | --- |
| 단순 CRUD | ✅ 충분 | ❌ 불필요 |
| ID로 조회 | ✅ `findById()` | ❌ 불필요 |
| 특정 필드로 조회 | ❌ 불가능 | ✅ `findByXxx()` |
| 여러 조건 조회 | ❌ 불가능 | ✅ `findByXxxAndYyy()` |
| 정렬 필요 | ❌ 불가능 | ✅ `findByXxxOrderByYyy()` |
| 페이징 필요 | ❌ 불가능 | ✅ `Page<T> findByXxx(Pageable pageable)` |
| 통계/집계 | ❌ 불가능 | ✅ `@Query` 사용 |

### 4. **Order 시스템에서 실제 추가할 만한 메서드들**

```java
@Repository
public interface OrderRepository extends JpaRepository<Order, Long> {

    // 현재 사용 중 (스케줄러용)
    List<Order> findByStatusAndCreatedAtBefore(OrderStatus status, LocalDateTime dateTime);

    // 추가로 필요할 수 있는 것들
    List<Order> findByUser(User user);                              // 사용자별 주문
    List<Order> findByUserAndStatus(User user, OrderStatus status); // 사용자+상태
    List<Order> findTop10ByOrderByCreatedAtDesc();                  // 최근 10개 주문

    @Query("SELECT COUNT(o) FROM Order o WHERE DATE(o.createdAt) = CURRENT_DATE")
    Long getTodayOrderCount();                                      // 오늘 주문 수

    @Query("SELECT o.status, COUNT(o) FROM Order o GROUP BY o.status")
    List<Object[]> getOrderCountByStatus();                         // 상태별 통계
}
```

---

## 🚨 핵심 정리

### 어노테이션 관련

- **전체**: 200개 이상 존재
- **실무 필수**: 30-40개 정도
- **Order 코드 기준**: 25개 정도면 충분

### Repository 메서드 선언 시점

1. **기본 CRUD** → 선언 불필요 (JpaRepository 기본 제공)
2. **특정 조건 조회** → 선언 필요 (`findByXxx`)
3. **복잡한 쿼리** → `@Query` 어노테이션 사용
4. **성능 최적화** → Fetch Join 등 고급 기법

### 🎯 실무 꿀팁

- **처음에는 기본 메서드만 사용**하다가 필요할 때 추가하기
- **메서드명 네이밍 규칙**을 잘 지키면 Spring이 자동으로 쿼리 생성
- **복잡한 로직은 Service에서**, **단순 데이터 접근은 Repository에서**
- **성능이 중요하면 @Query로 직접 최적화**

## 🏷️ 핵심 어노테이션 정리

### Entity (Order.java)

```java
@Entity                    // JPA 엔티티 선언
@Getter                    // getter 메서드 자동 생성
@Table(name = "orders")    // 테이블명 지정 (order는 예약어)
@NoArgsConstructor         // 기본 생성자 (JPA 필수)

@Id @GeneratedValue(strategy = GenerationType.IDENTITY)  // 기본키 + AUTO_INCREMENT
@Column(nullable = false)                                // NOT NULL 제약
@Setter(AccessLevel.PRIVATE)                            // private setter 생성
@Enumerated(EnumType.STRING)                            // Enum을 문자열로 저장

@ManyToOne(fetch = FetchType.LAZY)   // N:1 관계, 지연 로딩
@JoinColumn(name = "user_id")        // 외래키 컬럼명
```

### Service (OrderService.java)

```java
@Service                   // 서비스 계층 선언
@RequiredArgsConstructor   // final 필드 생성자 주입
@Transactional            // 트랜잭션 처리
@Scheduled(fixedRate = 60000)  // 60초마다 실행
```

### Controller (OrderController.java)

```java
@RestController           // REST API 컨트롤러
@RequestMapping("/orders") // 기본 URL 경로
@PostMapping              // POST 요청 처리
@GetMapping("/{id}")      // GET 요청 + 경로 변수

@RequestBody              // JSON → Java 객체
@PathVariable             // URL 경로에서 값 추출
@AuthenticationPrincipal  // 로그인 사용자 정보
```

### DTO

```java
@Getter @NoArgsConstructor           // Request DTO
@Getter @NoArgsConstructor @AllArgsConstructor  // Response DTO
```

---

## 🗄️ Repository 메서드 설계

### 기본 JpaRepository 메서드

```java
save(order)           // 저장
findById(id)          // ID로 조회
findAll()             // 전체 조회
delete(order)         // 삭제
existsById(id)        // 존재 여부
```

### 커스텀 메서드 네이밍 규칙

| 패턴 | 예시 | 설명 |
| --- | --- | --- |
| `findBy필드명` | `findByStatus(status)` | 단일 조건 |
| `findBy필드And필드` | `findByUserAndStatus(user, status)` | 여러 조건 |
| `findBy필드Before/After` | `findByCreatedAtBefore(date)` | 날짜 비교 |
| `countBy필드` | `countByUser(user)` | 개수 세기 |
| `existsBy필드` | `existsByUserAndItem(user, item)` | 존재 확인 |

### Order 시스템 실무 메서드 예시

**현재 사용 중:**

```java
List<Order> findByStatusAndCreatedAtBefore(OrderStatus status, LocalDateTime dateTime);
```

**추가로 필요할 수 있는 메서드들:**

```java
// 사용자별 조회
List<Order> findByUser(User user);
List<Order> findByUserOrderByCreatedAtDesc(User user);

// 상태별 조회
List<Order> findByStatus(OrderStatus status);
Long countByStatus(OrderStatus status);

// 날짜 범위 조회
List<Order> findByCreatedAtBetween(LocalDateTime start, LocalDateTime end);

// 금액 기반 조회
List<Order> findByFinalPriceGreaterThan(int minPrice);

// 페이징
Page<Order> findByUser(User user, Pageable pageable);
```

### 복잡한 조회는 @Query 사용

```java
// 월별 매출 통계
@Query("SELECT MONTH(o.createdAt), SUM(o.finalPrice) FROM Order o WHERE YEAR(o.createdAt) = :year GROUP BY MONTH(o.createdAt)")
List<Object[]> getMonthlyRevenue(@Param("year") int year);

// 인기 상품 조회
@Query("SELECT o.item, COUNT(o) FROM Order o WHERE o.status = 'COMPLETE' GROUP BY o.item ORDER BY COUNT(o) DESC")
List<Object[]> getPopularItems();
```

---

## ⚡ 핵심 패턴

### 1. 어노테이션 사용 원칙

- **Entity**: 데이터 보호 중심 (`@Setter` 최소화)
- **Service**: 트랜잭션과 비즈니스 로직 (`@Transactional`)
- **Controller**: HTTP 요청/응답 처리만 (`@RestController`)

### 2. Repository 메서드 추가 시점

- **기본 CRUD로 부족할 때**
- **복잡한 조건 조회가 필요할 때**
- **성능 최적화가 필요할 때**
- **비즈니스 요구사항이 추가될 때**

### 3. 성능 고려사항

```java
// ✅ 인덱스 활용
findByUserId(Long userId)        // user_id 외래키 인덱스 사용
findByStatus(OrderStatus status) // status 컬럼 인덱스 필요

// ⚠️ 전체 테이블 스캔
findByQuantity(int quantity)     // 인덱스 없으면 느림

// ✅ Fetch Join으로 N+1 해결
@Query("SELECT o FROM Order o JOIN FETCH o.user JOIN FETCH o.item")
List<Order> findAllWithUserAndItem();

```

### 4. 자주 하는 실수들

**❌ 잘못된 사용:**

```java
@Entity @Setter  // 전체 필드에 setter → 데이터 무결성 위험
@Service @Component  // 어노테이션 중복
@Transactional // 단순 조회에 트랜잭션 → 성능 저하
```

**✅ 올바른 사용:**

```java
@Entity @Getter @NoArgsConstructor  // 필요한 것만
@Service @RequiredArgsConstructor   // 간결하게
@Transactional(readOnly = true)     // 조회용 최적화
```

---

## 🎯 실무 체크리스트

### Entity 설계 시

- [ ]  `@Entity`, `@Table`, `@Id`, `@GeneratedValue` 필수
- [ ]  `@Column(nullable = false)` 제약 조건 설정
- [ ]  `@Enumerated(EnumType.STRING)` Enum 안전 저장
- [ ]  `@Setter` 최소화, 필요한 곳만 사용

### Repository 설계 시

- [ ]  기본 JpaRepository 메서드로 충분한지 확인
- [ ]  메서드명 네이밍 규칙 준수
- [ ]  복잡한 쿼리는 `@Query` 사용
- [ ]  성능 고려한 인덱스 활용

### Service 설계 시

- [ ]  `@Transactional` 적절히 적용
- [ ]  읽기 전용은 `readOnly = true`
- [ ]  스케줄링 필요시 `@Scheduled`

### Controller 설계 시

- [ ]  HTTP 메서드 올바르게 매핑
- [ ]  `@RequestBody`, `@PathVariable` 적절히 사용
- [ ]  응답 DTO로 변환하여 반환
