# 김영한님의 spring JPA 기본편 강의 정리
<br>
<hr>
<br>

## ✔️ ex1-hello-jpa directory
### 학습 범위 : 1-2-1 - 1-2-2
- 프로젝트 생성

- jpa 설정 : `persistence.xml`

- jpa curd
<br>
<br>

### 학습 범위 : 1-3-1 - 1-3-5
- 영속성 컨텍스트

- 변경감지 (dirty checking)

- Flush

- 준영속 상태
  - `em.detach(entity)`
  - `em.clear()`
  - `em.close()`
<br>

- Flush -> dirty checking -> 1차 캐시 확인 및 스냅샷 비교 -> commit
<br>
<br>

### 학습 범위 : 1-4-1 - 1-4-4
- 객체와 테이블 맵핑

- `hibernate.hbm2ddl.auto`
  - create
  - create-drop
  - update
  - none

- 필드와 컬럼 매핑 어노테이션

- 기본 키 매핑
  - `@Id`
  - `@GeneratedValue(strategy = GenerationType.IDENTITY, SEQUENCE, TABLE)`
<br>
<hr>
<br>

## ✔️ jpashop directory
### 학습 범위 : 1-4-5 - 1-5-4
- 요구사항 분석과 기본 매핑 실전 예제

- 단방향, 양방향, 연관관계 매핑
  - `@ManyToOne` : FK를 가지는 연관관계의 주인
  - `@OneToMany`
  - `mappedBy` : 연관관계의 주인이 아닌 곳에서 멤버 객체 필요 시 추가
<br>

- 기본적으로 연관관계 매핑 시 단방향`(@ManyToOne, @OneToOne)`으로 최대한 처리 후, 필요 시 양방향 매핑 진행


