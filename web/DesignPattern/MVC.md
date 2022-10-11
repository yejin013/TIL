# MVC (Model-View-Controller)

![MVC process](https://github.com/yejin013/TIL/blob/6d8281bb0307e89541043a75a1d495a6df92c9d6/img/MVC-Process.svg)

- 사용자 인터페이스, 데이터 및 논리 제어를 구현하는데 사용되는 소프트웨어 디자인 패턴
    - 디자인 패턴이란, 각 모듈의 세분화된 역할이나 모듈들 간의 인터페이스와 같은 코드를 작성하는 수준의 세부적인 구현 방안을. 설계할 때 참조할 수 있는 전형적인 해결 방식 또는 예제
- Model
    - 애플리케이션이 '무엇'을 할 것인지 정의한다.
    - 내부 비즈니스 로직을 처리하기 위한 역할을 한다. 
    - 처리되는 알고리즘, CRUD, 데이터 등이 여기에 들어간다.
    - Spring) Service class, DAO class
- Controller 
    - Model과 View 사이에 있는 컴포넌트이다. 
    - Model이 데이터를 '어떻게' 처리할지 알려주는 역할을 한다. 
    - 사용자가 접근 한 URL에 따라서 사용자의 요청사항을 파악한 후에 그 요청에 맞는 데이터, 명령을 Model에 보냄으로써 모델의 상태를 변경할 수 있다. 
    - Controller가 관련된 View에 명령을 보냄으로써 Model의 표시 방법을 바꿀 수 있다.
    - Spring) Dispatcher Servlet
- View
    - 사용자에게 무엇을 '화면 (UI)' 보여준다. 
    - Controller로부터 받은 데이터를 Web Browser에 렌더링 되도록 한다.
    - 클라이언트 측 기술인 html/css/javascript들을 모아둔 컨테이너로, 사용자가 볼 결과물을 생성하기 위해 Model로부터 정보를 얻어 온다.
    - Spring) .html, .jsp

## MVC - WEB에 동작하는 순서

![MVC web process](https://github.com/yejin013/TIL/blob/6d8281bb0307e89541043a75a1d495a6df92c9d6/img/Router-MVC-DB.svg)

1. 사용자가 웹사이트에 접속한다. (Uses)
2. Controller는 사용자가 요청한 웹페이지를 서비스 하기 위해서 모델을 호출한다. (Manipulates)
3. Model은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후에 그 결과를 리턴한다.
4. Controller는 Model이 리턴한 결과를 View에 반영한다. (Updates)
5. 데이터가 반영된 VIew는 사용자에게 보여진다. (Sees)
* 사용자가 입력을 담당하는 View를 통해 요청을 보내면 해당 요청을 Controller가 받고, Controller는 Model을 통해 데이터를 가져오고, 해당 데이터를 바탕으로 출력을 담당하는 View를 제어해서 사용자에게 전달한다.

## MVC 장점

-   Model-View-Controller 분리되어서 작업되기 때문에 유지보수성이 높다.
-   애플리케이션의 확장성이 높다.
-   재사용이 가능하기 때문에 중복코딩 문제점이 사라진다.
-   보통 개발자들에게 친숙한 패턴이기 때문에 협업이 용이하다.
-   프론트와 백엔드가 분리되어 작업할 수 있다.

## MVC 단점

-   코드 일관성을 유지해야 한다.
-   Massive MVC가 되면 MVC의 장점이 사라질 수 있다.
- 프로젝트의 규모가 커질수록 컨트롤러가 비대화되고, 모델과 뷰의 의존성을 완벽히 분리할 수 없어 유지보수가 어려워진다.
