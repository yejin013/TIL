# SOP (Same Origin Policy)

- 다른 출처의 리소스를 사용하는 것에 제한하는 보안 방식
- URL의 Protocol, Host, Port를 통해 비교

다른 출처의 리소스가 필요하다면? → CORS

# CORS

> CORS (Cross-Origin Resources Sharing)는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제

- 프론트와 백엔드의 협업으로 발생하는 문제점

## CORS 접근제어 시나리오

### 단순 요청 (Simple Request)

- Preflight 요청 없이 바로 요청을 날리는 작업, 그 즉시 Cross-Origin인지 확인
- 조건
    - GET, POST, HEAD method 중 하나
    - Content-Type
        - application/x-www-form-urlencoded
        - multipart/form-data
        - text/plain
    - 헤더: Accept, Accept-Language, Content-Language, Content-Type만 허용

### 프리플라이트 요청 (Preflight Request)

- 사전 확인 작업
- OPTIONS 메서드를 통해 다른 도메인의 리소스에 요청이 가능한지 확인 작업
    - 다른 도메인의 리소스에 가도 되는지 확인
- 요청이 가능하다면 실제 요청 (Actual Request)을 보낸다.
    - 요청이 불가능하다면 실제 요청이 보내지지 않는다.
- 클라이언트와 서버가 Preflight request와 response를 먼저 주고 받는다.
    - Preflight Request에 들어가는 정보
        - Origin: 요청 출처
        - Access-Control-Request-Method: 실제 요청의 메서드
        - Access-Control-Request-Headers: 실제 요청의 추가 헤더
    - Preflight Response
        - Access-Control-Allow-Origin: 서버 측 허가 출처
        - Access-Control-Allow-Methods: 서버 측 허가 메서드
        - Access-Control-Allow-Headers: 서버 측 허가 헤더
        - Access-Control-Max-Age: Preflight 응답 캐시 기간
        - 응답 코드는 200대여야 한다.
        - 응답 바디는 비어있는 것이 좋다.
    

### 인증정보 포함 요청 (Credentialed Request)

- 인증 관련 헤더를 포함할 때 사용하는 요청 (ex. JWT)
- 클라이언트측에서도 credentials를 include로 해야한다.
- 서버측에서는 Access-Control-Allow-Credentials를 true로 해야하고, Access-Control-Allow-Origin:*를 하면 안 된다.

## preflight가 필요한 이유

> CORS를 모르는 서버를 위함

preflight 요청이 없으면 서버는 이미 일을 처리한 후에 Browser가 CORS 에러를 client에게 보낸다.

preflight 요청이 있으면 서버는 안전하게 지켜진다.

```java
@Configuration
public class CorsConfiguration implements WebMvcConfigurer {
	
	@Override
	public void addCorsMappings(CorsRegistry registry) {
		registry.addMapping("/api")
				.allowOrigins("http://localhost:8081");
	}
}
```

참고자료: [https://youtu.be/-2TgkKYmJt4](https://youtu.be/-2TgkKYmJt4)
