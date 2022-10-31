# 유닛테스트
> 유닛테스트란, 컴퓨터 프로그래밍에서 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차

## 유닛테스트 특징

- 모든 함수와 메서드에 대한 테스트 케이스를 작성하는 절차
- 목적 : 프로그램의 각 부분을 고립 시켜서 각각의 부분이 정확하게 동작하는지 확인하는 것
- 장점
    - 테스트 간 결합도가 늦아서 통합이 간단해진다.
    - 오류 발견
        - 개발 부분 별로 테스트를 진행하여 오류 발견이 수월하다.
    - 테스팅에 대한 시간과 비용을 절감할 수 있다.
    - 변경이 쉽다.
        - 작은 부분으로 나누어 테스트를 진행하면서 변경을 하고 그걸 테스트하기 쉽다.
    - 새로운 기능을 추가할 때 그 부분과 관련 부분만 테스트하면 된다.
    - 리팩토링 시 안정성을 확보할 수 있다.
- 단점
    - 진입 장벽이 높다.
    - 유닛테스트 작성으로 인해 개발 시간이 늘어날 수 있다.

# JUnit

- JUnit이란, 자바 프로그래밍 언어용 유닛 테스트 프레임워크

## JUnit 특징

- 단위 테스트 Framework
- 어노테이션으로 간결하게 지원한다.
- 결과는 SUCCESS (초록색), 실패는 FAIL (빨간색)으로 나타낸다.

## JUnit Pattern

given-when-then pattern
준비-실행-검증 

- given : 테스트를 위한 준비 단계. 테스트에 사용할 변수 및 입력값 설정, Mock 객체 정의  
- when : 테스트 진행/실행 
- then : 테스트 검증 단계. 예상한 결과값과 실행값이 같은지 확인 

## JUnit Test Annotation

- @Test: 테스트를 만드는 모듈이라는 선언
- @Before: 테스드 메서드 전에 실행
- @After: 테스트 메서드 후에 실행
- @Ignore: 무시
- @DisplayName: 테스트 클래스 또는 테스트 메서드의 사용자 정의 표시 이름을 정의
- @ExtendWith: 사용자 정의 확장명을 등록하는데 사용
- @Disable: 테스트 클래스 또는 메서드를 비활성화

## JUnit 단정문 

- assertArrayEquals(a, b) : 배열 a와 b가 일치함을 확인 
- assertEquals(a, b) : 객체 a와 b의 값이 같은지 확인
- assertSame(a, b) : 객체 a와 b가 같은 객체임을 확인 
- assertTrue(a) : a가 참인지 확인
- assertNotNull(a) : a 객체가 null이 아님을 확인 

build.gradle
```
dependencies {
    implementation 'junit:junit:4.13.1'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

예시 
kotlin 
```kotlin
class DeleteAdminTest : BaseControllerTest() {
    @Test
    @DisplayName("어드민 계정 삭제 테스트(성공)")
    fun deleteAdminSuccess() {
        // given & when
        val adminId = "Admin1"
        val test = mockMvc.delete("/v2/admin/$adminId") { // mockito 사용
            header("Authorization", "Bearer $adminAccessToken")
        }
        // then
        test.andExpect {
            status { isOk() }
            jsonPath("message") { value("$adminId 삭제") }
        }
    }
}
```

java
```java
class StudyRoomServiceTest {
    @BeforeEach
    void setUp() {
        user = createUser();
        studyRoom = createStudyRoom(user);
    }

    @Test
    void 스터디룸_생성_성공() {
        //given
        StudyRoomDto studyRoomDto = studyRoomMapper.toDto(studyRoom);

        //when
        given(studyRoomRepository.save(any(StudyRoom.class))).willReturn(studyRoom);
        given(userRepository.findById(any())).willReturn(Optional.ofNullable(studyRoom.getUser()));
        StudyRoomDto newStudyRoomDto = studyRoomService.createStudyRoom(user, studyRoomDto);

        //then
        for (String name : newStudyRoomDto.getHashtags()) {
            assertTrue(studyRoomDto.getHashtags().contains(name));
        }
        assertTrue(studyRoom.getName().contains(studyRoomDto.getCategory().getValue()));
    }
}
```