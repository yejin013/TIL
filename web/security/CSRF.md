# CSRF Token
> CSRF Token이란, 서버에 들어온 요청이 실제 서버에서 허용한 요청이 맞는지 확인하기 위한 토큰

- 서버에서 뷰 페이지를 발행할 때 랜덤으로 생성된 Token을 같이 준 뒤 사용자 세션에 저장해둔다.
- 서버에 작업을 요청할 때 페이지에 Hidden으로 숨어있는 Token 값이 같이 서버로 전송된다.
- Token 값이 세션에 저장된 값과 일치하는지 확인하여 해당 요청이 위조된 것을 아니라는 것을 확인한다.
- 새로운 뷰 페이지가 발행할 때마다 새로 생성한다.

```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
```
