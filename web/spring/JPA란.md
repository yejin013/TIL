# JPA (Java Persistence API)
> JPA(Java Persistence API)란, 자바 ORM 기술에 대한 표준 명세로, JAVA에서 제공하는 API

- 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스의 모음
- JPA는 애플리케이션과 JDBC 사이에서 동작한다. JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.
- 데이터를 객체지향적으로 관리할 수 있기 때문에 개발자는 비즈니스 로직에 집중할 수 있고 객체지향 개발이 가능하다.
- 객체를 통해 쿼리를 작성할 수 있는 JPQL (Java Persistence Query Language)를 지원한다.
- DBMS에 대한 종속성이 줄어든다. DBMS가 변경된다 하더라도, 소스, 쿼리, 구현방법, 자료형 타입 등을 변경할 필요가 없기 때문에 프로그래머는 Object에만 집중하면 되고, DBMS를 교체하는 작업에도 비교적 적은 리스크와 시간이 소요된다.
- 가장 대중적인 것은 Hibernate
 

## cf) ORM (Object-Relational Mapping)

> ORM(Object-Relational Mapping)이란, 객체와 관계형 데이터베이스를 매핑하는 것.

- 객체는 객체대로 설계하고, 관계형 데이터베이스는 관계형 데이터베이스대로 설계하고 ORM이 연결한다.
- SQL 쿼리가 아니라 메서드로 데이터를 조작할 수 있다.
 

## JPA 사용 이유
### 생산성
- JPA를 자바 컬렉션에 객체를 저장하듯 JPA에게 저장할 객체를 전달한다.
- 간단하게 CRUD를 작성할 수 있다.
- DDL문을 자동으로 생성해주기 때문에 데이터베이스 설계 정심을 객체 설계 중심으로 변경할 수 있다.

### 유지보수
- 기존에는 필드 변경 시 모든 SQL을 수정해야 한다.
- 개발자가 작성해야 할 SQL과 JDBC API 코드를 JPA가 대신 처리해주기 때문에 유지보수 해야 하는 코드 수가 줄어든다.

### 패러다임의 불일치
- 연관된 객체를 사용하는 시점에 SQL을 전달할 수 있다.
- 같은 트랜잭션 내에서 조회할 때 동일성을 보장한다.
- 상속, 연관관계, 객체 그래프 탐색, 비교하기 등과 같은 패러다임 불일치를 해결한다.

### 성능
- 어플리케이션과 데이터베이스 사이의 성능 최적화 기능을 제공한다.
- 같은 트랜잭션 안에서는 같은 엔티티를 반환하기 때문에 데이터 베이스와의 통신 횟수를 줄일 수 있다.
- 트랜잭션을 commit 하기 전까지 메모리에 쌓고 한 번에 SQL을 전송한다.

### 데이터 접근 추상화와 벤더 독립성
- 데이터베이스 기술에 종속되지 않도록 한다.
- 데이터베이스를 변경하면 JPA에게 다른 데이터베이스를 사용한다고 알려주면 간단하게 변경이 가능하다.
