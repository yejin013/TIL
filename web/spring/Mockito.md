# Mockito
> Mockito란, 개발자가 동작을 직접 제어할 수 있는 가짜(Mock) 객체를 지원하는 테스트 프레임워크

- Mockito를 활용하면 의존성 있는 객체들을 특정 테스트에 주입할 수 있어, 단위 테스트를 용이하게 한다.
- 데이터베이스나 외부 API를 테스트 할 때, Mock 객체를 만들어 사용하면 편리하게 테스트를 할 수 있다.

## Mockito 사용하기
### 0. 준비
스프링부트에는 Mockito가 기본적으로 `spring-boot-starter-test` 의존성이 포함되어 있다.

build.gradle
```
repositories { mavenCentral() }
dependencies { 
    testImplementation "org.mockito:mockito-core:3.+" 
    testImplementation 'org.mockito:mockito-junit-jupiter:3.11.2'
}
```

### 1. Mock 객체 만들기
Mock 객체 의존성 주입 어노테이션
- @Mock : Mock 객체를 만들어 반환해주는 어노테이션
  - ex) 사용할 Repository, Client 객체 등
- @Spy : Stub하지 않은 메소드들은 원본 메소드 그대로 사용하는 어노테이션
    - ex) PasswordEncoder나 jwtToken 관련 메소드를 정의할 때 사용한 적 있다.
- @InjectMocks : @Mock 또는 @Spy로 생성된 Mock 객체를 자동으로 주입시켜주는 어노테이션
    - ex) 사용할 Service 객체: @Mock 또는 @Spy로 생성한 Mock 객체를 Service 시 생성자에 자동 주입시켜 Service 객체를 사용할 수 있게 하는 용도

```java
@ExtendWith(MockitoExtension.class)
class AdminServiceBase {
    @Spy
    PasswordEncoder passwordEncoder = BCryptPasswordEncoder();

    @Mock
    AdminRepository adminRepository;

    @InjectMocks
    AdminService adminService;
}
```

### 2. Mock 객체 Stubbing (행동 정의)
> Test Stub이란, 테스트 호출 중 테스트 스텁은 테스트 중에 만들어진 호출에 대해 미리 준비된 답변을 제공하는 것. 어떤 값을 리턴할지 정의 

- Mock 객체의 기본 리턴 값
    - Optional 타입은 Optional.empty를 리턴하고, 나머지 타입은 Null을 리턴한다.
    - Primitive 타입은 기본 Primitive 값을 리턴한다.
    - Collection은 비어있는 Collection을 리턴한다.
    - void method는 예외를 던지지 않고 아무런 일이 발생하지 않는다.

- OngoingStubbing 메소드 : when에 넣은 메소드의 리턴값을 정의해주는 메소드
  - when({스터빙할 메소드}).{OngoingStubbing 메소드} 
  - mock 되기 전에 실제 메소드를 호출하고, 결과값이 기대값을 반환해야한다고 지정 
  - 실제 메소드를 호출하여 대상 메소드에 문제점이 있는지 파악 가능
  - thenReturn() : 스터빙한 메소드 호출 후 어떤 객체를 리턴할 건지 정의
  - thenThrow() : 스터빙한 메소드 호출 후 어떤 Exception을 Throws할 건지 정의

- Stubber 메소드 : when에 스터빙할 클래스를 넣고 그 후에 메소드 호출. 스터빙이 반드시 실행되어야 하는 경우 사용
  - {Stubber 메소드}.when({스터빙할 클래스}).{스터빙할 메소드} 
  - 메소드를 실제 호출하지 않으면서 리턴 값을 임의로 정의 가능
  - doReturn() : Mock 객체가 특정한 값 반환
  - doNothing() : Mock 객체가 아무것도 반환하지 않는 경우
  - doThrow() : Mock 객체가 예외를 발생시키는 경우
    
### 3. Mock 객체 검증하기 
verify() 메소드로 Mock 객체가 어떻게 사용됐는지 확인 가능
```java
verify(memberService, times(1)).create(study); // memberService에서 create(study) 메소드가 한 번 호출되었는지 확인 
verify(memberService, never()).validate(any()); // 특정 메소드가 호출되지 않았는지 확인 (validate라는 메소드 호출되었는지 확인)
verify(mock, timeout(100),times(1)).someMethod(); // 특정 메서드가 시간 이내에 호출 되었는지 확인
verifyNoInteractions(mock); // verify 이후 다른 인터랙션이 없었다는 것을 확인

// 특정 메서드가 어떤 순서로 호출 되었는지 확인
InOrder inOrder = inOrder(memberService);
inOrder.verify(memberService).notify(study);
inOrder.verify(memberService).notify(member);
```

참고자료 : https://site.mockito.org/, https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#2, https://en.wikipedia.org/wiki/Test_stub, https://scshim.tistory.com/439