> REST란, "Representational State Transfer"라는 용어의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것.

# REST 인터페이스 원칙

1.  자원의 식별 : 요청 내에 기술된 개별 자원을 식별할 수 있어야 한다. ex. URI
2.  메시지를 통한 리소스 조작 : 클라이언트가 어떤 자원을 지칭하는 메시지와 특정 메타데이터만 가지고 있다면 이것으로 서버 상의 해당 자원을 변경하고 삭제할 수 있는 충분한 정보를 가지고 있는 것이다. 
3.  자기 서술적 메시지 : 각 메시지는 자신을 어떻게 처리해야 하는지에 대한 충분한 정보를 포함해야 한다. 어떤 파서를 이용해야 하는지에 대한 정보도 포함해야 한다. 메시지를 이해하기 위해 그 내용까지 살펴봐야 한다면, 자기 서술적 메시지라고 할 수 없다.
4.  애플리케이션의 상태에 대한 엔진으로서 하이퍼미디어 : 만약에 클라이언트가 관련된 리소스에 접근하기를 원한다면, 리턴되는 지시자에서 구별될 수 있어야 한다.

> REST 구성  
> \- 자원 Resource : URI  
> \- 행위 Verb : HTTP Method  
> \- 행위에 대한 표현 Representations : HTTP Message Pay Load

# REST 특징

-   Uniform (유니폼 인터페이스) : URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일
-   Stateless (무상태성) : 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않아, API 서버는 들어오는 요청만을 단순히 처리하면 된다.
-   Cacheable (캐시 가능) : HTTP가 가진 캐싱 기능 적용이 가능하다. ex. Last-Modified, E-Tag
-   Self-descriptiveness (자체 표현 구조) : REST 인터페이스의 원칙인 자기서술적 메시지
-   Client-Server 구조 : REST servce는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트 (세션, 로그인 정보) 등을 직접 관리하는 구조로 각각의 역할 확실히 구분되어 있다.
-   계층형 구조 : REST 서버는 다중 계층으로 구성될 수 있다. API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있으며, 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다. PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.
