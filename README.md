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

### 학습 범위 : 1-8-1 - 1-8-4
- 프록시
  - `em.getReference()` : 데이터베이스 조회를 미루는 가짜(프록시) 엔티티 객체 조회 -> db에 쿼리가 안나가고 조회
  - 실제로 값을 get으로 꺼내올 때 쿼리가 나감
  - 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 `em.getReference()`를 호출해도 실제 엔티티 반환
  - 프록시 강제 초기화 : `Hibernate.initialize(entity)` -> JPA 표준은 강제 호출 없음
<br>

- 지연 로딩 : `fetch = FetchType.LAZY`
  - proxy 통해 가져오기 때문에 필요한 엔티티를 조회 할 때 초기화가 됨
<br>

- 즉시 로딩 : `fetch = FetchType.EAGER`
  - 조인된 엔티티를 무조건 가져옴 (proxy 사용 x)
  - 실무에서는 가급적 지연 로딩만 사용해야 함
  - 예상하지 못한 SQL이 발생할 수도 있음
  - JPQL에서 N+1 문제를 일으킴 -> SQL로 변환이 된 쿼리가 실행되고, `EAGER` 확인 후 또 실행하기 때문 -> `fetch join` 을 통한 해결 방법 존재
  - 💡 `@ManyToOne`, `@OneToOne`은 기본이 즉시 로딩이기 때문에 LAZY 설정 필수
  - `@OneToMany`, `@ManyToMany`는 기본이 지연 로딩
<br>

- 영속성 전이 : CASCADE
  - `cascade = CascadeType.ALL) : parent 엔티티에 속한 child 엔티티를 자동으로 persist 해줌
  - child가 독립적인 parent에만 의존적일 때 사용
<br>

- 고아 객체 : 부모 엔티티와 연관관계가 끊어진 자식 엔티티
  - 고아 객체 제거 : `orphanRemoval = true `
  - child가 참조하는 부모가 하나일 때만 사용 (CascadeType.REMOVE 처럼 동작)
<br>

- 연관관계 관리 예제
<br>
<br>

### 학습 범위 : 1-9-1 - 1-9-6
- 기본값 타입 : 생명주기를 엔티티에 의존
  - 자바 기본 타입(int, double)
  - 래퍼 클래스(Integer, Long)
  - String
<br>

- 엠비디드 타입(복합 값 타입) : 새로운 값 타입을 직접 정의
  - 주로 기본 값 타입을 모아 만들어서 복합 값 타입이라고도 함
  - `@Embeddable` : 값 타입을 정의하는 곳에 표시
  - `@Embedded` : 값 타입을 사용하는 곳에 표시 
  - 기본 생성자 필수
  - 기본 값 타입과 동일하게 엔티티에 생명주기를 의존
  - 공유 참조로 인한 위험성 때문에 엔티티 안에서 동시 사용 불가능 -> deep copy로 사용 가능하기는 함
  - 불변 객체로 생성하여 Setter를 없애고 생성자로만 값을 설정하도록 설계
<br>

- 컬렉션 값 타입
  - 값 타입을 하나 이상 저장할 때 사용
  - `@ElementCollection`
  - `@CollectionTable`
<br>

- 값 타입 매핑 예제
<br>
<hr>
<br>

## ✔️ jpql directory
### 학습 범위 : 1-10-1 - 1-10-9
- 해당 directory는 학습 코드를 지우고 새로 적으면서 진행했기에 소스코드가 온전하지 않음.

- JPQL 소개 및 사용

- 기본 문법과 쿼리 API
  - `TypeQuery`, `query`
  - `getResultList`, `getSingleResult`
<br>

- 프로젝션 (select) : 영속성 컨텍스트로 관리됨
  - 엔티티 프로젝션 : `select m from Member m, Member.class`
  - 임베디드 타입 프로젝션 : `select m.address from Member m, Address.class`
  - 스칼라 타입 프로젝션 : `select m.age, m.username from Member m`
<br>

- 페이징
  - `setFirstResult` : 조회 시작 위치
  - `setMaxResults` : 조회할 데이터 수
<br>

- 조인
  - 내부조인 : `select m from Member m [INNER] JOIN m.team t`
  - 외부조인 : `select m from Member m LEFT [OUTER] JOIN m.team t`
  - 세타조인 : `select count(m) from Member m, Team t where m.username = t.name`
  - 연관 관계 없는 엔티티 외부 조인 : `select m, t from Member m LEFT JOIN Team t on m.username = t.name`
<br>

- 서브쿼리
  - JPA는 WHERE, HAVING 절에서만 서브 쿼리 사용 가능 -> 하이버네이트는 FROM에서도 가능
  - FROM절의 서브쿼리는 JPQL에서 불가능
  - 나이가 평균보다 많은 회원 : `select m from Member m where m.age > (select avg(m2.age) from Member m2`
  - 한 건이라도 주문한 고객 : `select m from Member m where (select count(o) from Order o where m = o.member) > 0`
  - [NOT] exists
  - ALL, ANY, SOME
  - [NOT] IN
<br>

- JPQL 타입 표현
  - 문자 : 싱글 코테이션
  - 숫자 : 10L, 10D, 10F
  - Boolean: TRUE, FALSE
  - ENUM: jpabook.MemberType.admin (패키지명 포함)
  - 엔티티 타입: TYPE(m) = Member (상속 관계에서 사용)
<br>

**조건식**
- 기본 CASE 식
```sql
select
  case when m.age <= 10 then '학생요금'
  case when m.age >= 60 then '경로요금'
       else '일반요금'
  end
from Member m
```
<br>

- 단순 CASE 식
```sql
select
  case t.name
    when '팀A' then '인센티브110%'
    when '팀B' then '인센티브120%'
    else '인센티브105%'
  end
from Team t
```
<br>

- clalesce: 하나씩 조회해서 null이 아니면 반환

- NULLIF: 두 값이 같으면 null 반환, 다르면 첫번째 값 반환
<br>

- JPQL 기본 함수
  - CONCAT, SUBSTRING, TRIM, LOWER, UPPER, LENGTH, LOCATE, ABS, SQRT, MOD, SIZE, INDEX
