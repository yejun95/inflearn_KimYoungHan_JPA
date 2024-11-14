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
<br>
<br>

### 학습 범위 : 1-6-1 - 1-7-3
- 다대일, 일대다, 일대일, 다대다

- 다대다는 실무에서 사용 금지 요망
  - 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계 표현 불가
  - 연결 테이블을 추가해서 일대다, 다대일 관계로 풀어내야함
  - 하지만 객체는 컬렉션을 사용해서 객체 2개로 다대다 관계 가능
  - `@JoinTable` 어노테이션으로 연결할 테이블 설정
  - 사용해야 하지 않는 이유는 연결 테이블이 설정하지 않은 컬럼들도 포함시켜 버릴 수가 있음
  - 다대다를 사용해야 한다면 연결 테이블을 엔티티로 만들어서 진행하는 것이 낫다.
<br>

- 다대일은 무조건 연관관계에서 주인이 된다. (mappedBy 속성이 없음)

- 다양한 연관관계 맵핑 예제

- 상속관계 매핑
  - 관계형 데이터베이스에서는 슈퍼타입-서브타입 개념으로 존재
  - `@Inheritancce(strategy = inheritanceType.xxx)` : JOINED, SINGLE_TABLE, TABLE_PER_CLASS(추천 X)
  - `@DiscriminatorColumn(name = "DTYPE")` : entity 명이 DTYPE 컬럼 값으로 들어감
  - `@DiscriminatorValue` : DTYPE에 들어갈 컬럼 값을 바꿀 수 있음 (상속받은 child entity에서 설정)
<br>

- `@MappedSuperclass` : 특정 엔티티에서 필요한 속성만 상속받아 사용
  - 공통 속성을 정의하는 엔티티에 넣는 어노테이션 (ex: 등록일, 수정일, 등록자 등)
  - 이를 필요한 엔티티에서 상속받아 사용한다.
  - 대신 해당 어노테이션이 붙은 엔티티는 조회, 검색이 불가능
  - 직접 생성해서 사용할 일이 없으므로 추상클래스로 선언 추천
<br>
<br>

### 학습 범위 : 1-8-1 - 
- 프록시
  - `em.getReference()` : 데이터베이스 조회를 미루는 가짜(프록시) 엔티티 객체 조회 -> db에 쿼리가 안나가고 조회
  - 실제로 값을 get으로 꺼내올 때 쿼리가 나감
  - 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 `em.getReference()`를 호출해도 실제 엔티티 반환
  - 프록시 강제 초기화 : `Hibernate.initialize(entity)` -> JPA 표준은 강제 호출 없음

- 즉시 로딩

- 지연 로딩
