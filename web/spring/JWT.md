# JWT (JSON Web Tokens)
> JWT란, 유저를 인증하고 식별하기 위한 토큰 기반 인증

- 토큰은 클라이언트에 저장하여 서버의 부담 줄일 수 있다. (세션은 서버에 저장)
- 토큰 자체에 사용자의 권한 정보나 서비스를 사용하기 위한 정보 포함
- JSON 데이터를 Base64 URL-safe Encode를 통해 인코딩하여 직렬화한 것 + 개인키를 통한 전자서명

## JWT 발급 프로세스
1. 클라이언트 사용자가 아이디, 패스워드를 통해 웹 서비스 인증
2. 서버에서 서명된 JWT를 생성하여 클라이언트에 응답으로 돌려주기 (accessToken, refreshToken)
3. 클라이언트가 서버에 데이터를 추가적으로 요구할 때 (accessToken 만료시) JWT(refreshToken)를 HTTP Header에 첨부
4. 서버에서 클라이언트로부터 온 JWT(refreshToken) 검증
5. 클라이언트에게 accessToken 재발급

## JWT 구조
- `.`을 기준으로 세 부분으로 구분

- Header : JWT에서 사용할 타입과 해시 알고리즘의 종류
- Payload : 서버에서 첨부한 사용자 권한 정보와 데이터
- Signature : Header, Payload를 Base64 URL-safe Encode를 한 이후 Header에 명시된 해시 함수를 적용하고, 개인키로 서명한 전자서명

## 특징
### 장점 
- 문자열로 구성되어 HTTP header, Body, URL 어디든 위치 가능
- 대부분의 주류 프로그래밍 언어에서 지원
- 토큰 자체가 모든 정보를 가지고 있음

### 단점
- 토큰에 정보가 담겨 토큰 크기가 큼
- 토큰 탈취 당할 시 사용자 정보 유출 가능성 

## LocalStorage에 저장할까? Cookie에 저장할까?
- LocalStorage
    - XSS 공격에 취약. JS를 통해 LocalStorage에 접근 가능
    - CSRF 공격 방어 가능. CSRF는 기본적으로 브라우저에서 보내주는 Cookie를 통해 공격
- Cookie
    - XSS 탈취 가능성 있음
        - HttpOnly 옵션의 경우 script에서 Cookie를 읽어올 수 없어 XSS 공격 ㅂ아어 가능
    - CSRF 취약
        - Cookie가 아닌 Header에 넣고 요청을 보내면 CSRF 공격 방어 가능. 하지만 HttpOnly 옵션 해제 필요 
    
참고자료 : https://pronist.dev/143, https://cjw-awdsd.tistory.com/48, 