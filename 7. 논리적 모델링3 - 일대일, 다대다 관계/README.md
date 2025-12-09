# 7. 논리적 모델링3 - 일대일, 다대다 관계

## 1. 일대일(1:1) 관계
- 한 테이블의 한 행이 다른 테이블의 한 행과만 매칭되는 관계.
- 실제 DB 설계에서는 **많이 사용되지 않음** → 대부분 하나의 테이블로 합쳐도 문제없음.
- 테이블 분리 이유:
  - 보안 강화(민감정보 별도 테이블 분리)
  - 성능(큰 텍스트 컬럼 분리)
  - 선택적 정보 관리
  - 책임 분리(주문 vs 배송 등)

## 2. 1:1 관계를 DB에서 강제하는 방법
- **FK + UNIQUE 제약조건** 조합으로만 보장 가능.
- 예: `member_detail.member_id` 에 `UNIQUE` 추가.

## 3. FK 위치에 따른 두 가지 설계 방식

### A. 보조 테이블에 FK (권장)
```
member (member_id PK)
member_detail (detail_id PK, member_id FK UNIQUE)
```
- 장점:
  - 확장성 우수 (1:1 → 1:N 전환 시 UNIQUE 제거만 하면 됨)
  - 선택적 관계(옵셔널) 표현 쉬움
  - 테이블 간 책임 분리 용이
- 단점:
  - 상세정보 존재 여부 확인 시 JOIN 필요

### B. 주 테이블에 FK
```
member (detail_id FK)
member_detail (detail_id PK)
```
- 장점: JOIN 없이 상세정보 존재 여부 확인 가능
- 단점:
  - 확장성 매우 낮음
  - NULL FK 증가
  - 구조 변경 비용 큼
  - 실무에서는 거의 사용되지 않음

## 4. 1:1 → 1:N 전환
- 보조 테이블 방식일 경우 UNIQUE 제거만으로 즉시 1:N 구조로 변경 가능.
- 요구사항 확장 시 테이블 구조 변경 최소화.

## 5. 다대다(M:N) 관계
- 한 행이 여러 행과 연결되고 그 반대도 가능한 관계.
- RDBMS에서는 **직접 표현 불가**:
  - FK는 단일 값만 저장 가능
  - PK 중복 불가
  - 컬럼 값은 원자값(Atomic)이어야 함

## 6. 연결 테이블(Junction Table)로 해결
M:N 관계는 반드시 1:N + N:1 구조로 해소.

예:
```
orders (1) — (N) order_item (N) — (1) product
```

연결 테이블 특징:
- 두 테이블의 PK를 각각 FK로 가짐
- (order_id, product_id) UNIQUE 구성 가능
- **대리키(PK) 추가하는 방식이 현대적 표준**

## 7. 관계에 존재하는 속성 — 연관 엔티티
많은 M:N 관계에는 **관계 자체의 속성**이 존재함.

예:
- 주문 수량(order_quantity)
- 주문 당시 가격(order_price)
- 좋아요 누른 시간(liked_at)
- 학생의 수강 성적(grade)

→ 이런 속성은 양쪽 엔티티 어디에도 속하지 않음  
→ 반드시 **연관 엔티티(Associative Entity)** 필요

연관 엔티티 명칭 예시:
- order_item
- order_detail
- enrollment
- user_role
- product_tag