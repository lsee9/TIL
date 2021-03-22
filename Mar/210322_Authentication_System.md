###### 210322_mon

# 1. Authentication System

- Autentication
  - 인증
  - 신원확인
- Authorization
  - 권한, 허가
  - 원하는 정보를 얻도록 허용하는 과정

<br>

## 1.1 Django Authentication System

- 인증(authentication)과 권한(authorization)부여를 함께 제공
- 따라서 일반적으로 __authentication system__(인증 시스템) 이라고 함

### main point

- __User object__
  - 유저에 대한 인증
- __Web request__
  - 요청에 대한 인증

<br>

## 1.2 Project 생성

> 05_django_auth_se_practice 에서 수업 전반적인 내용 처음부터 진행
>
> static file까지 crud가 구현된 프로젝트에서 시작입니다!

- 가상환경

```shell
$ python -m venv venv
$ source venv/Scripts/activate
```

- requirements.txt 설치

```shell
$ pip install -r requirments.txt
```

- migrate
  - 프로젝트를 받아오면 db가 없으므로 만들어줍니다

```shell
$ python manage.py migrate
```

- 서버확인

```shell
$ python manage.py runserver
```

<br>

#### 관리자계정 생성

- 생성하기

```shell
$ python manage.py createsuperuser
```

- id/pw
  - admin

<br>

#### 유저관련 app 생성

- __accounts__를 생성합니다
  - 유저, 인증에 관련된 앱 이름은 이것을 권장합니다.

```shell
$ python manage.py atartapp accounts
```

- crud/settings.py - 앱등록

```python
INSTALLED_APPS = [
    'articles',
    'accounts',
    ...
]
```

- crud/urls.py

```python
urlpatterns = [
    ...
    path('accounts/', include('accounts.urls')),
]
```

- accounts/urls.py 생성

```python
from django.urls import path
from . import views

app_name = 'accounts'
urlpatterns = [
]
```

<br>

#### 유저 목록 출력하는 페이지

> /accounts/의 url주소를 가지며, 모든 유저 목록을 출력합니다

- accounts/urls.py

```python
urlpatterns = [
    path('', views.index, name=index),
]
```

- accounts/views.py - index

```python
from django.contrib.auth import get_user_model

def index(request):
    #user model instance
    User = get_user_model()
    #모든 정보 받아옴
    users = User.objects.all()
    context = {
        'users': users,
    }
    return render(request, 'accounts/index.html', context)
```

- /templates/accounts/index.html

```html
{% extends 'base.html' %}

{% block content %}
<h1><strong>User List</strong></h1>
<hr>
{% for myuser in users %}
  <h3>User Name: {{ myuser.username }}</h3>
  <p>First Name: {{ myuser.first_name }}</p>
  <p>Last Name: {{ myuser.last_name }}</p>
  <hr>
{% endfor %}
<a href="{% url 'articles:index' %}">[Main]</a>
{% endblock content %}
```

<br>

#### 회원가입하는 페이지

>  /accounts/signup/
>
> user name, password를 입력하면 회원가입
>
> 자동 로그인과 함께 /articles/로 돌아갑니다.

- accounts/urls.py

```python
urlpatterns = [
    ...
    path('signup/', views.signup, name='signup'),
]
```

- accounts/views.py - signup
  - Built-in form인 UserCreationForm을 사용합니다

```python
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import login as auth_login

def signup(request):
    #로그인했으면 articles:index로 요청보냄
    if request.user.is_authenticated:
        return redirect('articles:index')

    #POST인 경우(회원가입)
    if request.method == 'POST':
        #POST로 들어온 정보를 저장
        form = UserCreationForm(request.POST)
        #값이 올바른경우
        if form.is_valid():
            #저장함(해당 user 정보를 반환)
            user = form.save()
            #해당 user로 login
            auth_login(request, user)
            return redirect('articles:index')
    #GET인경우(signup버튼)
    else:
        #빈 form을 반환함
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

- templates/accounts/signup.html

```html
{% extends 'base.html' %}

{% block content %}
<h1><strong>Signup</strong></h1>
<hr>
<form action="" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit" value="submit">
</form>
<a href="{% url 'articles:index' %}">[Main]</a>
{% endblock content %}
```

<br>

#### 로그인하는 페이지

>  /accounts/login/
>
> 로그인이 끝나면 /articles/ 페이지로 이동합니다/

- accounts/urls.py

```python
urlpatterns = [
    ...
    path('login/', views.login, name='login'),
]
```

- accounts/views.py - login
  - Built-in 함수인 login을 사용합니다 (함수와 이름 겹치면 안되니까 auth_login으로 사용)
    - session을 만듦(sessionid를 session table에 기록)
    - sessionid로 쿠키에 심으라는 응답 마련

```python
def login(request):
    #로그인한 경우 articles:index로 요청 보냄
    if request.user.is_authenticated:
        return redirect('articles:index')

    #POST 요청인 경우(login)
    if request.method == 'POST':
        #form에 POST로 받아온 정보 저장
        form = AuthenticationForm(request, request.POST)
        #값이 올바른 경우
        if form.is_valid():
            #login을 함
            auth_login(request, form.get_user())
            return redirect('articles:index')
    #GET 요청인 경우(login page)
    else:
        #빈 form을 제공함
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/login.html', context)
```

- templates/accounts/login.html
  - signup과 동일

```html
{% extends 'base.html' %}

{% block content %}
<h1><strong>Login</strong></h1>
<hr>
<form action="" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit" value="submit">
</form>
<a href="{% url 'articles:index' %}">[Main]</a>
{% endblock content %}
```

<br>

#### 로그아웃

>  /accounts/logout/
>
> 로그아웃이 끝나면 /articles/ 페이지로 이동합니다/

- accounts/urls.py

```python
urlpatterns = [
    ...
    path('logout/', views.logout, name='logout'),
]
```

- accounts/views.py - logout
  - Built-in 함수인 logout을 사용합니다
  - cookies와 db를 모두 지워줍니다

```python
@require_POST  #POST 요청만 받도록
def logout(request):
    auth_logout(request)  #해당 user logout
    return redirect('articles:index')
```

<br>

#### 로그인과 비로그인 접근 제한

##### 속성(attribute) : is_authenticated

> 사용자가 인증되었는지 확인하는 방법

- 유저를 통해 만들어진 인스턴스인지 아닌지
  - 즉, 로그인된건지 아닌지만 판단

- User : True
- AnoymousUser : False

##### 테코레이터(decorator) : login_required

- 사용자가 로그인 했는지 확인하는 view를 위한 데코레이터
- 로그인하지 않은 사용자 settings.LOGIN_URL에 설정된 경로로 redirect
  - LOGIN_URL의 기본 값 : '/acounts/login/'





- accounts/urls.py

```python
urlpatterns = [
    ...
    path('login/', views.login, name='login'),
]
```

- accounts/views.py - login
  - Built-in 함수인 login을 사용합니다 (함수와 이름 겹치면 안되니까 auth_login으로 사용)
    - session을 만듦(sessionid를 session table에 기록)
    - sessionid로 쿠키에 심으라는 응답 마련

```python
def login(request):
    #로그인한 경우 articles:index로 요청 보냄
    if request.user.is_authenticated:
        return redirect('articles:index')

    #POST 요청인 경우(login)
    if request.method == 'POST':
        #form에 POST로 받아온 정보 저장
        form = AuthenticationForm(request, request.POST)
        #값이 올바른 경우
        if form.is_valid():
            #login을 함
            auth_login(request, form.get_user())
            return redirect('articles:index')
    #GET 요청인 경우(login page)
    else:
        #빈 form을 제공함
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/login.html', context)
```

- templates/accounts/login.html
  - signup과 동일

```html
{% extends 'base.html' %}

{% block content %}
<h1><strong>Login</strong></h1>
<hr>
<form action="" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit" value="submit">
</form>
<a href="{% url 'articles:index' %}">[Main]</a>
{% endblock content %}
```



