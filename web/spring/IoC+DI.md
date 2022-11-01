## Container
> Container란, 객체의 생성, 사용, 소멸에 해당하는 라이프사이클을 담당하며, 라이프사이클을 기본으로 애플리케이션 사용에 필요한 주요 기능을 제공한다.

### Container 기능
- 라이프사이클 관리
- Dependency 객체 제공
- Thread 관리
- 기타 애플리케이션 실행에 필요한 환경

### Container 필요성
- 비즈니스 로직 외에 부가적인 기능들에 대해서는 독립적으로 관리되도록 하기 위함
- 서비스 look up 이나 Configuration에 대한 일관성을 갖기 위함
- 서비스 객체를 사용하기 위해 각각 Factory 또는 Singleton 패턴을 직접 구현하지 않아도 됨

> cf) <b>Spring DI Container</b> <br>
> Spring DI Container가 관리하는 객체를 빈(Bean)이라 하고, 이 빈들의 생명주기(Life-Cycle)를 관리하는 의미로 빈팩토리(BeanFactory)라 한다.

# IoC (Inversion of Control)
> 프로그래머가 작성한 프로그램이 재사용 라이브러리의 흐름 제어를 받게 되는 소프트웨어 디자인 패턴

전통적인 프로그래밍에서는 필요한 위치에서 개발자가 필요한 객체 생성 로직을 구현한다.
제어 역행에서는 객체 생성을 Container에게 위임하여 처리한다.

전통적인 프로그래밍
```java
public class A {
    
    private B b;
    
    public A() {
        b = new B();
    }
}
```

제어 역행 프로그래밍
```java
public class A {
    @Autowired
    private B b;
}
```

## IoC 유형
<img src="https://user-images.githubusercontent.com/45252618/199264594-382051a3-cf5a-4782-965b-4c4c3213bda7.png">

### DL(Dependency Lookup)
- 컨테이너가 lookup context를 통해서 필요한 Resource나 Object를 얻는 방식
- Lookup한 Object를 필요한 타입으로 캐스팅 필요
- Naming Exception을 처리하기 위한 로직 필요
  
### DI(Dependency Injection)
- 각 클래스 간의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것 
- Object에 lookup 코드를 사용하지 않고 컨테이너가 직접 의존 구조를 Object에 설정할 수 있도록 지정해주는 방식 
- Lookup 관련된 코드들이 Object 내에서 사라짐
- Setter Injection 
- Constructor Injection 
- Method Injection

## IoC 특징
- 장점 : 객체 간의 결합도를 떨어뜨릴 수 있음
    - 결합도가 높을 경우 : 해당 클래스가 유지보수 될 때 그 클래스와 결합된 다른 클래스도 같이 유지보수 되어야 할 가능성이 높음

# DI (Dependency Injection)
> DI란, 각 클래스간의 의존 관계를 빈 설정 (Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것
- 개발자들은 빈 설정 파일에 의존관계가 필요하다는 정보를 추가하면 된다.
- 객체 레퍼런스를 컨테이너로부터 주입 받아서 실행 시에 동적으로 의존관계가 생성된다.
- 컨테이너가 흐름의 주체가 되어 애플리케이션 코드에 의존관계를 주입한다. 
- 느슨한 결합
  - 객체는 인터페이스에 의한 의존 관계만을 알고 있으며, 이 의존 관계는 구현 클래스에 대한 차이를 모르는 채 서로 다른 구현으로 대체 가능
- 장점 : 코드의 단순화. 결합도 제거

## 스프링 빈 설정
### XML
- XML 문서 형태로 빈의 설정 메타 정보 기술
- 단순하며 사용하기 쉬움
- `<bean>` 태그를 통해 세밀한 제어 가능

### Annotation
- 애플리케이션 규모가 커지고 빈의 개수가 많아질 경우 XML 파일로 관리하는 것이 번거로움
- 빈으로 사용될 클래스에 특별한 annotation을 부여해 주면 자동으로 빈 등록 가능
- 오브젝트 빈 스캐너로 빈 스캐닝을 통해 자동 등록
  - 빈 스캐너는 기본적으로 클래스 이름을 빈의 아이디로 사용
- component-scan 설정 필요
  - <context:component-scan base-package="com.test.hello.*"/>
- @Autowired, @Resource

참고 자료 : https://ko.wikipedia.org/wiki/제어_반전, https://dog-developers.tistory.com/12