# 5. 논리적 모델링1 - 키

## 1. 데이터베이스 설계 3단계

1.  **개념적 모델링 (Conceptual Modeling)**: 비즈니스 요구사항을 ERD(Entity-Relationship Diagram)로 표현하여 데이터의 큰 그림을 그리는 단계.

2.  **논리적 모델링 (Logical Modeling)**: 개념적 모델을 관계형 데이터베이스 구조에 맞게 변환하는 단계.<br>
엔티티는 테이블로, 속성은 컬럼으로 변환하며 기본 키(Primary Key)와 외래 키(Foreign Key)를 정의. 이 단계는 특정 DBMS에 종속되지 않음.

3.  **물리적 모델링 (Physical Modeling)**: 논리적 모델을 실제 사용할 DBMS(예: MySQL)에 최적화하여 구현하는 단계. 데이터 타입, 인덱스 등을 결정.