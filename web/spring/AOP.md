# AOP (Aspect Oriented Programming)

> AOP(Aspect Oriented Programming)는 관점 지향 프로그래밍의 약자. 흩어진 Aspect를 모듈화할 수 있는 프로그래밍 기법. Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것.
OOP 한계를 극복시키기 위한 기법

<img src="https://user-images.githubusercontent.com/45252618/196095003-a4090bc9-e65d-4f3c-8a5a-db2c38dd2cfb.png">

- 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점을 나눠보고 그 관점을 기준으로 각각 모듈화 하겠다는 의미
  - 핵심적인 관점 : 개발자가 적용하고자 하는 핵심 비즈니스 로직
  - 부가적인 관점 : 핵심 로직을 수행하기 위해 필요한 DB 연결 (JDBC), 로깅, 파일 입출력 등  
- 인프라/부가기능을 모듈화한다.
- 기능을 비즈니스 로직과 공통 모듈로 구분하고, 개발자의 코드 밖에서 필요할 때 비즈니스 로직에 삽입하는 형식으로 실행된다.
- 공통적인 기능을 종단간으로 삽입할 수 있게 한다.
- Aspect 별로 처리한다.
  
### AOP의 취지 
- Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것


## cf) OOP (Object Oriented Programming)

> OOP(Object Oriented Programming)는 객체 지향 프로그래밍의 약자. 모든 데이터를 현실에 빗대어 객체로 다루는 프로그래밍 기법.
비즈니스 로직을 모듈화한다.

- 객체를 상속/위임을 통해 재사용하여 반복되는 코드의 양을 줄여주지만, 없앨 수 없는 상황이 생기고 어플리케이션 전체에서 사용되는 부가기능들을 모듈화하기 어렵다.
- 공통적인 기능을 각 객체의 횡단으로 입력한다.
- 객체별로 처리한다.


## AOP의 주요 개념
- Aspect: 흩어진 공통부분을 모듈화하는 것
- Target: Aspect를 적용하는 곳
- Advice: 실질적으로 어떤 일을 해야할 지에 대한 구현체
- JointPoint: Advice가 적용될 위치, 끼어들 수 있는 지점. 메서드 진입 지점, 생성자 호출 시점, 필드에서 값을 꺼내올 때 등 다양한 시점에 적용가능
- PointCut: Joint point 의 상세 스펙을 정의한 것


## 특징
- 프록시 기반의 AOP 구현체
- 스프링 빈에만 AOP 를 적용할 수 있다.
- 동적 프록시 빈을 만들어 등록시켜준다.

## 스프링 AOP 특징
- 프록시 패턴 기반의 AOP 구현체, 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위해서
- 스프링 빈에만 AOP를 적용 가능
- 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적

### 스프링 AOP : @AOP
스프링 @AOP를 사용하기 위해서는 다음과 같은 의존성을 추가해야 한다.
~~~
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
~~~