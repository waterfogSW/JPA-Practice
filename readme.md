# Spring-boot & JPA 활용

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