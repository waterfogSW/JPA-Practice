# Spring-boot & JPA 활용

![image](https://user-images.githubusercontent.com/28651727/150728923-086a8f67-160d-4266-9d27-22b18dc42ce2.png)

## 프로젝트 환경 설정

- spring-boot-starter-web
- spring-boot-starter-thymeleaf
  - server-side 뷰 렌더링
- spring-boot-starter-data-jpa
  - JPA
- org.projectlombok:lombok
  - 웹개발을 좀더 편하게 하기 위함(코드 단축)
- com.h2database:h2 
  - h2데이터베이스 클라이언트


> **Tips**  
> `implementation 'org.springframework.boot:spring-boot-devtools'`
> - devtools는 수정한 파일만 recompile하면 수정사항이 반영되도록 할 수 있다.


### 쿼리 파라미터 로그 남기기

외부 라이브러리 사용
```text
implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.6'
```

> 이러한 개발 라이브러리들은 성능 저하의 원인이 될 수 있기때문에 운영에 적용할때는 성능테스트를 하고 적용해야 한다.

## 도메인 분석 설계

- 회원가입
  - 회원 등록
  - 회원 조회
- 상품기능
  - 상품 등록
  - 상품 수정
  - 상품 조회
- 주문기능
  - 상품 주문
  - 주문 내역 조회
  - 주문 취소
- 기타 요구사항
  - 제고 관리
  - 종류 : 도서 음반 영화
  - 카테고리 구분
  - 주문시 배송정보 입력

## 엔티티 설계 주의점

**모든 연관 관계는 지연 로딩으로 설정!**
- 즉시로딩(`EAGER`)는 예측이 어렵고 어떤 SQL이 실행될지 추적하기 어렵다.
- 실무에서 모든 연관관계는 지연로딩(`LAZY`)으로 설정해야 한다.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, Fetch join또는 엔티티 그래프 기능을 사용한다.
- @XToOne(OneToOne, ManyToOne)관계는 기본이 즉시 로딩이므로 직접 지연로딩으로 설정해야 한다.

**컬렉션은 필드에서 초기화하자**
```text
private List<Order> orders = new ArrayList<>();
```
- `null`문제에서 안전하다.
- 임의의 메서드에서 컬렉션을 잘못 생성하면 하이버네이트 내부 메커니즘에 문제가 발생할 수 있다.
- 가급적이면 컬렉션을 변경하지 않는것이 좋다.

### 테이블 컬럼명 생성 전략

`SpringPhysicalNamingStrategy`

스프링 부트 신규 설정(엔티티(필드) -> 테이블(컬럼))
1. 카멜 케이스 -> 언더스코어 (memberPoint -> member_point)
2. .(점) -> _(언더스코어)
3. 대문자 -> 소문자

**적용 2단계**
1. 논리명 생성 
- `spring.jpa.hibernate.naming.implicit-strategy` : 테이블이나 컬럼명 명시하지 않을시 논리명 적용
2. 물리명 적용
- `spring.jpa.hibernate.naming.physical-strategy` : 모든 논리명에 적용됨, 실제 테이블에 적용
-  username -> usernm 등의 회사 룰로 바꿀 수 있다.