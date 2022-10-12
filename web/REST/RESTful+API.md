# RESTful API : REST의 원리를 따르는 API
각 요청이 어떤 동작이나 위한 것인지를 그 요청의 모습 자체로 추론이 가능한 것
REST API의 설계 규칙으 올바르게 지킨 시스템을 RESTful하다 라고 하며, REST API를 사용했지만 RESTful 하지 못한 시스템이 있을 수 있다.

## RESTful API 작동 방식
1. 클라이언트가 서버에 요청 전송
2. 서버가 클라이언트르 인증하고 해다 요청으 수행할 수 있는 권한이 클라이언트에 있는지 확인
3. 서버가 요청을 수신하고 내부적으로 처리
4. 서버가 클라이언트에 응답 반환. (요청 성공 여부와 정보 포함)

## RESTful API 장점
- 확장성 : 클라이언트-서버 분리로, 효율적으로 크기 조정이 가능하다.
- 유연성 : 완전한 클라이언트-서버 분리 뿐 아니라 애플리케이션 함수르 계층화하여 유연성을 향상시킨다. 
- 독립성 : API 설계에 영향을 주지 않고 다양한 프로그래밍 언어로 클라이언트 및 서버 애플리케이션을 모두 작성할 수 있으며, 통신에 영향을 주지 않고 양쪽을 기본 기술을 변경할 수 있다.

## REST API 설계 규칙
1. URI는 명사를 사용한다. (동사 X)
```
GET /delete-user/1 (X)
DELETE /user/1 (O)
```

2. 슬래시로 계층 관계를 표현한다. (마지막에 슬래시를 포함하지 않는다.)
```
GET /furniture/sofa
POST /animals/mammals/rabbits
```

3. 하이폰을 사용한다.
```
GET /test_blog (X)
GET /test-blog (O)
```

4. 파일 확장자를 URI에 포함하지 않는다.
```
GET /photo.jpg (X)
GET /photo (O)
```

5. URI는 소문자로만 구성한다.
```
GET /testBlog (X)
GET /test-blog (O)
```