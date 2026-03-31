# Spring Boot 계층별 아키텍처와 패키지 분리

## 📚 학습 목표

- Spring Boot 프로젝트의 계층별 구조를 이해한다
- 각 계층의 역할과 책임을 파악한다
- 패키지 분리의 필요성과 장점을 학습한다
- Item 예제를 통해 실제 구현을 살펴본다

---

## 🤔 왜 계층별로 분리해야 할까?

### 분리하지 않았을 때의 문제점

```java
// ❌ 모든 로직이 하나의 클래스에 있는 경우
public class ItemEverything {
    // 데이터베이스 접근
    // 비즈니스 로직
    // HTTP 요청 처리
    // 데이터 변환
    // 모든 것이 뒤섞여 있음!
}
```

**문제점:**

- **유지보수 어려움**: 코드가 복잡하고 어디에 무엇이 있는지 찾기 어려움
- **테스트 어려움**: 하나의 기능만 테스트하기 어려움
- **재사용 불가**: 다른 곳에서 일부 기능만 사용하기 어려움
- **협업 어려움**: 여러 개발자가 동시에 작업하기 어려움

### 계층별 분리의 장점

- **관심사의 분리**: 각 계층이 하나의 책임만 가짐
- **유지보수성**: 변경사항이 해당 계층에만 영향
- **테스트 용이성**: 각 계층을 독립적으로 테스트 가능
- **재사용성**: 다른 프로젝트나 기능에서 재사용 가능
- **확장성**: 새로운 기능 추가나 변경이 쉬움

---

## 🏗️ Spring Boot 계층별 아키텍처

```
📦 likelion13th.shop
 ┣ 📂 controller     ← 🌐 웹 계층 (Presentation Layer)
 ┣ 📂 service        ← 🧠 비즈니스 계층 (Business Layer)
 ┣ 📂 repository     ← 💾 데이터 접근 계층 (Data Access Layer)
 ┣ 📂 domain         ← 📋 도메인 계층 (Domain Layer)
 ┣ 📂 DTO            ← 📦 데이터 전송 객체
 ┗ 📂 global         ← 🔧 공통/설정 계층
 
 -> 이 외에도 login, S3 파일이 있지만 이건 별개의 파일이니 추후 설명!
```

---

## 🌐 웹 계층 (Presentation Layer)이란?

### 웹 계층을 카페로 이해해보기 ☕

**카페의 카운터 직원**을 생각해보세요!

- **손님이 주문**: "아메리카노 한 잔 주세요" (HTTP 요청)
- **직원이 주문 받기**: 메뉴판 확인, 가격 안내 (Controller가 요청 받기)
- **바리스타에게 전달**: "아메리카노 한 잔 만들어주세요" (Service 호출)
- **완성된 커피 받기**: 바리스타가 만든 커피를 받음 (Service 결과 받기)
- **손님에게 제공**: "아메리카노 나왔습니다!" (HTTP 응답)

```java
// 카페 카운터 직원 = Controller
@RestController
public class ItemController {
    // 바리스타 = Service
    private final ItemService itemService;

    // 손님의 주문을 받는 메서드
    @GetMapping("/items/{itemId}")
    public ApiResponse<?> getItemById(@PathVariable Long itemId) {
        // 1. 손님 주문 확인: "1번 상품 정보 주세요"
        // 2. 바리스타(Service)에게 요청
        ItemResponseDto item = itemService.getItemById(itemId);
        // 3. 바리스타가 만든 결과를 손님에게 전달
        return ApiResponse.onSuccess(SuccessCode.ITEM_GET_SUCCESS, item);
    }
}
```

---

## 🧠 비즈니스 계층 (Business Layer)이란?

### 비즈니스 계층을 카페로 이해해보기 ☕

**카페의 바리스타**를 생각해보세요!

- **레시피 알고 있음**: 아메리카노 만드는 방법 (비즈니스 로직)
- **재료 확인**: 원두, 물이 충분한지 확인 (데이터 검증)
- **커피 제조**: 실제로 커피를 만드는 과정 (핵심 업무 처리)
- **품질 검사**: 맛있게 잘 만들어졌는지 확인 (결과 검증)

```java
// 바리스타 = Service
@Service
public class ItemService {
    // 재료 창고 = Repository
    private final ItemRepository itemRepository;

    // 커피 만들기 = 비즈니스 로직
    public ItemResponseDto getItemById(Long itemId) {
        // 1. 재료 창고에서 재료 가져오기
        Item item = itemRepository.findById(itemId).orElse(null);

        // 2. 재료가 있는지 확인 (비즈니스 검증)
        if (item == null) {
            return null; // 재료가 없으면 만들 수 없음
        }

        // 3. 추가 비즈니스 로직 (예시)
        // - 할인가 계산
        // - 재고 확인
        // - 권한 검사

        // 4. 손님이 받을 형태로 변환 (DTO 변환)
        return ItemResponseDto.from(item);
    }
}
```

---

## 💾 데이터 접근 계층 (Data Access Layer)이란?

### 데이터 접근 계층을 카페로 이해해보기 ☕

**카페의 창고 관리자**를 생각해보세요!

- **재료 찾기**: "원두 가져와주세요" → 창고에서 원두 찾기 (데이터 조회)
- **재료 저장**: "새로 온 원두 저장해주세요" → 적절한 곳에 보관 (데이터 저장)
- **재고 관리**: 어떤 재료가 어디에 얼마나 있는지 관리 (데이터 관리)
- **창고 규칙**: 유통기한 확인, 보관 온도 관리 (데이터 무결성)

```java
// 창고 관리자 = Repository
@Repository
public interface ItemRepository extends JpaRepository<Item, Long> {

    // 기본 창고 작업들 (JpaRepository가 자동 제공):
    // save(item)           → "이 상품 저장해주세요"
    // findById(id)         → "1번 상품 가져와주세요"
    // findAll()            → "모든 상품 가져와주세요"
    // delete(item)         → "이 상품 버려주세요"
    // count()              → "상품이 총 몇 개 있나요?"

    // 특별한 찾기 작업들:
    List<Item> findByCategories(Category category);     // "이 카테고리 상품들 가져와주세요"
    List<Item> findByPriceBetween(int min, int max);    // "이 가격대 상품들 가져와주세요"
    List<Item> findByBrand(String brand);               // "이 브랜드 상품들 가져와주세요"
}
```

---

**전체 흐름 요약:**

```
📱 클라이언트: "맥북 2개 주문할게요!"
    ↓
🌐 웹 계층: "주문 요청 확인! 비즈니스팀에 전달할게요"
    ↓
🧠 비즈니스 계층: "재고 확인하고, 가격 계산하고, 주문 생성할게요"
    ↓
💾 데이터 계층: "데이터베이스에서 정보 가져오고 저장할게요"
    ↓
🧠 비즈니스 계층: "처리 완료! 결과 만들어서 웹팀에 전달할게요"
    ↓
🌐 웹 계층: "주문 완료 응답을 클라이언트에게 보낼게요"
    ↓
📱 클라이언트: "주문 성공! 감사합니다"
```

---

### 데이터 흐름

```
클라이언트 요청
    ↓
Controller (웹 계층)
    ↓
Service (비즈니스 계층)
    ↓
Repository (데이터 접근 계층)
    ↓
Database
```

---

## ⭐️ 시작하기에 앞서 GLOBAL ⭐️

[GLOBAL 이 뭐예요?](https://www.notion.so/GLOBAL-203f249f748881b99aeff2e6783bbe88?pvs=21)

---

## 🌐 Controller 계층 (웹 계층)

### 역할과 책임

- **HTTP 요청 처리**: 클라이언트의 요청을 받아서 처리
- **응답 반환**: 처리 결과를 클라이언트에게 응답
- **요청 데이터 검증**: 기본적인 입력값 검증
- **서비스 계층 호출**: 실제 비즈니스 로직은 서비스에 위임

### ItemController.java 분석

```java
package likelion13th.shop.controller; // 패키지 선언 - 컨트롤러 클래스들이 위치한 패키지

// 필요한 라이브러리들 import
import io.swagger.v3.oas.annotations.Operation; // Swagger API 문서화를 위한 어노테이션
import likelion13th.shop.DTO.response.ItemResponseDto; // 상품 응답 데이터 전송 객체
import likelion13th.shop.global.api.ApiResponse; // 통일된 API 응답 형식을 위한 클래스
import likelion13th.shop.global.api.ErrorCode; // 에러 코드 정의 enum
import likelion13th.shop.global.api.SuccessCode; // 성공 코드 정의 enum
import likelion13th.shop.service.ItemService; // 상품 관련 비즈니스 로직을 처리하는 서비스
import lombok.RequiredArgsConstructor; // final 필드에 대한 생성자를 자동 생성하는 롬복 어노테이션
import org.springframework.web.bind.annotation.GetMapping; // HTTP GET 요청을 처리하는 어노테이션
import org.springframework.web.bind.annotation.PathVariable; // URL 경로 변수를 매개변수로 받는 어노테이션
import org.springframework.web.bind.annotation.RequestMapping; // 클래스 레벨에서 기본 URL 매핑을 설정하는 어노테이션
import org.springframework.web.bind.annotation.RestController; // REST API 컨트롤러임을 나타내는 어노테이션

@RestController // 이 클래스가 REST API 컨트롤러임을 Spring에 알림 (JSON 응답 자동 변환)
@RequestMapping("/items") // 이 컨트롤러의 모든 엔드포인트 앞에 "/items" 경로가 붙음
@RequiredArgsConstructor // final 필드인 itemService에 대한 생성자를 자동으로 생성 (의존성 주입용)
public class ItemController { // 상품 관련 HTTP 요청을 처리하는 컨트롤러 클래스

    private final ItemService itemService; // 상품 관련 비즈니스 로직을 처리할 서비스 객체 (final로 불변성 보장)

    // ============================================
    // 개별 상품 조회 API (현재 비활성화 상태)
    // ============================================
    
    /*
    @GetMapping("/{itemId}") // HTTP GET 요청을 "/items/{itemId}" 경로로 매핑
    @Operation(summary = "상품 개별 조회", description = "상품을 개별 조회합니다.") // Swagger 문서에 표시될 API 설명
    public ApiResponse<?> getItemById(@PathVariable Long itemId) { // URL의 {itemId} 부분을 Long 타입 매개변수로 받음
        
        // 서비스에서 해당 ID의 상품 정보를 조회
        ItemResponseDto item = itemService.getItemById(itemId);
        
        // 조회된 상품이 없는 경우 에러 응답 반환
        if (item == null) {
            return ApiResponse.onFailure(
                ErrorCode.ITEM_NOT_FOUND,           // 상품을 찾을 수 없다는 에러 코드
                "해당 상품을 찾을 수 없습니다."        // 사용자에게 보여줄 에러 메시지
            );
        }
        
        // 성공 시 상품 데이터와 함께 성공 응답 반환
        return ApiResponse.onSuccess(
            SuccessCode.ITEM_GET_SUCCESS,           // 상품 조회 성공 코드
            item                                    // 조회된 상품 데이터를 응답에 포함
        );
    }
    */
}
```

### 📝 Controller가 하지 말아야 할 것들

- ❌ 복잡한 비즈니스 로직 처리
- ❌ 데이터베이스 직접 접근
- ❌ 복잡한 데이터 변환 로직

---

## 🧠 Service 계층 (비즈니스 계층)

### 역할과 책임

- **비즈니스 로직 처리**: 핵심 업무 규칙과 로직 구현
- **트랜잭션 관리**: 데이터 일관성 보장
- **여러 Repository 조합**: 복잡한 데이터 조작
- **도메인 객체와 DTO 변환**: 계층 간 데이터 변환

### ItemService.java 분석

```java
package likelion13th.shop.service;

import jakarta.transaction.Transactional;
import likelion13th.shop.DTO.response.ItemResponseDto;
import likelion13th.shop.domain.Item;
import likelion13th.shop.repository.CategoryRepository;
import likelion13th.shop.repository.ItemRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

@Service
@RequiredArgsConstructor
public class ItemService {
    private final ItemRepository itemRepository;
    private final CategoryRepository categoryRepository;

    //개별 상품 조회
    /*@Transactional
    public ItemResponseDto getItemById(Long itemId) {
        Item item = itemRepository.findById(itemId).orElse(null);
        return item != null ? ItemResponseDto.from(item) : null;
    }*/
}
```

### 📝 Service에서 처리할 수 있는 비즈니스 로직 예시

- 상품 재고 확인 및 차감
- 할인 정책 적용
- 주문 가능 여부 검증
- 복잡한 계산 로직

---

## 💾 Repository 계층 (데이터 접근 계층)

### 역할과 책임

- **데이터 접근**: 데이터베이스 CRUD 연산
- **쿼리 실행**: 복잡한 검색 조건 처리
- **데이터 캐싱**: 성능 최적화

### ItemRepository.java 분석

```java
package likelion13th.shop.repository;

import likelion13th.shop.domain.Category;
import likelion13th.shop.domain.Item;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public interface ItemRepository extends JpaRepository<Item, Long> {
    //save, findById, findAll 같은 기본 기능 자동으로 제공됨
    //카테고리별 아이템 조회
    List<Item> findByCategories(Category category);
}
```

### 📝 Repository의 특징

- **인터페이스**: 구현체는 Spring Data JPA가 자동 생성
- **메서드 네이밍**: 메서드 이름으로 쿼리 자동 생성
- **@Query**: 복잡한 쿼리는 직접 작성 가능

---

## 📋 Domain 계층 (도메인 계층)

### 역할과 책임

- **도메인 모델**: 비즈니스 개념을 코드로 표현
- **데이터 구조 정의**: 엔티티의 속성과 관계 정의
- **비즈니스 규칙**: 도메인 객체 내부의 규칙 구현

### Item.java 분석

```java
package likelion13th.shop.domain;

import com.fasterxml.jackson.annotation.JsonIgnore;
import jakarta.persistence.*;
import likelion13th.shop.domain.entity.BaseEntity;
import lombok.*;

import java.util.ArrayList;
import java.util.List;

@Entity
@Getter
@Setter
@Table(name = "item")
@NoArgsConstructor
@AllArgsConstructor
public class Item extends BaseEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "item_id")
    @Setter(AccessLevel.PRIVATE)
    private Long id;

    @Column(nullable = false)
    private String item_name;

    @Column(nullable = false)
    private int price;

    @Column(nullable = false)
    private String imagePath;

    @Column(nullable = false)
    private String brand;

    @Column(nullable = false)
    @Setter
    private boolean isNew= false;

    //Category와 다대다 연관관계 설정
    @ManyToMany(mappedBy = "items")
    private List<Category> categories = new ArrayList<>();

    //Order과 일대다 연관관계 설정
    @OneToMany(mappedBy = "item", cascade = CascadeType.ALL)
    @JsonIgnore
    private List<Order> orders = new ArrayList<>();

    public Item(String item_name, int price, String thumbnail_img, String brand, boolean isNew) {
        this.item_name = item_name;
        this.price = price;
        this.imagePath = imagePath;
        this.brand = brand;
        this.isNew= false;
    }
}
```

### 📝 Domain 객체에서 할 수 있는 것들

- 도메인 규칙 검증 (예: 가격은 0보다 커야 함)
- 상태 변경 메서드 (예: 상품 할인 적용)
- 연관관계 편의 메서드

---

## 📦 DTO (Data Transfer Object)

### 역할과 책임

- **데이터 전송**: 계층 간 데이터 전송
- **데이터 캡슐화**: 필요한 데이터만 노출
- **API 스펙 정의**: 클라이언트와의 인터페이스 명세

### DTO의 종류

```java
// 요청 DTO
public class ItemRequestDto {
    private String itemName;
    private int price;
    private String brand;
    // validation 어노테이션 추가
}

// 응답 DTO
public class ItemResponseDto {
    private Long id;
    private String itemName;
    private int price;
    private String brand;
    private boolean isNew;

    // Entity -> DTO 변환 메서드
    public static ItemResponseDto from(Item item) {
        return ItemResponseDto.builder()
            .id(item.getId())
            .itemName(item.getItem_name())
            .price(item.getPrice())
            .brand(item.getBrand())
            .isNew(item.isNew())
            .build();
    }
}
```

---

## 🔧 Global 패키지

### 주요 구성 요소

- **api**: 공통 API 응답 형식
- **config**: 설정 클래스들
- **exception**: 예외 처리
- **constant**: 상수 정의

```java
package likelion13th.shop.global.api;

import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.annotation.JsonPropertyOrder;
import lombok.AccessLevel;
import lombok.AllArgsConstructor;
import lombok.Getter;

@Getter
@AllArgsConstructor(access = AccessLevel.PROTECTED)
@JsonPropertyOrder({"isSuccess", "code", "message", "result"})
public class ApiResponse<T> { // API 응답

    private final Boolean isSuccess; // 성공 여부
    private final String code; // 응답 코드
    private final String message; // 메시지
    @JsonInclude(JsonInclude.Include.NON_NULL)
    private final T result; // 응답 데이터

    //성공
    public static <T> ApiResponse<T> onSuccess(BaseCode code, T result) {
        return new ApiResponse<>(true, code.getReason().getCode(), code.getReason().getMessage(), result);
    }

    //실패
    public static <T> ApiResponse<T> onFailure(BaseCode code, T data) {
        return new ApiResponse<>(false, code.getReason().getCode(), code.getReason().getMessage(), data);
    }
}
```

---

## 📊 계층별 상호작용 정리

| 계층 | 역할 | 의존성 | 주요 어노테이션 |
| --- | --- | --- | --- |
| **Controller** | HTTP 요청/응답 처리 | Service | `@RestController`, `@RequestMapping` |
| **Service** | 비즈니스 로직 처리 | Repository | `@Service`, `@Transactional` |
| **Repository** | 데이터 접근 | Entity | `@Repository` |
| **Domain** | 도메인 모델 정의 | - | `@Entity`, `@Table` |
| **DTO** | 데이터 전송 | Domain | - |

---

## ✨ 핵심 포인트

**🎯 각 계층은 하나의 책임만 가져야 한다**

- Controller: HTTP 처리만
- Service: 비즈니스 로직만
- Repository: 데이터 접근만

**🎯 의존성 방향을 지켜야 한다**

- Controller → Service → Repository → Domain
- 역방향 의존성은 금지

**🎯 계층 간 데이터 전송은 DTO를 사용한다**

- Entity를 직접 반환하지 않기
- 필요한 데이터만 노출하기

---

## 🔍 더 알아보기

- Spring Boot 공식 가이드
- Clean Architecture 개념
- DDD (Domain Driven Design)
- SOLID 원칙과 계층 구조
