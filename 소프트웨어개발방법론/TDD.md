**TDD**란, **Test Driven Development**의 약자로 ‘**테스트 주도 개발**’이라고 한다.

-   반복 테스트를 이용한 소프트웨어 방법론으로, 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현한다.
-   실패하는 테스트 코드 작성이 우선되어야 한다.
-   TDD를 이용한 개발 방법 순서  
    1.  테스트 케이스 작성
    2.  테스트 케이스를 통과하는 코드 작성
    3.  작성한 코드 리팩토링

[##_Image|kage@dedGPr/btrsxJRV53b/6KoDx2yQ7kdeKokhasSULk/img.png|CDM|1.3|{"originWidth":751,"originHeight":529,"style":"alignCenter","width":596,"height":428,"filename":"blob"}_##]

TDD 개발 방식의 장점

-   객체 지향적인 코드 개발 : TDD는 코드의 재사용성을 보장하기 때문에, TDD로 코드를 개발하게 되면 기능별로 모듈화가 이루어진다.
-   재설계 시간의 단축 : 개발 완성보다 테스트 코드를 먼저 작성하기 때문에 설계에 문제점이 있을 시 보다 빠르게 문제점을 파악하고 해결할 수 있다.
-   리팩토링(유지보수)의 용이성 : 단위 테스트를 기반으로 하기 때문에, 추후 문제가 생겼을 때 버그를 찾기 용이하다.
-   테스트 문서 대체 가능 : 테스팅을 자동화시킴과 동시에 더욱 정확한 테스트 근거를 산출해 정의서를 작성할 수 있다.

TDD 개발 방식의 단점

-   생산성 저하 : TDD 코드 작성도 개발 단계이기인데, 개발 시간이 늘어나게 되기 때문에 TDD를 하지 않는 경우도 있다.

대표적인 TDD Tool은 JUnit (Java 단위 테스트 프레임워크) 이다.

짧게 직접 작성했었던 JUnit을 활용한 TDD 코드는 아래에서 확인할 수 있다.

```
class DeleteUserTest : BaseControllerTest() {

    @Test
    @DisplayName("어드민 계정 삭제 테스트(실패) - 삭제하려는 어드민 계정 없음")
    fun deleteUserFailBecauseUserNotFound() {
        // given & when
        val userId = "User1"
        val test = mockMvc.delete("/v1/user/${userId + 1}") {
            header("Authorization", "Bearer $adminAccessToken")
        }
        // then
        test.andExpect {
            status { isNotFound() }
            jsonPath("error") { value("User-003") }
        }
    }
}
```

특히 장기적으로 운영되는 동아리와 같은 경우에는, 계속 부원들이 추가되고 나가고를 반복하기 때문에 유지보수성 용이 부분에서 TDD를 작성하는 것이 좋다고 생각한다.
