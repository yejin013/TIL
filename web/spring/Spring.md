# Spring Framework
> 스프링 프레임워크 (Spring Framework)란, 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크. 엔터프라이즈급 애플리케이션을 개발하기 위한 모든 기능을 종합적으로 제공하는 경량화된 솔루션

## 스프링 프레임워크의 특징
### POJO(Plain Old Java Object) 방식
- POJO는 평범한 자바 객체를 의미한다. 
- 특정 환경이나 기술에 종속적이지 않은 객체지향 원리에 충실한 자바객체 
- Java EE의 EJB (Enterprise JavaBeans) 는 한가지 기능을 위해 불필요한 복잡한 로직이 과도하게 들어가는 단점이 있다. 이에 반발하여 나온 용어가 POJO이다. 
- 별도의 프레임워크 없이 Java EE를 사용할 때에 비해 특정 인터페이스를 직접 구현하거나 상속받을 필요가 없어 기존 라이브러리를 지원하기가 용이하고, 객체가 가볍다는 특징이 있다. 
- 객체지향 설계에 맞게 코드를 작성해야한다.
  
### 관점 지향 프로그래밍(Aspect Oriented Programming, AOP) 
- 로깅, 트랜잭션, 보안 등 핵심적인 비즈니스 로직과 관련은 없으나, 여러 모듈에서 공통적으로 사용하는 기능을 분리하여 관리하고, 실행 시에 서로 조합하여 사용할 수 있다. 
- 이에 따라 효율적인 유지보수와 재활용성이 극대화된다. 기존에 널리 사용되고 있는 강력한 관점 지향 프로그래밍 프레임워크인 AspectJ도 내부적으로 사용할 수 있으며, 스프링 자체적으로 지원하는 실행시(Runtime)에 조합하는 방식도 지원한다.

### 의존성 주입(Dependency Injection, DI)
- 프로그래밍에서 구성요소 간의 의존 관계가 소스코드 내부가 아닌 외부에서 설정을 통해 정의되는 방식 
- 코드 재사용을 높여 소스코드를 다양한 곳에 사용할 수 있으며 모듈간의 결합도도 낮출 수 있다. 
- 계층, 서비스 간에 의존성이 존재하는 경우 스프링 프레임워크가 서로 연결시켜준다. 
  - 의존 관계에 있는 객체를 직접 생성하거나 검색할 필요가 없다.
- 스프링은 설정 파일이나, 어노테이션을 통해서 객체 간의 의존 관계를 설정할 수 있다.

### 제어 역전(Inversion of Control, IoC)
- 제어의 흐름을 사용자가 컨트롤하지 않고 프레임워크(Spring)에게 맡기는 것 
- 전통적인 프로그래밍: 개발자가 작성한 프로그램이 외부 라이브러리의 코드를 호출해서 이용했다. 
- 제어 역전: 외부 라이브러리 코드가 개발자의 코드를 호출하게 된다. 
- 제어권이 프레임워크에게 있어 필요에 따라 스프링 프레임워크가 사용자의 코드를 호출한다.

### PSA(Portable Service Abstraction)
- 환경과 세부기술의 변경과 관계없이 일관된 방식으로 기술에 접근할 수 있게 해주는 설계 원칙
- 트랜잭션 추상화, OXM 추상화, 데이터 액세스틔 Exception 변환 기능 등 기술적인 복잡함은 추상화를 통해 Low Level의 기술 구현 부분과 기술을 사용하는 인터페이스 분리

### 생명주기 관리
- 스프링 프레임워크는 Java 객체의 생성, 소멸을 직접 관리하며 필요한 객체만 사용할 수 있다.

### 데이터 엑세스 프레임워크
- 데이터베이스에 접속하고 자료를 저장 및 읽어오기 위한 여러 가지 유명한 라이브러리
- JDBC, iBATIS(MyBatis), 하이버네이트 등에 대한 지원 기능을 제공하여 데이터베이스 프로그래밍을 쉽게 사용할 수 있다.

### 트랜잭션 관리 프레임워크
- 추상화된 트랜잭션 관리를 지원하며 XML 설정파일 등을 이용한 선언적인 방식 및 프로그래밍을 통한 방식을 모두 지원한다.

### 모델-뷰-컨트롤러 패턴
- DispatcherServlet이 Controller 역할을 담당하여 각종 요청을 적절한 서비스에 분산시켜주며 이를 각 서비스들이 처리를 하여 결과를 생성하고 그 결과는 다양한 형식의 View 서비스들로 화면에 표시될 수 있다.

### 배치 프레임워크
- 특정 시간대에 실행하거나 대용량의 자료를 처리하는데 쓰이는 일괄 처리(Batch Processing)을 지원하는 배치 프레임워크를 제공한다. 주로 Quartz 기반으로 동작한다.

## 스프링 프레임워크의 구조 

<img src="https://user-images.githubusercontent.com/45252618/196095446-1ee506df-8305-4fc2-acc2-4a36f81e79f6.png">

- Core: Spring Container를 의미한다. 여기서 핵심적인 것은 Bean Factory Container로, 제어 역전(IoC)과 의존성 주입(DI) 기능을 제공하여 객체 구성부터 의존성 처리까지 모든 일을 처리하는 역할을 한다.
- Context: context 정보들을 제공하는 설정 파일이다. JNDI, EJB, Validation, Scheduiling, Internaliztaion 등 엔터프라이즈 서비스들을 포함한다.
- DAO: JDBC 추상 계층을 제공하여, 코딩이나 예외처리 하는 부분을 간편화 시켜 일관된 방법으로 코드를 짤 수 있게 도와준다. 데이터가 담겨있는 VO(Value Object) 클래스를 이용해 사용한다.
- ORM: JPA, Hibernate와 같은 ORM이나 MyBatis 같은 데이터베이스 API 등과 통합할 수 있는 기능을 제공한다.
- AOP: 스프링 프레임워크에서 제공하는 AOP 패키지를 제공한다.
- Web: Spring Web MVC, Struts, WebWork 등 웹 어플리케이션 구현에 도움되는 기능을 제공한다.
- MVC: Model2 구조로 application을 만들 수 있게 한다. 전략 인터페이스를 통해 고급 구성 가능하며 JSP, Velocity, Tiles, iText 및 POI를 포함한 수많은 뷰 기술을 지원하고 있다.