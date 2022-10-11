# MTV (Model-Template-View)
Django 에서 사용한 디자인 패턴 

![MTV](https://github.com/yejin013/TIL/blob/4a4bbf2da28ca2aa47a9a8a1616dc34e5588597c/img/MTV.png)

- Model
    - 애플리케이션이 '무엇'을 할 것인지 정의한다.
    - 데이터 저장 형태를 어떻게 할지 설정한다.
    - View에서 요청한 데이터 제공한다.
    - Django의 model 모듈을 이용해 models.py에 작성한 코드, DB
- Template
    - 사용자에게 무엇을 '화면 (UI)' 보여준다. 
    - MVC 패턴에서 View의 역할
    - Django에서 urls.py에서 template (html 등)과 연결을 한다.
- View
    - Model이 데이터를 '어떻게' 처리할지 알려주는 역할을 한다. 
    - Template에서 받은 request (요청)에 대해 처리하고 response (응답) 해주는 역할을 한다.
    - Model에게 데이터를 요청해서 받은 후 데이터를 가공하여 Template에게 전달한다.
    - MVC 패턴에서 Controller의 역할
    - Django에서 views.py에 class를 정의한다.

URLConf (URL 설계)
```
from django.urls import path
from . import views

app_name = ‘test’

url patterns = [
	path(‘’, views.HomeView.as_vies() , name=‘home’),
	path(‘login’, views.LoginView.as_view(), name=‘login’),
	path(‘login’, views.SignupView.as_view(), name=‘signup’),
]
```

## MTV - WEB에 동작하는 순서
1. 사용자가 웹사이트에 접속한다.
2. URLConf를 통해 해당 url과 매핑된 View를 호출한다.
3. View에서 지정한 로직을 수행하고, Model에게 CRUD를 지시한다.
4. Model은 Django에서 제공해주는 ORM을 통해, (또는 직접 SQL을 구현하여) Database에서 CRUD를 수행한다.
5. View는 Model에게서 처리된 값을 받고 지정된 template을 렌더링한다.
6. 최종 결과를 response로 반환한다.
